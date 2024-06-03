<template>
	<view class="content">
		<view class="section">
			<view class="left">账号名称: </view>
			<view class="right">
				<input class="input" v-model="account"></input>
			</view>
		</view>
		<view class="section">
			<view class="left">appkey: </view>
			<view class="right">
				<input class="input" v-model="appkey"></input>
			</view>
		</view>
		<view class="section">
			<view class="left">使用p8证书: </view>
			<view class="right">
				<switch :checked="usingP8Certificate" @change="changeApnsCertificate" />
			</view>
		</view>
		<view class="section">
			<view class="left">当前状态: </view>
			<view class="right">{{ connState }}</view>
		</view>
		<view class="section">
			<view class="center">
				<button size="mini" class="btn connect" type="default" @click="connect">连接</button>
				<button size="mini" class="btn destroy" type="default" @click="destroy">销毁</button>
				<button size="mini" class="btn destroy" type="default" @click="clearLog">清空日志</button>
			</view>
		</view>
		<scroll-view :scroll-y="true" :scroll-top="scrollTop" class="log-wrap">
			<rich-text class="log" :nodes="logArr"></rich-text>
		</scroll-view>
	</view>
</template>

<script>
const { log, error } = console
const NIMSDK = require('nim-web-sdk-ng/dist/NIM_UNIAPP_SDK')
const { pushParam, imLoginParam } = require('./config')

export default {
	data() {
		return {
			account: 'test',
			appkey: imLoginParam.appkey,
			connState: 'disconnect',
			logArr: [],
			scrollTop: 0,
			scrollTimeout: 0,
			scrollFlag: true,
			usingP8Certificate: true
		}
	},
	mounted() {
		console.log("!!!!!! mounted!!!!!!")
		const that = this;
		console.log = function () {
			log.apply(console, Array.prototype.slice.call(arguments))
			that.refreshLog('grey', arguments[0])
			// that.scrollToBottom()
		}.bind(console)
		console.error = function () {
			error.apply(console, Array.prototype.slice.call(arguments))
			that.refreshLog('red', arguments[0])
			// that.scrollToBottom()
		}.bind(console)
	},
	onUnload() {
		clearInterval(this.scrollTimeout);
		console.log = log.bind(console);
		console.error = error.bind(console);
	},
	methods: {
		connect() {
			const loginStr = {
				token: imLoginParam.token,
				debugLevel: 'debug',
				lbsUrls: imLoginParam.lbsUrls
			}
			const syncStr = {
				roamingMsgs: true,
			}
			const app = getApp()
			app.globalData.nim = new NIMSDK({
				appkey: this.appkey,
				...loginStr,
				account: this.account,
				logger: console
			}, syncStr);
			const eventList = [
				// 登陆完成事件
				'logined', 'disconnect', 'willReconnect', 'pushStage1', 'pushStage2', 'pushStage3', 'pushStage4', 'pushStage5', 'pushStage6'
			]
			eventList.forEach(key => {
				app.globalData.nim.on(key, (res) => {
					console.log(`收到 ${key} 事件：`, res ? JSON.parse(JSON.stringify(res)) : res);
					if (key === 'logined') {
						this.connState = 'logined'
					} else if (key === 'disconnect') {
						this.connState = 'disconnect'
					} else if (key === 'willReconnect') {
						this.connState = 'willReconnect'
					}
				});
			})

			const nimPushPlugin = uni.requireNativePlugin('NIMUniPlugin-PluginModule')
			app.globalData.nim.offlinePush.setOfflinePushConfig({
				plugin: nimPushPlugin,
				authConfig: pushParam
			})

			this.connState = 'connecting'
			app.globalData.nim.connect()
		},
		destroy() {
			const app = getApp()
			if (app.globalData.nim) {
				app.globalData.nim.destroy()
				app.globalData.nim = null
			}
		},
		refreshLog: function (type, content) {
			this.logArr.push({
				name: 'li',
				attrs: { style: `word-break: break-all; font-size: 13px; margin-bottom: 8px; color: ${type}` },
				children: [{
					type: 'text',
					text: content
				}]
			})
			if (this.logArr.length > 1000) {
				this.logArr = this.logArr.slice(-1000)
			}
		},
		changeApnsCertificate: function (event) {
			if (event) {
				this.usingP8Certificate = event.detail.value
			} else {
				console.error('Cannot change apns certificate, event is empty')
			}
		},
		clearLog: function () {
			this.logArr = []
		}
	}
}
</script>

<style>
@import url("./index.css");
</style>
