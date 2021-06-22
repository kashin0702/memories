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



### MessageBox弹框

调用`$confirm`方法即可打开消息提示，它模拟了系统的 `confirm`。Message Box 组件也拥有极高的定制性，我们可以传入`options`作为第三个参数，它是一个字面量对象。`type`字段表明消息类型，可以为`success`，`error`，`info`和`warning`，无效的设置将会被忽略。注意，第二个参数`title`必须定义为`String`类型，如果是`Object`，会被理解为`options`。在这里我们用了 Promise 来处理后续响应。

```html
<template>
  <el-button type="text" @click="open">点击打开 Message Box</el-button>
</template>
```

```js
 export default {
    methods: {
      open() {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      }
    }
  }
```



### Cascader 级联选择器

#### 添加商品分类

```html
<el-form-item label="父级分类">
    <!--expand 展开方式、 options绑定数据源、props指定配置选项 change-on-select允许选中任意项 -->
    <!--v-model 双向绑定props内的value数组-->
	<el-cascader expand-trigger="hover" ：options="cateList" :props="cascaderProps" v-model="selectOptions" @change="handleCascaderChange" change-on-select>
    </el-cascader>
</el-form-item>
```

```js
//级联选择器的配置对象
cascaderProps: {
    value: 'cat_id',    //指定选择器绑定的数据对象的属性值 
    label: 'cat_name',  //指定页面上看到的名称
    children:  'children'  //指定父子节点嵌套属性
},
//选中的分类id数组，级联选择器可能会选择多项，所有被选中的项会保存在一个数组中
//例如选择'电视'->'索尼'->'40寸'  就是一个数组
selectOptions: []

//选择器发生变化时触发
handleCascaderChange(){
    console.log(this.selectOptions)//selectOptions即选择器绑定的 value:cat_id 数组
    if(this.selectOptions.length){
        //1.如果长度大于0表示选了具体分类，把数组的最后一个cat_id传给api参数对象
        this.addForm.cat_id = this.selectOtions[this.selectOptions.length - 1]
        this.addForm.cat_level = this.selectOptions.length //cat_level即层数 等于数组的长度
    }else{ //2.没选父级分类，即要设置为1级分类，api的传参参数都设置为0
        this.addForm.cat_id = 0
        this.addForm.cat_level = 0
    }
}

//发起添加分类post请求,传入上面获得的addForm对象
 addCate(){
    this.$refs.addCatFormRef.validate(async valid => { //先验证表单
        if(!vaild) return
      	const {data:res} = await this.$http.post('categories',this.addForm) //传入addForm参数对象
        if(res.data.status !== 200) return this.$message.error('添加分类失败')
        this.$message.success('添加分类成功')
        this.getCateList()  //重新获取分类列表，刷新页面
        this.addDialogVisible = false  //隐藏对话框
    })
}

//额外知识:如何让cascader只能选择第3级分类? 
//判断v-model绑定的selectOptions数组长度是否等于3
handleCascaderChange(){
    if(this.selectOptions.length !== 3){ //不等于3表示选择的不是3级分类
        this.selectOptions = []  //清空数组并返回
        return
    }
    // 正常处理代码
}
```



### el-tabs 标签页

```html
<!--v-model绑定的是el-tab-pane的name属性-->
<el-tabs @click="tabClick" v-model="activeName">
	<el-tab-pane label="用户管理" name="many">动态参数</el-tab-pane>
    <el-tab-pane label="角色管理" name="only">静态参数</el-tab-pane>
</el-tabs>
```

```js
data(){
    return {
        activeName: ''
    }
}
//获取商品参数接口categories/:id/attributes
//传参：1.id为商品id， 2.sel: many表示动态参数 sel:only表示静态参数 可动态绑定tabs的name属性
async handleChange(){ //在级联选择器选中商品后即发起请求
    if(this.selectOptions !== 3){
        this.selectOptions = []
        return 
    }
    //selectOptions数组的最后一项就是要传的商品id,传参动态绑定v-model的activeName
    const {data:res} = await this.$http.get('categories/'+ this.selectOptions[2] + '/attributes',		{params: {sel: this.activeName}}) 
    //根据点击的tab标签分别保存2个页面的数据，使页面能分别渲染不同的数据源
    if(this.activeName === 'many'){
        this.manyDataList = res.data
    }else{
        this.onlyDataList = res.data
    }
}
```



#### 2个标签页共用1个el-dialog对话框组件

```html
<!--利用计算属性动态修改对话框的标题， dialogVisible控制是否可见-->
<el-dialog :title="'添加' + titleText" :visible.sync="dialogVisible" @close="dialogClosed">
</el-dialog>
```

```js
data(){
    return {
        dialogVisible: 'false'
    }
}
computed:{
    titleText(){
        if(this.activeName === 'many'){
            return '动态属性'
        }else{
            return '静态属性'
        }
    }
},
    methods: {
        dialogClosed(){
            this.$refs.formRef.resetField()  //每次关闭对话框时重置表单数据
        }
    }
```



#### 添加商品参数接口

api路径： categories/:id/attributes

传参: 1. :id  2.attr_name   3.attr_sel (many||only)   4.attr_vals （如果是many就要填值，可选参数） 

```js
addParams(){
    this.$refs.formRefs.validate(async valid => { 
        if(!valid) return //先对表单进行预验证
        //验证通过后发起post请求
        const {data: res} = await this.$http.post(`categories/${this.cateId}/attributes`,{
        attr_name: this.addForm.attr_name, //根据form表单绑定数据源获得
        attr_sel: this.activeName
    })
    if(res.data.status !== 201){
        this.$message.error('添加商品参数失败')
        return
    }
    this.$message.success('添加商品参数成功')
    this.dialogVisible = false //隐藏对话框
    this.getParams() //更新数据列表
    })
    
}
```



#### 删除商品参数接口

```html
<!--通过scope把要删除的参数id传给函数-->
<el-button @click="removeParams(scope.row.attr_id)">删除</el-button>
```

```js
//根据id执行删除操作
async removeParams(attr_id){
    //confirm会返回一个promise对象 
    const confirmResult = await this.$confirm('此操作将永久删除该参数，是否继续？','提示',
     {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
    }).catch(err => err)
    //1.用户点击了取消
    if(confirmResult !== 'confirm'){ 
        return this.$message.info('已取消删除')
    }
    
    //2.用户点击了删除，执行删除请求
   const {data:res} = await this.$http.delete(`categories/${this.cateId}/attributes/${attr_id}`)
   if(res.data.status !== 201)return this.$message.error('删除参数失败')
   this.$message.success('删除成功')
   this.getParams()
}
```



### form input switch 包括table双向绑定是核心思想

双向绑定v-model  和本地data 的互相绑定 驱动页面



### 修改用户状态   put请求

请求路径: users/:userID/state/:type    冒号表示参数

请求方法： put  (修改后台数据，都用put方法)

 入参： uid：用户id     type:用户状态

```js
//响应数据
{
    "data": {
        "id": 111,
        "username": "admin",
        "mobile": "136",
        "state": 0
    },
        "meta": {
            "msg": "状态修改成功",
            "status": 200
        }
}
//前端请求 在switch监听事件中进行put请求，入参通过slot-scope获取
userStateChange(userInfo){
    //使用模板字符串动态传参
    axios.put(`/users/${userInfo.id}/state/${userInfo.state}`).then(res => {
    if(res.meta.status !== 200) {
        userInfo.state = !userInfo.state // 更新状态失败时要保证前端页面状态不会更改，所以改回去
        return this.$message.error('修改状态失败')
    }
    this.$message.success('修改状态成功')
})
}

```

### 修改用户信息 put请求

请求路径: /users/:userId

请求参数: email  mobile   即要修改的内容

```js
editUserInfo(){
    this.$refs.userForm.validate(valid => {
        if(!valid) return
        this.$http.put('/users/' + this.form.id,{
            email: this.form.email,
            mobile: this.form.mobile
        }).then(res => {
            if(res.meta.status !==200){
               return this.$message.error('修改用户信息失败')
            }
             //修改数据三部曲 1.提示 2.关对话框 3.刷新页面
            this.$message.success('修改用户信息成功')
            this.editdialogVisible = false  //关闭对话框
            this.getUserList()  //每次做增删改查操作后都要重新更新数据！！！
        })
    })
}
```



### 添加用户操作 post请求

请求路径： /users

入参： username,password,email,mobile

```js
this.$http.post('/users', this.form).then(res => {
    if(res.meta.status !== 201){
       return this.$message.error('添加失败')
    }
    this.$message.success('添加成功 ')
})
```

### slot-scope 作用域插槽拿当前行数据

### 根据传入用户id查询到用户信息

```js
getUserInfo(id){
    this.$http.get('/users/'+id).then(res => {})
}
```



### el-dialog    visible.snyc = "isShow"

控制对话框显示或隐藏



### el-table的列项 el-table-column 特殊属性

type="expand"    展开效果项

type="index"   索引项



### messageBox  弹框

```html
<template>
  <el-button type="text" @click="open">点击打开 Message Box</el-button>
</template>

```

```js
export default {
    methods: {
      open() {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      }
    }
  }
```



### 权限的概念

用户---》角色-----》权限

用户绑定不同的角色， 角色拥有不同的权限

只要用户拥有这个角色，那这个用户就有这个角色下的所有权限



权限有层级概念，一级权限，二级权限，三级权限



### 删除权限接口

roles/:roleId/rights/:rightsId

入参: roleId 角色id   rightsId 权限id

方法：delete

```js
//点击el-tag的删除图标触发@close删除事件
//通过作用域插槽获得scope.row,item.id   传入role rightsId给删除函数
removeRightsById(role,rightsId){
    this.$confirm('是否删除该权限？',{
         confirmButtonText: '确定',
         cancelButtonText: '取消',
         type: 'warning'
    }).then(() => {
        this.$http.delete(`roles/${role.id}/rights/${rightsId}`).then(res =>{
            if(res.meta.status !== 200){
                return this.$message.error('删除权限失败')
            }        
        this.$message({
            type: 'success',
            message: '删除成功!'
        })
    })
   	.catch(err => {
        this.$message({
            type: 'info',
            message: '已取消删除'
          }); 
    })
    
}
```



### 树形控件 tree



```html
<!--defKeys是默认要勾选的权限列表对应的id数组, treeProps:绑定rightLists对象的具体属性值-->
<el-tree :data="rightLists" :props="treeProps" show-checkbox node-key="id" default-expand-all :default-checked-keys="defKeys"></el-tree>

```

```js
treeProps: {
    label: 'authName',  //控件要显示的名字对应的属性
    children: 'children' //父子嵌套对应的属性
}
```



### 递归函数

加载角色当前已有权限

通过递归获取树形3级权限列表,并保存到数组中

```js
data: {
    return {
        defKeys: []
    }
}

getLeafKeys(node,arr){
    //1.如果当前node节点不包含children属性，则是3级节点，把node的id保存到数组arr中
    if(!node.children){
        return arr.push(node.id)
    }
    //2.不是3级节点，循环children属性，拿到字节点item，对item再次调用函数进行递归
    node.children.forEach(item => {
        this.getLeafKeys(item,arr)
    })
}
//调用,传当前角色信息(scope.row)，包含了权限列表，传要保存的数组
this.getLeafKeys(role,this.defKeys)
//注意，每次关闭对话框要清空数组，防止每次点击分配权限按钮时都会添加权限到数组中,产生bug
dialogClosed(){
    this.defKeys = []
}
```



### 权限列表分配权限

```js
//分配权限事件
async allotKeys(){
    const keys = [ //利用tree组件的2个方法拿到所有选中和半选中数组，合并成一个数组
        ...this.$refs.treeRef.getcheckedKeys(), ...this.$refs.treeRef.getHalfCheckedKeys()
    ]
    const str = keys.join(',') //把数组转换成字符串作为参数传给接口
    const {data:res} = await this.$http.post(`roles/${this.roleId}/rights`,{rids: str})
        this.$message.success('更新列表成功')
    	this.getRoleList()  //请求成功后，更新权限列表
    	this.setdialogVisible = false  //隐藏对话框
    })
    
}
```



### 第三方插件vue-table-with-tree-grid

```js
//安装
npm install vue-table-with-tree-grid -S
//导入
import TreeTable from 'vue-table-with-tree-grid'
//注册全局组件
Vue.component('tree-table'，TreeTable)
```

```html
<!--使用组件,绑定数据源, columns每列定义规则数据-->
<tree-table :data="catelist" :columns="columns">
	
</tree-table>
```

```js
//columns定义每列数据展示规则：name,type

```



### 计算属性绑定button的disabled属性

```html
<el-button :disabled="isDisabled"></el-button>
```

```js
computed: {
    isDisabled(){
        if(this.selectOptions.length !== 3){ //未选中3级分类，返回true，即button不可用
            return true
        }
        return false
	}
}

```



###  el-tabs和el-steps 实现数据联动效果

```html
<!--关键点： steps的active属性和tabs的v-model都绑定在activeIndex上，实现数据联动效果-->
<!--让activeIndex - 0 可以把字符串转化为数值型 -->
<el-steps :space="200" :active="activeIndex - 0">
    <el-step title="基本信息"></el-step>
    <el-step title="商品参数"></el-step>
    <el-step title="完成操作"></el-step> 	
</el-steps>

<!--当tabs的v-model修改了activeIndex,自动激活steps的对应效果-->
<el-tabs v-model="activeIndex" ：tab-position="left">
	<el-tab-pane label="基本信息" name="0">基本信息</el-tab-pane>
    <el-tab-pane label="商品参数" name="1">商品参数</el-tab-pane>
    <el-tab-pane label="完成操作" name="2">完成操作</el-tab-pane>
</el-tabs>
```



### el-form嵌套el-tabs

只能在form内嵌套tabs, **不能在tabs内嵌套el-form**，会报错

```html
<!--绑定表单数据对象和校验规则-->
<el-form :model="addForm" :rules="addFormRules">
    <el-tabs>
        <!--标签内插入form-item-->
        <el-tab-pane label="基本信息">
            <el-form-item label="商品名称" prop="goods_name">
                <el-input v-model="addForm.goods_name"></el-input>
            </el-form-item>
        </el-tab-pane>
    </el-tabs>
</el-form>
```

```js
data(){
    return {
        addForm: {
            goods_name: ''
        },
        addFormRules: {
            goods_name:[
                {required: true, message: '请输入商品名称', trigger: 'blur'}
            ]
        }
    }
}
```

