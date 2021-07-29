### actionsheet组件

属性:

menus    菜单项列表

value       使用v-model绑定是否显示变量

show-cancel    是否显示取消菜单，对安卓风格无效

cancel-text     取消菜单的显示文字

事件:

@on-click-menu  (menuKey, menuItem)   点击菜单时触发

@on-click-menu-cancel    点击取消时触发

@on-click-mask   点击遮罩时触发

```vue
<actionsheet v-model="show" 
             :menus="actionsheetMenus" 
             @on-click-menu="clickMenu" 
             show-cancel>
</actionsheet>

<script>
actionsheetMenus: [{
        label: `<span style="font-size:12px;">报装单上证件号与该地址上模拟客户的证件号不一致</span>`,
        type: 'info'
      }, {
        label: '新装客户',
        type: 'primary',
        value: 'cust'
      }, {
        label: '新装用户',
        type: 'primary',
        value: 'user'
      }],
    methods: {
        clickMenu (menuKey, menuItem) {
      // gyf add 打印参数 点了新装客户
      console.log('参数menuKey', menuKey) // cust  即value值
      console.log('参数menuItem', menuItem)//{label: '新装客户', type: 'primary' ,value: 'cust'}
      if (menuItem === undefined) {
        return
      }
      let self = this
      if (menuItem.value === 'user') {
        self.$router.push('/broadband/confirm')
      } else if (menuItem.value === 'cust') {
        // 设置默认开通方式为新增客户
        self.ADD_INFO_SET_OPEN_TYPE(constant.OPEN_TYPE_XZKH)
        self.ADD_INFO_SET_PAY_METHOD(constant.ENUMS_PAY_MODE_XJ)
        // 设置是否为共享机顶盒
        self.EQUIPMENT_SET_IS_INNER(false)
        // self.EQUIPMENT_SET_IS_INNER_CHOSE(true)
        self.$router.push('/broadband/confirm/cust')
      }
    }
    }
</script>
```

