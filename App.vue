<script>	
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			const app = getApp()
			app && app.globalData.nim && app.globalData.nim.offlinePush.updateAppBackground({
				isBackground: false
			})
			app.globalData.appHide = false
			console.log('App Show')
		},
		onHide: function() {
			const app = getApp()
			app && app.globalData.nim && app.globalData.nim.offlinePush.updateAppBackground({
				isBackground: true
			})
			app.globalData.appHide = true
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
