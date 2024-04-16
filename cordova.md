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