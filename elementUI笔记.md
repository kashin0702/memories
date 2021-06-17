### el-form 主表单

主表单需要绑定属性  **:model="form"**绑定form对象的数据

form对象包含所有该表单控件内的数据

```html
<el-form :model="form">
	
</el-form>
```

```js
data(){
    return {
        form: {
            
        }
    }
}
```

### el-form-item 表单域

在el-form中，每个控件(input,select,checkbox)都要放在表单域中，可以设置label属性

```html
<el-form :model="form">
    <el-form-item label="用户名">
    	
    </el-form-item>
</el-form>
```

### el-input 输入控件

每个控件都需要使用**v-model**对数据双向绑定

```html
<el-form :model="form">
    <el-form-item label="用户名">
        <el-input v-model="form.name"></el-input>
    </el-form-item>
       
    <el-form-item label="密码">
        <el-input v-model="form.pwd" show-password></el-input>
    </el-form-item>
</el-form>
```

```js
data(){
    return {
        form: {
            //子控件数据保存在form对象下
            name: '',
            pwd: ''
        }
    }
}
```

### el-button 按钮

#### 重置表单： resetFields 

```html
<!--主控件必须绑定ref用于获取该实例对象-->
<el-form ref="loginForm">
                          <!--绑定事件-->
<el-button type="primary" @click="resetLogin">确定</el-button>
```

```js
methods: {
    resetLogin(){
        this.$refs.loginForm.resetFields()
    }
}
```



### 表单验证规则

1.el-form控件绑定 **:rules="loginRules"**验证规则

2.el-form-item通过**prop**绑定对应的规则

```html
<el-form :model="form" :rules="loginRules">
    							<通过prop把规则传给item>
    <el-form-item label="用户名" prop="username">
        <el-input v-model="form.name"></el-input>
     </el-form-item>
    
        <el-form-item label="密码" prop="password">
        	<el-input v-model="form.pwd" show-password></el-input>
        </el-form-item>
   
</el-form>
```

```js
data(){
    return {
        loginRules: {
            username: [ //验证规则是一个数组，每条保存一个规则
            	{required: true, message: '用户名不能为空',trigger: 'blur'},
           	 	{min: 6, max: 10, message:'用户名长度为6到10个字符', trigger: 'blur'}
            ],
            password: [
                {required: true, message: '密码不能为空',trigger: 'blur'},
           	 	{min: 6, max: 10, message:'密码长度为6到10个字符', trigger: 'blur'}
            ]
        }
    }
}
```



### 登录预验证validate()



### 路由导航守卫

#### 控制访问权限

```js
router.beforeEach((to,from,next) => {  //next表示放行函数
    //情况1：如果访问登录页，直接放行
    if(to.path === '/login') return next()
    //从ssesion拿到token
    const str_token = window.ssesionStorage.getItem('token')
    //情况2：若token不存在，则跳转回登录页
    if(!str_token) return next('/login') //放行并强制跳转到login页
    //情况3：token存在，直接放行
    next()
})
```

#### 退出功能(清除token)

```js
//清除token
window.ssessionStorage.clear()
//跳转到登录页
this.$router.push('/login')
```

#### 所有请求需要加上token，通过axios拦截器添加

```js
axios.interceptors.request.use(config => {
    config.headers.Authrization = window.sessionStorage.getItem('token')
    return config
})
```



### 左侧菜单el-menu

el-menu 展开收拢效果 :collapse="boolean"

展开收拢动画效果开关  :collapse-transition="boolean"

```html
<el-menu>
    <el-submenu>
        <!--菜单图标-->
        <i class="el-icon-menu"></i>
        <span>一级菜单</span>
        <!---嵌套二级菜单-->
        <el-menu-item>
        	<i class="el-icon-menu"></i>
        	<span>二级菜单</span>
        </el-menu-item>
    </el-submenu>
</el-menu>
```



### 左侧菜单接口 请求方法:get

```js
{
    "data": 
    {
        "id": 1001,
        "authName": '商品管理',
        "path": null,
        "children": [ //children表示二级菜单
            {"id": 1002, "authName": 'xxx', "path": null, "children": {}},
            {"id": 1003, "authName": 'xxx', "path": null, "children": {}}
        ]
    },
    {
        
    }
    "meta": {
        "msg": "获取菜单列表成功",
        "status": 200
    }
}
```



### 左侧路由改造

官网navMenu导航菜单- 属性：router

1.给el-menu 添加属性  **:router="true" 开启路由模式**

2.给每个要跳转的二级频道的 index绑定接口返回的path  **:index=" '/'+item.path "**



### breadcrumb 面包屑导航



### card 卡片视图



### Layout 栅格布局

el-row  一行默认是24   :gutter 两个col的间距

​	el-col :span="6" 占据一行的1/4 根据col设置的数字进行一行的排列



### el-table-column转换单元格内数据

绑一个formatter方法

```html
<el-table-column prop="status" label="状态" :formatter="formatterStatus">
```

```js
formatterStatus(row){  //方法内会传入一个row对象，可对数据进行转换
      switch(row.status){
        case 0:
          return '已提交'
        case 1:
          return '已到店'
        case -1:
          return '未到店'
        case -2:
          return '已过期'
        case -3:
          return '已取消'
        case -4:
          return '延期'
      }
```

