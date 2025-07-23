UniApp推送Demo

## 安装依赖

npm install

## 启动项目

注意去 pages/index/index.vue 修改成自己的登录信息和推送配置

```
const YOUR_ACCOUNT = 'cs3'
const APPKEY = 'fe41664*********1847ad2547'
const STATIC_TOKEN = 'e10adc394*******e057f20f883e'
const OFFLINE_PUSH_CONFIG = {
  miPush: {
    appId: '2882*****40056',
    appKey: '518*****056',
    certificateName: '***_MI_PUSH'
  },
  vivoPush: {
    certificateName: '***_VIVO_PUSH'
  },
  oppoPush: {
    appId: '34**155',
    appKey: '6clw0*****488o0os',
    secret: 'e163705*******C440A94673',
    certificateName: '****_OPPO_PUSH'
  },
  hwPush: {
    appId: '104***65',
    certificateName: '****_HW_PUSH'
  },
  fcmPush: {
    certificateName: '****_FCM_V0'
  },
  mzPush: {
    appId: '11***10',
    appKey: '282bd********bbf9d2369',
    certificateName: '****_MZ_PUSH'
  },
  honorPush: {
    certificateName: '****_HONOR_PUSH'
  },
  apns: {
    certificateName: 'NIM****_DEV'
  }
}
```

“运行” -> "运行到 Android 基座" -> “使用自定义基座”


## 注意事项

1. 截止 25/07/23 日前推送插件版本最新为 1.1.3, 可以检查云信官网看看有没有更新的 [实现 uni-app 离线推送](https://doc.yunxin.163.com/messaging2/guide/zc4MTg5MDY?platform=client#%E7%AC%AC%E4%BA%8C%E6%AD%A5%E8%AE%BE%E7%BD%AE-uni-app-%E5%B7%A5%E7%A8%8B)
2. 本工程使用前需要制作一份自定义基座. 打基座的过程中会因为 uniapp 应用标识时 __UNI__B380AC4, 和开发者本人没有关联而失败, 所以建议开发者只是搬运这个工程的代码到新工程后后去打基座.
3. 本工程已经引入了 nativeplugins, 并且在 manifest.json 里开启了推送配置, 请开发者参照
4. 重点参照 App.vue 的推送拉起事件 addOpenNotificationListener 监听, 和 pages/index/index.vue 中的注入插件 setOfflinePushConfig 函数, 和 sendMessage 发送消息逻辑
5. 工程里的具体的 IM 账号, 证书都需要开发者替换.

此外推送还有其他注意事项

4. 华为注册厂商推送服务时，包含 agconnect-services.json 文件。您需要下载该文件，并将其放至您的 uni-app 应用根目录下的 nativeplugins/NIMUniPlugin/android/assets 文件夹下。
5. 荣耀注册厂商推送服务时，包含 mcs-services.json 文件。您需下载该文件，并将其放至您的 uni-app 应用根目录下的 nativeplugins/NIMUniPlugin/android/assets 文件夹下。