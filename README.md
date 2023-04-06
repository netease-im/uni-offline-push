UniApp推送Demo。

## 准备工作
1. 在各大手机厂商开发平台上，创建安卓应用
2. 在云信业务后台配置推送证书。

## 下载仓库及依赖
1. git clone git@github.com:netease-im/uni-offline-push.git
2. cd uni-offline-push
3. npm install

## 设置HBuilderX
1. 打开HBuilderX，导入该仓库
2. 点击 manifest.json，基础配置，选择 重新获取 uni-app 应用标识
3. 点击 manifest.json，选择 App 原生插件配置，选择本地插件，导入 NIMUniPlugin

## 配置推送参数
1. 点击 manifest.json，选择 App 原生插件配置，设置 VIVO 推送参数
2. 点击 pages/index/config.js，设置其他厂商的推送参数。certificateName为云信业务后台配置的推送证书名称
3. 在 nativeplugins/android/assets 中，下载华为的推送配置文件，并替换该仓库的 agconnect-services.json 文件

## 制作自定义基座
1. 在 HbuilderX 中打开当前项目，选择 运行 -> 运行到手机或模拟器 -> 制作自定义调试基座
2. 在弹窗中填入对应信息。注意，Android包名，和iOS bundleId应该和云信业务后台，以及各大厂商中注册的保持一致
3. 等待自定义基座制作完成

## 配置 IM 参数
1. 点击 pages/index/config.js，设置IM参数。应用默认的账号名为 test。该参数可以在Demo的应用界面中手动修改

## 运行应用
1. 在 HbuilderX 中打开当前项目，选择 运行 -> 运行到手机或模拟器 -> 运行到Android/iOS App基座
2. 选择自定义基座
3. 选择连接的手机
4. 运行应用

## 连接IM & 接收推送
1. 打开应用后，设置账户名称
2. 点击连接
3. 连接后，杀死应用
4. 用另一个设备给刚刚登录的账号发送信息（带有推送内容），刚刚的设备应该收到一条离线推送提醒
