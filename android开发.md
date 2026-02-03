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







### 上海项目加载前端工程

加载用的文件路径：app/src/main/java/com.asiainfo.newmboss/feature/main/MainActivity

可加载打包后的静态文件，也可直接加载本地起来的前端工程地址

![image-20250417102242932](D:\typora-img\image-20250417102242932.png)





### 学习路线（AI推荐）

### 🌱 阶段 1：基础准备（1–2 周）

1. 安装开发环境
   - 下载 [Android Studio](https://developer.android.com/studio)
   - 熟悉项目结构：`build.gradle`、`AndroidManifest.xml`、`res/` 目录
2. 学习 Kotlin 基础
   - 变量、函数、类、空安全、扩展函数、Lambda
   - 推荐资源：
     - [Kotlin 官方教程](https://kotlinlang.org/docs/home.html)
     - B站/YouTube 搜索 “Kotlin 零基础入门”
3. 理解 Android 四大组件
   - Activity（页面）、Service（后台）、BroadcastReceiver（广播）、ContentProvider（数据共享）

### 🧱 阶段 2：核心开发技能（3–6 周）

1. UI 开发
   - 布局：`ConstraintLayout`（重点）、`LinearLayout`
   - 常用控件：`TextView`, `Button`, `RecyclerView`（列表核心！）
   - 使用 XML 写界面（类似写 HTML + CSS）
2. 页面跳转与数据传递
   - `Intent` 机制
3. 异步与网络
   - 学习 **Kotlin 协程**（替代回调地狱）
   - 使用 **Retrofit + kotlinx.serialization** 请求 API
4. 数据持久化
   - `SharedPreferences`（轻量存储）
   - **Room 数据库**（官方推荐，类似 SQLite 封装）

### 🏗️ 阶段 3：现代架构与进阶（4–8 周）

1. MVVM 架构
   - `ViewModel` + `LiveData` / `StateFlow` 管理 UI 状态
2. Jetpack 组件
   - Navigation（页面导航）
   - Hilt（依赖注入）
   - WorkManager（后台任务）
3. 测试基础
   - 单元测试（ViewModel）
   - UI 测试（Espresso）

### 🚀 阶段 4：实战与发布

- 做 2–3 个完整项目

  ，例如：

  - 天气 App（调用公开 API）
  - 新闻阅读器（RecyclerView + 网络 + 缓存）
  - 待办事项（Room + MVVM）

- 学习 **Google Play 发布流程**

- 了解 **性能优化**（内存、帧率、启动速度）

------

## 三、优质学习资料推荐

### 📚 官方资源（免费且权威）

- Android Developers 官网
  - **Android Basics in Kotlin**（零基础课程）
  - **Modern Android Development (MAD)** 路线图
  - **Codelabs**：手把手实战教程（强烈推荐！）

### 🎥 视频课程

- B站：
  - “【2025 最新】Android 开发从入门到精通”（搜索关键词）
  - “Kotlin + Jetpack 全套教程”
- YouTube：
  - **Philipp Lackner**（英文，但清晰易懂）
  - **Coding in Flow**