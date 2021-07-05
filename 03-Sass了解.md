### Sass

Sass也是css预处理语言



#### 1. 安装

##### 1.1  官网安装ruby

因为Sass是基于ruby的,所以要先安装ruby



##### 1.2 安装sass

方式一,在线安装

进入ruby控制台安装

```shell
gem  install sass
```



方式二: 本地安装

官网下载 安装包

下载完安装包以后在通过ruby命令行本地安装

```shell
gem install c:\sass-3.4.23.gem
```





##### 1.3  测试是否安装成功

```shell
$ sass -v
```



#### 3. sass的基础语法

2007年出现,但到目前普及度不高,原因是最初的语法不太容易让开发人员接受

```sass
@color: #f00    // 定义一个 变量

body
	font-size: 16px
	color: @color
```





#### 4. 编译:

注意: ,sass的文件 后缀名是scss

```shell
sass d:\1.scss d:\1.css
```

