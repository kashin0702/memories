### 了解Less

#### 1. 什么是Less

Less 是一门CSS预处理语言,它扩充了CSS语言,增加了诸如变量,混合(mixin),函数等功能,让CSS更易维护,提高开发效率

预处理(不能直接在浏览器运行,需要编译成CSS文件才能在浏览器运行)



#### 2. 安装Less

在 服务器端最容易的安装方式就是通过npm(node.js的包管理器)安装

```shell
$ npm install -g less
```



##### 2.1 验证less有没有安装成功

```shell
$ lessc -v
```

如果打印出了版本号就是安装成功



#### 3.  编译

通过命令行编译less文件,注意: 在编译之前,一定要有一个less文件



##### 3.1  命令行方式编译

###### 1.  未指定编译路径

```shell
$ lessc index.less
```

编译完成以后,会在控制台打印编译结果,不会生成任何文件



###### 2 指定编译路径

```shell
$ lessc index.less index.css
```

或者

```shell
$ lessc index.less > index.css
```

这样会在编译成功后,生成新的index.css文件,编译的结果不会打印在控制台



###### 3.  编译后将编译结果的css文件进行压缩

需要借助less-plugin-clean-css插件

先安装 插件

```shell
$ npm install less-plugin-clean-css -g
```



使用插件进行压缩

```shell
$ lessc index.less index.css --clean-css
```



##### 3.2  外部工具的编译方式

为什么需要使用外部工具的编译方式呢,因为命令行的编译方式,没写一次less,都需要手动的编译,就非常耗时间,还不能实时刷新

外边的工具可以帮我们解决这样一些问题,通过实时编译刷新,就会想我们使用css一样,实时看到开发效果,



###### 1. 安装考拉less 客户端编译工具

官网下载

使用Koala外部编译软件,可以选择是否压缩编译结果compress,可以选择是否监听less文件的变化,同时可以自动编译,还可以生成资源地图Source Map文件



##### 3.3 使用开发工具webstorm 编译

WebStorm 内置 File Watchers, 

设置方式:

>  文件 >> 设置 >> 工具 >> File Watchers >> 添加选择less >> 指定输出目录



##### 3.4 使用开发工具vscode编译

在vscode中通过下载 easy Less插件,来使用less自动编译功能