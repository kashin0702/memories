### 简介

cordova是一个移动应用开发框架

本质是在html/css/js外包装一个原生壳， 通过webView渲染页面，通过cordova-plugin调用原生API，即hybrid app

cordova提供一系列设备相关API, **通过这些API，移动应用能够以JS访问原生的设备**，如摄像头，麦克风等。





### 环境准备

1.node

2.cordova CLI 

```txt
npm install cordova -g
```

3.java SDK

4.android SDK (建议使用android studio安装)



### 创建项目

```txt
cordova help   -- 命令查看

// 创建项目 生成cordova工程
cordova create <path> <包名> <项目名> -- path为路径名必填，包名和项目名可省略

// 添加运行平台 (添加安卓平台, 依赖安卓sdk)
cordova platform add android  -- 添加成功后 platform文件夹内会多一个andoroid文件夹，内部就是android的源代码

// 编译 执行一次， 之后更新前端代码执行cordova run android即可，会自动被放到android的www内进行解析编译
cordova build  --本质是生成了android/app/outputs/apk/debug/app-debug.apk 这个apk包

// 将apk安装到安卓手机上运行(提前连接手机到电脑上，打开调试模式，或者用android studio模拟器)
cordova run android
```



### 项目结构

![image-20240412165211940](D:\typora-img\image-20240412165211940.png)

hooks,platforms,plugins都是原生环境相关内容，前端代码在www中

![image-20240412165538287](D:\typora-img\image-20240412165538287.png)

添加android platform成功后 ,platforms文件夹内会多一个andoroid文件夹，内部就是android的工程代码



### 运行原理

**真正运行时，www代码会被拷贝到platforms/android/app/src/assets这个资源文件中进行编译运行**

**cordova build 本质是生成一个debug-apk包， 在android/app/outputs/apk/debug/app-debug.apk**

**cordova run android 就是将apk包放到手机上运行**



### 使用插件

```txt
// cordova项目内执行命令，安装相机插件 会添加一个全局的navigator.camera对象
cordova plugin add cordova-plugin-camera
```

```js
// 必须添加deviceready事件，等待设备加载完成
document.addEventListener('deviceready', function () {
    const btn = document.getElementById('btn')
    // 点击按钮，进行拍照
    btn.addEventListener('click', function () {
        takePhoto()
    })
    
    function takePhoto() {
        navigator.camera.getPicture(success, fail, options) // 接收一个成功和一个失败回调函数, options配置项，拍照或读取相册等设置
    }
    
    function success(fileUrl) { // 返回图片url
        console.log('拍照成功')
    }
    function fail() {
        console.log('拍照失败')
    }
})
```





### 项目打包(nx-ionic)

前置条件： 各个项目对版本要求不同

1.安装jdk-11

2.安装android studio (安装sdk tools 30.0.3)

3.配置gradle-wrapper.properties:  经常下载失败，配置成国内镜像地址

​	**distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-7.1.1-all.zip**

4.配置项目的jdk版本，根目录右键open module settings 设置jdk版本



打包：

1. ionic前端工程中执行**ionic cordova platform add android** 创建android项目，把新生成的android文件夹导入到android studio进行apk打包
2. Android studio  右上角**sync project with gradle files**进行构建(此处注意修改gradle下载地址和版本)
3. 生成构建后的android项目，左上角Build- **generate signed bundle /APK**

![image-20240415115633461](D:\typora-img\image-20240415115633461.png)

4.导入签名文件，签名文件在前端根目录XXX.keystore， 签名和密码在release-sigining.properties





## 安装插件

## 一、确认 SDK 类型

### 1. **JavaScript SDK**

- 如果 SDK 是纯 JS 文件（如 .js 文件或 npm 包），可以直接在 Web 层使用。
- 适用于不需要访问原生功能（如摄像头、蓝牙等）的场景。

### 2. **原生 SDK（Android/iOS）**

- 通常以 `.aar` / `.jar`（Android）或 `.framework` / `.xcframework`（iOS）形式提供。
- 必须通过 **Cordova 插件** 封装后才能在 Cordova 项目中调用。

------

## 二、集成方式

### ✅ 情况 A：SDK 已封装为 Cordova 插件（推荐）

如果供应商提供了 Cordova 插件（通常包含 `plugin.xml` 文件），可直接安装：

```
1# 本地路径安装（假设插件在 ./my-sdk-plugin 目录）
2cordova plugin add ./my-sdk-plugin
3
4# 或从 Git 安装
5cordova plugin add https://github.com/xxx/my-sdk-plugin.git
6
7# 或从 npm 安装（如果已发布）
8cordova plugin add cordova-plugin-my-sdk
```

然后在 JS 中调用：

```
1// 示例：调用插件方法
2window.mySdkPlugin.doSomething(
3  successCallback,
4  errorCallback,
5  params
6);
```

> 💡 注意：确保插件已正确声明 JS 接口（通常在 `www/` 目录下有 .js 文件，并在 `plugin.xml` 中 `<js-module>` 引用）。

## 三、验证插件是否添加成功

- 查看 `config.xml` 文件，会自动添加 `<plugin>` 节点。
- 查看 `plugins/` 目录下是否包含该插件文件夹。
- 查看 `package.json` 中的 `cordova.plugins` 字段。

------

## 四、在代码中使用插件

大多数插件会挂载到全局对象（如 `navigator` 或 `window`）上。例如：

```
1// 使用相机插件
2navigator.camera.getPicture(onSuccess, onFail, {
3    quality: 50,
4    destinationType: Camera.DestinationType.FILE_URI
5});
6
7function onSuccess(imageURI) {
8    console.log('Image URI: ' + imageURI);
9}
10
11function onFail(message) {
12    alert('Failed because: ' + message);
13}
```