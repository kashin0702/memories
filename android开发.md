### android studio安装

### 文件结构

布局文件：

app--> src --> main --> res --> layout --> activity_main.xml

**AndroidManifest.xml** : app运行配置文件，即清单文件

**res**:  布局文件，图标，图片文件，图形描述文件，values常量文件



**gradle**: 项目自动化构建工具， 做了依赖，打包，部署，发布，各种取到差异化管理工作。

**gradle scripts**: 工程编译配置文件

![image-20250331173544636](D:\typora-img\image-20250331173544636.png)



### 什么是Activity

activity是一个应用程序组件或者叫页面，提供一个屏幕，用来提供交互完成某项任务

在AndroidManifest.xml中可以看到activity配置， 配置的是MainActivity，即用户第一个看到的主界面

```xml
<activity
    android:name=".feature.main.MainActivity"
    android:configChanges="orientation|screenSize|keyboardHidden"
    android:exported="false"
    android:hardwareAccelerated="true"
    android:launchMode="singleTask"
    android:screenOrientation="portrait"
    tools:ignore="DiscouragedApi,LockedOrientationActivity" />
```





### 设计规范（工程目录中的xml和java）

xml描绘用户界面，java编写程序逻辑和交互





### 布局文件

每个布局文件都有一个根节点

```xml
//线性布局
<LinearLayout></LinearLayout>
```



### 生命周期

```kotlin
class MainActivity : BaseActivity(), IJavascriptInterface {
   	lateinit var webView: DWebView
    override fun onCreate(savedInstanceState: Bundle?) {
         // 生命周期onCreate
        super.onCreate(savedInstanceState)
        webView = DWebView(this)
        // 显示指定的布局
        setContentView(webView)
    }
}
```



### 设置文本

两种方式

1. 在xml文件中通过属性android:text 设置文本
2. 在java文件中调用文本视图对象的setText方法设置文本