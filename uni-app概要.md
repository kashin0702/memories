### 创建uni-app项目

#### 命令行创建项目

1. 全局安装脚手架： npm install @vue/cli -g
2. 创建项目： vue create -p dcloudio/uni-preset-vue my-project
3. 启动项目（微信小程序）: npm run dev:mp-weixin
4. 微信小程序开发者工具导入项目



### 目录结构

App.vue ：  应用配置，用来配置App全局样式及监听

main.js:  Vue初始化入口文件

mainfest.json : 配置应用名称、appid、logo 、版本等打包信息

pages.json: 配置页面路由、导航条、选项卡等页面类信息

uni.scss: 内置的sass变量，可直接使用



pages->index->index.vue 页面组件

static->  静态资源