```js
//获取一级分类列表  catering.goods.category/index
		getCateList: function() {
			let _this = this;
			getApp().globalData._get('catering.goods.category/index', {}, function(res) {
				let list = res.data;
				let cateList = [];
				list.map(v => {
					if (v.child) {  //如果有child 即有二级分类
						v.child.map(item => {  
							cateList.push(item); //把二级分类push给cateList
						});
					} else {
						cateList.push(v); //没有则把一级分类push给cateList
					}
				});

				_this.setData(
					{
						cates: list,   //一级分类cates
						cateList: cateList, //包含一级和二级的cateList
					},
					() => {
						_this.getProductList(_this.currentSort); //回调获取商品列表
					}
				);
			});
		},
            
            
//点击分类 切换显示不同的菜单  item.category_id, item.name, index 直接把遍历对象的值传入动态切换
changeCate(cate_id, name, index) {
			this.selectedCateId = cate_id;    
			this.selectedCateName = name;
			this.selectedCateIndex = index; //保存当前点击的索引
			this.selectedChildCateId = 0;
			this.selectedChildCateName = '全部';
			this.isShowSelect = true
			if(!this.cates[index].child){  
				this.changeChildCate(0, '全部') //没有child子分类，就获取全部分类
			}
		},
    

changeChildCate(cate_id, name) { //传入分类id,名称 切换二级选项
			this.selectedChildCateId = cate_id;
			this.selectedChildCateName = name;
			this.isShowSelect = false;
			if (cate_id == 0) {
				this.getProductList(this.selectedCateId); //获取商品列表
			} else {
				this.getProductList(cate_id);
			}
		},
 
  //核心方法，切换1，2级分类都会调用。点击菜品分类时，获取商品列表  catering.goods.goods/index   
getProductList: function(id) {
			let _this = this;
			getApp().globalData._get(
				'catering.goods.goods/index',
				{
					category_id: id,
					listRows: 200,
					search: _this.search
				},
				function(res) {
					_this.setData({
						productList: res.data.data
					});

					_this.getShopCarList(); //调用获取购车列表
				}
			);
		},
  
  //每次获取商品列表时同时获取购物车列表 catering.order.cart/lists
getShopCarList: function() {
			let _this = this;
			getApp().globalData._get(
				'catering.order.cart/lists',
				{
					seat_name: getApp().globalData.seatName,
					seat_id: getApp().globalData.seatId,
					store_user_id: _this.store_user_id
				},
				function(res) {
					if (res.data.seat_info && res.data.seat_info.open_time) {
						res.data.seat_info.open_time = getApp().globalData.timestampToTime(res.data.seat_info.open_time);
					}
					_this.setData({
						shopCarList: res.data //接口返回的购物车数据
					});

					_this.InitShopCarList(res.data, _this.productList);//初始化购物车
				}
			);
		},
            
//初始化购物车列表        
InitShopCarList: function(shopCarList, productList) {
			for (var i = 0; i < productList.length; i++) {
				for (var j = 0; j < productList[i].sku.length; j++) {
					productList[i].sku[j].shopNum = 0;

					for (var k = 0; k < shopCarList.pre_goods_list.length; k++) {
						if (productList[i].sku[j].spec_sku_id == shopCarList.pre_goods_list[k].goods_sku_id && productList[i].goods_id == shopCarList.pre_goods_list[k].goods_id) {
							productList[i].sku[j].shopNum = shopCarList.pre_goods_list[k].total_num;
						}
					}
				}
			}

			this.setData({
				productList: productList
			});
		},
            
            
//添加到购物车    
add: function(spec_sku_id, goods_id, index) {
			let _this = this;
			let gid = goods_id;    //保存goods_id和spec_sku_id
			let sid = spec_sku_id;
			let item = '';
			if (index) {
				item = _this.productList[index]; //若index存在则item等于当前商品对象
				if (item.sku[0].shopNum > 0) { 
					item.sku[0].shopNum = item.sku[0].shopNum + 1; //shop_num》0则 +1
				} else {
					item.sku[0].shopNum = 1; //为0即没有添加，购物车数量=1
				}
				_this.$set(_this.productList, index, item);//productList添加index,item属性
			}

			getApp().globalData._post_form(
				'catering.order.cart/add', //发起post调用添加购物车接口
				{
					goods_id: gid,  //goods_id
					goods_num: 1,   //商品数量
					goods_sku_id: sid,  //sku_id
					store_user_id: _this.store_user_id,
					seat_name: getApp().globalData.seatName,
					seat_id: getApp().globalData.seatId
				},
				() => {
					// 获取购物车列表
					_this.getShopCarList();  //调用成功后发起购物车列表事件
				},
				err => {
					if (index) {
						item.sku[0].shopNum = 0;
						_this.$set(_this.productList, index, item);
					}
				},
				() => {
					uni.hideLoading();
				}
			);
		},
```





### mill添加购物车事件

思路：通过catering.goods.goods接口返回， 获取返回数组内item上的**is_combo_goods**   **combo_goods**内数据

关键事件 **getProductList**

```js
//点击一级菜单  1先获取当前分类下商品列表，2再获取购物车列表数据
getProductList: function(id) {
			let _this = this;
			getApp().globalData._get(
				'catering.goods.goods/index',
				{
					category_id: id,
					listRows: 200,
					search: _this.search
				},
				function(res) {
					_this.setData({
						productList: res.data.data
					});

					_this.getShopCarList(); //获取购物车列表
				}
			);
		},
   
  //获取完商品列表后，获取购物车列表
 getShopCarList: function() {
			let _this = this;
			getApp().globalData._get(
				'catering.order.cart/lists',
				{
					seat_name: getApp().globalData.seatName,
					seat_id: getApp().globalData.seatId,
					store_user_id: _this.store_user_id
				},
				function(res) {
					if (res.data.seat_info && res.data.seat_info.open_time) {
						res.data.seat_info.open_time = getApp().globalData.timestampToTime(res.data.seat_info.open_time);
					}
					_this.setData({
						shopCarList: res.data //接口返回的购物车数据
					});

					_this.InitShopCarList(res.data, _this.productList);//初始化购物车
				}
			);
		},
```





遍历 shopCarList.pre_goods_list

左侧购物车去掉index和num 

点击一级菜单时，发生请求获取商品列表，获取购物车列表

1. 调**catering.order.cart/add**  发起添加购物车请求   (接口返回商品数量)
2. 通过add接口的返回，发起getShopcarList事件，调用**catering.order.cart/lists**



### 编辑会员信息 会请求的数据

```js
//1.catering.order.order/changeUser
//只返回消息 res.data= '更换用户成功'
//2.catering.order.order/checkOut
//返回订单状态在内的各种信息
//3.catering.order.order/detail
//  返回订单详情
//4.user/card_list
// 返回储值卡数据，没有储值卡则无数据


//关键方法: 关闭编辑会员信息弹窗时触发，发送自定义事件'closeMemberListPop' 以及user_id(会员ID), user_info(会员信息), user_card_id(会员卡ID) 3个关键数据 给父组件
changePop(e){
	if(!e.show){						           this.$emit('closeMemberListPop',this.chooseUserId,this.userInfo,this.chooseUserCardId)
				}
			},
 //关闭弹窗，清空所有数据
 closeMemberListPop() {
				this.$refs.memberListsPopup.close();
				this.search = ''
				this.page = 1
				this.list = []
				this.user_card_list = []
			}
//确认按钮，其实也是调用上面的方法
confirmMemberListPop(){
				this.closeMemberListPop()
			}


//父组件调用，获得3个数据
closeMemberListPop(user_id,userInfo,user_card_id) {
			// this.$refs.memberListsPopup.close();
			this.showMemberList = false;
			this.user_id = user_id;
			this.userInfo = userInfo
			this.user_card_id = user_card_id
		},
            
//核心思路：父组件通过watch监听数据改变，从而发起请求刷新页面
watch: {
    //监听到user_id发生改变，发起changeUser请求
   user_id: function(val, old) {
			console.log(val, old);
			this.setData({
				choosePayName: '现金',
				choosePayType: 100,
				discountRatio:1
			});
			if (old !== '' && val >= 0) {
				
				this.changeUser(val); //发起请求
			}
		}, 
}
//changeUser 修改会员请求
changeUser(user_id) {
			let _this = this;
			getApp().globalData._get(
				'catering.order.order/changeUser',
				{
					order_id: _this.id,
					user_id:user_id || 0,
					mobile:_this.userInfo && _this.userInfo.mobile || '',	
					realName:_this.userInfo && _this.userInfo.realName || '',	
					card_no:_this.userInfo && _this.userInfo.card_no || ''
				},
				function(res) {
					uni.showToast({
						title: '修改成功',
						icon: 'none'
					});
					_this.getCoupons(_this.id);  //调用getCoupons请求
				}
			);
		}

//getCoupons调用 ---catering.order.order/checkOut 
//getCoupons内调用getDetails --- catering.order.order/detail
```

