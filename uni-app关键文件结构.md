## mill-uniapp结构

### App.vue

```vue
<script>
	//app.js
	// 站点配置文件
	import siteinfo from "./siteinfo"; // 工具类
	// 工具类
	const util = require("./utils/util.js");
	var wss_url = 'wss://mp3.minstech.cn/wss';
	let lastRequestInfo = {
		lastClickTime: 0,
		lastUrl: ''
	}
	export default {
		globalData: {
			com: util,
			siteinfo: siteinfo,
			// api地址
			api_root: siteinfo.api_root + 'index.php?s=/store_api/',
			uniacid: siteinfo.uniacid,
			seatId: '',
			isFastClick: function(url, interval = 1000) {
				let time = new Date().getTime();
				let flag = false;
				if (lastRequestInfo.lastClickTime + interval <= time) { //时间没到
					flag = false;
				} else if (url != lastRequestInfo.lastUrl) {
					flag = false;
				} else {
					flag = true;
				}
				lastRequestInfo.lastClickTime = time;
				lastRequestInfo.lastUrl = url;
				return flag;
			},
			/**
			 * 获取商城ID，根据url路径上的参数设置
			 */
			setWxappId(){
				let routes = getCurrentPages() //页面栈数组
				let curOptions = routes[routes.length - 1].options
				let {wxapp_id} = curOptions	
				uni.setStorageSync("wxapp_id",wxapp_id)		
			},
			getWxappId() {
				return uni.getStorageSync("wxapp_id") || siteinfo.wxapp_id
			},


			/**
			 * 获取场景值(scene)
			 */
			getSceneData(query) {
				return query.scene ? util.scene_decode(query.scene) : {};
			},

			/**
			 * 当前用户id
			 */
			getUserId: function() {
				return uni.getStorageSync('user_id');
			},

			sockectReConnect(currentSocket) {
				// console.info("进入了重连");
				// console.info(currentSocket);

				// if (!currentSocket) {
				//   this.sockectInitConnect();
				//   return;
				// }

				// if (currentSocket.readyState !== 0 && currentSocket.readyState !== 1) {
				//   this.sockectInitConnect();
				// }
			},

			sockectInitConnect() {
				let currentSocket;
				currentSocket = uni.connectSocket({
					url: wss_url,
					success: function(res) {
						console.log(res);
					},
					fail: function(err) {
						console.log(err);
					}
				});
				this.currentSocket = currentSocket;
			},

			socketAddGroup() {
				let _this = this;

				var send = {
					type: 'group',
					group_id: 'server_11',
					msg: 'add group'
				};
				send = JSON.stringify(send);
				uni.sendSocketMessage({
					data: send,
					complete: function(data) {
						console.log('加入分组', data);
					}
				});
			},

			socketSendServerGroup() {
				let _this = this;

				var send = {
					type: 'message',
					group_id: 'server_11',
					msg: 'change order'
				};
				send = JSON.stringify(send);
				uni.sendSocketMessage({
					data: send,
					complete: function(data) {
						console.log(data);
					}
				});
			},

			/**
			 * 小程序主动更新
			 */
			updateManager() {
				if (!uni.canIUse('getUpdateManager')) {
					return false;
				}

				const updateManager = uni.getUpdateManager();
				updateManager.onCheckForUpdate(function(res) { // 请求完新版本信息的回调
					// console.log(res.hasUpdate)
				});
				updateManager.onUpdateReady(function() {
					uni.showModal({
						title: '更新提示',
						content: '新版本已经准备好，即将重启应用',
						showCancel: false,

						success(res) {
							if (res.confirm) {
								// 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
								updateManager.applyUpdate();
							}
						}

					});
				});
				updateManager.onUpdateFailed(function() {
					// 新的版本下载失败
					uni.showModal({
						title: '更新提示',
						content: '新版本下载失败',
						showCancel: false
					});
				});
			},

			/**
			 * get请求
			 */
			_get(url, data, success, fail, complete, check_login) {
				// uni.showNavigationBarLoading();

				let _this = this;

				data = data || {};
				data.wxapp_id = _this.getWxappId();
				data.shop_id = uni.getStorageSync("shop_id");

				if (uni.getStorageSync("sessionId")) {
					var header = {
						'content-type': 'application/x-www-form-urlencoded',
						'sessionId': uni.getStorageSync("sessionId") || '',
						'authKey': uni.getStorageSync("authKey") || ''
					};
				} else {
					var header = {
						'content-type': 'application/x-www-form-urlencoded'
					};
				} // 构造get请求


				let request = function() {
					data.token = uni.getStorageSync('token');
					uni.request({
						url: _this.api_root + url,
						header: header,
						data: data,
						sslVerify: false,
						success(res) {
							if (res.statusCode !== 200 || typeof res.data !== 'object') {
								console.log(res);

								_this.showError('网络请求出错');

								return false;
							}

							if (res.data.code === -1) {
								// 登录态失效, 重新登录

								_this.doLogin();
							} else if (res.data.code === 0) {
								_this.showError(res.data.msg, function() {
									fail && fail(res);
								});

								return false;
							} else {
								success && success(res.data);
							}
						},

						fail(res) {
							_this.showError(res.errMsg, function() {
								fail && fail(res);
							});
						},

						complete(res) {
							// uni.hideNavigationBarLoading();
							// uni.hideLoading();
							complete && complete(res);
						}

					});
				}; // 判断是否需要验证登录


				check_login ? _this.doLogin(request) : request();
			},

			/**
			 * post提交
			 */
			_post_form(url, data, success, fail, complete, isShowNavBarLoading) {
				let _this = this;

				data.wxapp_id = _this.getWxappId();
				data.token = uni.getStorageSync('token');
				data.shop_id = uni.getStorageSync("shop_id");

				if (uni.getStorageSync("sessionId")) {
					var header = {
						'content-type': 'application/x-www-form-urlencoded',
						'sessionId': uni.getStorageSync("sessionId") || '',
						'authKey': uni.getStorageSync("authKey") || ''
					};
				} else {
					var header = {
						'content-type': 'application/x-www-form-urlencoded'
					};
				} // 在当前页面显示导航条加载动画



				uni.request({
					url: _this.api_root + url,
					header: header,
					method: 'POST',
					data: data,
					sslVerify: false,
					success(res) {
						if (res.statusCode !== 200 || typeof res.data !== 'object') {
							console.log(res, '网络请求出错');

							_this.showError('网络请求出错');

							return false;
						}

						if (res.data.code === -1) {
							// 登录态失效, 重新登录
							_this.doLogin();

							return false;
						} else if (res.data.code === 0) {
							_this.showError(res.data.msg, function() {
								fail && fail(res);
							});

							return false;
						}

						success && success(res.data);
					},

					fail(res) {
						// console.log(res);
						_this.showError(res.errMsg, function() {
							fail && fail(res);
						});
					},

					complete(res) {
						// uni.hideNavigationBarLoading();
						// uni.hideLoading();
						complete && complete(res);
					}

				});
			},

			/**
			 * 显示成功提示框
			 */
			showSuccess(msg, callback) {
				uni.showToast({
					title: msg,
					icon: 'success',
					mask: true,
					duration: 1500,

					success() {
						callback && setTimeout(function() {
							callback();
						}, 1500);
					}

				});
			},

			/**
			 * 显示失败提示框
			 */
			showError(msg, callback) {
				uni.showModal({
					title: '友情提示',
					content: msg,
					showCancel: false,

					success(res) {
						callback && callback();
					}

				});
			},

			/**
			 * 执行用户登录
			 */
			doLogin() {
				// 保存当前页面
				let pages = getCurrentPages();

				if (pages.length) {
					let currentPage = pages[pages.length - 1];
					"pages/auth/auth" != currentPage.route;
				} // 跳转授权页面


				uni.reLaunch({
					url: '/pages/auth/auth'
				});
			},

			setNavBar: function() {
				uni.getSystemInfo({
					success: res => {
						console.log(res);
						let totalTopHeight = uni.getMenuButtonBoundingClientRect().bottom + uni
							.getMenuButtonBoundingClientRect().top;
						this.statusBarHeight = res.statusBarHeight;
						this.titleBarHeight = totalTopHeight - res.statusBarHeight * 2;
					},
					fail: () => {
						this.statusBarHeight = 20;
						this.titleBarHeight = 44;
					}
				});
			},
			userInfo: null,
			//时分秒
			timestampToTime: function(timestamp) {
				var date = new Date(timestamp * 1000); //时间戳为10位需*1000，时间戳为13位的话不需乘1000

				var Y = date.getFullYear() + '/';
				var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '/';
				var D = date.getDate() < 10 ? '0' + date.getDate() + ' ' : date.getDate() + ' ';
				var h = date.getHours() < 10 ? '0' + date.getHours() + ':' : date.getHours() + ':';
				var m = date.getMinutes() < 10 ? '0' + date.getMinutes() + ':' : date.getMinutes() + ':';
				var s = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds();
				return Y + M + D + h + m + s;
			},
			//年月日
			timestampToYear: function(timestamp) {
				var date = new Date(timestamp * 1000); //时间戳为10位需*1000，时间戳为13位的话不需乘1000
				var Y = date.getFullYear() + '-';
				var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
				var D = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
				return Y + M + D;
			},
		},
		onLaunch: function(option) {

			if (!uni.getStorageSync("wxapp_id")) {
				console.log('分享进入的新用户', option);
			}

			let _this = this;

			// _this.globalData.updateManager();

			// _this.globalData.setNavBar();

			// _this.globalData.sockectInitConnect();
		},
		methods: {}
	};
</script>
<style>
	@import "./app.css";
</style>

```



### main.js

```js
import Vue from 'vue';
import App from './App';
import MyHeader from 'components/MyHeader.vue'
import SeatItem from 'components/SeatItem.vue'
//6.7 并台组件
import SeatItem2 from 'components/SeatItem2.vue'
//6.7 并台二次确认组件
import SeatSingle from 'components/SeatSingle.vue'
import MemberList from 'components/MemberList.vue'
import rangeDatePick from '@/components/pyh-rdtpicker/pyh-rdtpicker.vue';
Vue.config.productionTip = false;

Vue.component('my-header', MyHeader)
Vue.component('seat-item', SeatItem)
Vue.component('member-list', MemberList)
Vue.component('rangeDatePick', rangeDatePick)
Vue.component('SeatItem2',SeatItem2)
Vue.component('SeatSingle',SeatSingle)
Vue.mixin({
	methods: {
		setData: function(obj, callback) {
			let that = this;
			let keys = [];
			let val, data;
			Object.keys(obj).forEach(function(key) {
				keys = key.split('.');
				val = obj[key];
				data = that.$data;
				keys.forEach(function(key2, index) {
					if (index + 1 == keys.length) {
						that.$set(data, key2, val);
					} else {
						if (!data[key2]) {
							that.$set(data, key2, {});
						}
					}
					data = data[key2];
				})
			});
			callback && callback();
		}
	}
});

App.mpType = 'app';

const app = new Vue({
	...App
});
app.$mount();

```



### siteInfo.js

```js
//上线后修改 这个ENV的值就行了 一般为dev（本地调试） 或test（测试环境） 或 prod（生产环境）

//上线之前 修改下面三个常量 其他不用修改
const ENV = 'dev';
const VERSION = '1.0.2';
const UPTATE_TIME = '2019-12-06';
const UNIACID = '10001'; //集团id

const WXAPP_ID = '10002'; //具体门店id 

//5.12 新增MILL PUFF wxapp_id数组
const WXAPPID_ARR = ['10002','10007']


const NAME = 'mill酒吧服务员端';
let siteinfo = {};

if (ENV == 'prod') {
  siteinfo = {
    'api_root': 'https://mp3.minstech.cn/catering/php/web/',
    'version': VERSION
  };
} else if (ENV == 'test') {
  siteinfo = {
    'api_root': 'https://mp3test.minstech.cn/catering/php/web/',
    'version': "测试环境" + VERSION
  };
}else if (ENV == 'dev') {
  siteinfo = {
    'api_root': '/api/',
    'version': "测试环境" + VERSION
  };
} else {
  siteinfo = {
    'api_root': 'https://catering.cn/',
    'version': "开发环境" + VERSION
  };
}

siteinfo.update_time = UPTATE_TIME;
siteinfo.uniacid = UNIACID;
siteinfo.wxapp_id = WXAPP_ID;
module.exports = siteinfo;
```

### mainfest.json(跨域部分)

```json
"h5" : {
    "devServer" : {
        "port" : 8080,
        "disableHostCheck" : true,
        "proxy" : {
            "/api" : {
                "target" : "https://mp3test.minstech.cn/catering/php/web/",
                "changeOrigin" : true,
                "secure" : false,
                "pathRewrite" : {
                    "^/api" : ""
                }
            }
        }
    },
    "publicPath" : "./",
    "domain" : "https://mp3test.minstech.cn",
    "template" : "public/index.html",
    "title" : "mill",
    "router" : {
        "base" : "/catering/wxapp_customer/mill/mill_serve_pad/dist/prod/h5/"
    }
}
```





## 善语-uniapp结构

### App.vue

```vue
<script>
import utils from './common/utils.js'
export default {
    onLaunch: function(e) {
        console.log('App Launch');
        let extConfig = wx.getExtConfigSync? wx.getExtConfigSync(): {}
        console.log('env:',extConfig.env,extConfig)
        //#ifdef MP-WEIXIN	
        //检查更新
        this.updateManager();
        wx.login(); //重新登录,防止解密失败
        //#endif
        //应用启动参数
        this.onStartupScene(e.query);
    },
    onShow: function() {
        //console.log('App Show')
    },
    onHide: function() {
        //console.log('App Hide')
    },
    methods: {
        updateManager: function() {
            const updateManager = uni.getUpdateManager();
            updateManager.onCheckForUpdate(function(res) {
                // 请求完新版本信息的回调
                if (res.hasUpdate) {
                    updateManager.onUpdateReady(function(res2) {
                        uni.showModal({
                            title: '更新提示',
                            content: '新版本已经准备好，即将重启应用',
                            showCancel: false,
                            success(res2) {
                                if (res2.confirm) {
                                    // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                                    updateManager.applyUpdate();
                                }
                            }
                        });
                    });
                }
            });

            updateManager.onUpdateFailed(function(res) {
                // 新的版本下载失败
                uni.showModal({
                    title: '更新提示',
                    content: '检查到有新版本，但下载失败，请检查网络设置',
                    showCancel: false
                });
            });
        },
        /**
         * 小程序启动场景
         */
        onStartupScene(query) {
            // 获取场景值
            let scene = utils.getSceneData(query);
            // 记录推荐人id
            let refereeId = query.referee_id;
            if (refereeId > 0) {
                if (!uni.getStorageSync('referee_id')) {
                    uni.setStorageSync('referee_id', refereeId);
                }
            }
            // 记录分销人id
            let uid = scene.uid;
            if (uid > 0) {
                uni.setStorageSync('referee_id', uid);
            }
            // 邀请有礼id
            let invitation_id = query.invitation_id;
            if (invitation_id > 0) {
                uni.setStorageSync('invitation_id', invitation_id);
            }
            // 邀请有礼id
            let invitid = scene.invitid;
            if (invitid > 0) {
                uni.setStorageSync('invitation_id', invitid);
            }
        },
    }
}
</script>

<style>
	@import './common/iconfont.css';
	/*每个页面公共css */
	@import './common/style.css';
</style>

```



### main.js

```js
import Vue from 'vue'
import App from './App'
import directive from './common/directive.js'
import utils from './common/utils.js'
import config from './config.js'
import onfire from './common/onfire.js'
import { gotopage } from '@/common/gotopage.js'
import mpNavigationBar from "./components/navigation-bar/navigation-bar";
Vue.component("mp-navigation-bar", mpNavigationBar);
import mpNavigationBar2 from "./components/navigation-bar2/navigation-bar";
Vue.component("mp-navigation-bar2", mpNavigationBar2);
Vue.prototype.$fire = new onfire()

Vue.config.productionTip = false

App.mpType = 'app'

Vue.prototype.config = config

const app = new Vue({
	...App
})
app.$mount()

Vue.prototype.websiteUrl = config.app_url;
Vue.prototype.app_id = config.app_id;
Vue.prototype.app_name = config.app_name;
Vue.prototype.wxapp = config.wxapp;

Vue.prototype.default_store_id = config.default_store_id;
//h5发布路径
Vue.prototype.h5_addr = config.h5_addr;
/*页面跳转*/
Vue.prototype.gotoPage = gotopage;
console.log(config,Vue.prototype)

//#ifdef H5
app.$router.afterEach((to, from) => {
	const u = navigator.userAgent.toLowerCase()
	if (u.indexOf("like mac os x") < 0 || u.match(/MicroMessenger/i) != 'micromessenger') return
	if (to.path !== global.location.pathname) {
		location.assign(config.h5_addr + to.fullPath);
	}
})
//#endif


//是否是ios
Vue.prototype.ios = function() {
	const u = navigator.userAgent.toLowerCase();
	if (u.indexOf("like mac os x") < 0 || u.match(/MicroMessenger/i) != 'micromessenger') {
		return false;
	}
	return true;
};

//get请求
Vue.prototype._get = function(path, data, success, fail, complete) {
	data = data || {};
	data.token = uni.getStorageSync('token') || '';
	data.app_id = this.getAppId();
	data.wxapp = this.wxapp;
	if(config.env != 'prod'){
		console.log('GET',this.websiteUrl + '/index.php/api/' + path,data)
	}
	uni.request({
		url: this.websiteUrl + '/index.php/api/' + path,
		data: data,
		dataType: 'json',
		method: 'GET',
		success: (res) => {
			if (res.statusCode !== 200 || typeof res.data !== 'object') {
				return false;
			}
			if (res.data.code === -1) {
				// 登录态失效, 重新登录
				console.log('登录态失效, 重新登录');
				this.doLogin();
			} else if (res.data.code === 0) {
				this.showError(res.data.msg, function() {
					fail && fail(res);
				});
				return false;
			} else {
				success && success(res.data);
			}
		},
		fail: (res) => {
			fail && fail(res);
		},
		complete: (res) => {
			uni.hideLoading();
			complete && complete(res);
		},
	});
};

//post请求
Vue.prototype._post = function(path, data, success, fail, complete) {
	data = data || {};
	data.token = uni.getStorageSync('token') || '';
	data.app_id = this.getAppId();
	data.wxapp = this.wxapp;
	if(config.env != 'prod'){
		console.log('POST',this.websiteUrl + '/index.php/api/' + path,data)
	}
	uni.request({
		url: this.websiteUrl + '/index.php/api/' + path,
		data: data,
		dataType: 'json',
		method: 'POST',
		header: {
			'content-type': 'application/x-www-form-urlencoded',
		},
		success: (res) => {
			uni.hideLoading();
			if (res.statusCode !== 200 || typeof res.data !== 'object') {
				return false;
			}
			if (res.data.code === -1) {
				// 登录态失效, 重新登录
				console.log('登录态失效, 重新登录');
				this.doLogin();
			} else if (res.data.code === 0) {
				this.showError(res.data.msg, function() {
					fail && fail(res);
				});
				return false;
			} else {
				success && success(res.data);
			}
		},
		fail: (res) => {
			uni.hideLoading();
			fail && fail(res);
		},
		complete: (res) => {
			
			complete && complete(res);
		},
	});
};
Vue.prototype.isGotPhone = function() {
	if(uni.getStorageSync('userInfo') && uni.getStorageSync('userInfo')){
		return true
	}else{
		uni.showToast({
			title:'请先授权手机号',
			icon:'none',
			mask:true
		})
		setTimeout(()=>{
			uni.switchTab({
				url: "/pages/user/index/index"
			});
		},1500)
		return false
	}
};
Vue.prototype.doLogin = function() {
	let pages = getCurrentPages();
	if (pages.length) {
		let currentPage = pages[pages.length - 1];
		if ("pages/login/login" != currentPage.route) {
			uni.setStorageSync("currentPage", currentPage.route);
			uni.setStorageSync("currentPageOptions", currentPage.options);
		}
	}
	//公众号
	// #ifdef  H5
	if(this.isWeixn()){
		let invitation_id = uni.getStorageSync('invitation_id')?uni.getStorageSync('invitation_id'):0;
		window.location.href = this.websiteUrl + '/index.php/api/user.usermp/login?app_id=' + this.getAppId() +
			'&referee_id=' + uni.getStorageSync('referee_id') + '&invitation_id=' + invitation_id;
	}else{
		uni.navigateTo({
			url: "/pages/login/weblogin"
		});
	}
	// #endif
	// #ifdef APP-PLUS
	uni.redirectTo({
		url: "/pages/login/openlogin"
	});
	return;
	// #endif
	// 非公众号,跳转授权页面
	// #ifndef  H5
	uni.navigateTo({
		url: "/pages/login/login"
	});
	// #endif
};


/**
 * 显示失败提示框
 */
Vue.prototype.showError = function(msg, callback) {
	uni.showModal({
		title: '友情提示',
		content: msg,
		showCancel: false,
		success: function(res) {
			callback && callback();
		}
	});
};

/**
 * 显示失败提示框
 */
Vue.prototype.showSuccess = function(msg, callback) {
	uni.showModal({
		title: '友情提示',
		content: msg,
		showCancel: false,
		success: function(res) {
			callback && callback();
		}
	});
};

/**
 * 获取应用ID
 */
Vue.prototype.getAppId = function() {
	return this.app_id || 10001;
};

Vue.prototype.compareVersion = function(v1, v2) {
	v1 = v1.split('.')
	v2 = v2.split('.')
	const len = Math.max(v1.length, v2.length)

	while (v1.length < len) {
		v1.push('0')
	}
	while (v2.length < len) {
		v2.push('0')
	}

	for (let i = 0; i < len; i++) {
		const num1 = parseInt(v1[i])
		const num2 = parseInt(v2[i])

		if (num1 > num2) {
			return 1
		} else if (num1 < num2) {
			return -1
		}
	}

	return 0
};

/**
 * 获取应用ID
 */
Vue.prototype.getAppId = function() {
	return this.app_id || 10001;
};


/**
 * 生成转发的url参数
 */
Vue.prototype.getShareUrlParams = function(params) {
	let self = this;
	return utils.urlEncode(Object.assign({
		referee_id: self.getUserId()
	}, params));
};

/**
 * 当前用户id
 */
Vue.prototype.getUserId = function() {
	return uni.getStorageSync('user_id');
};

//#ifdef H5
var jweixin = require('jweixin-module');

Vue.prototype.configWx = function(signPackage, shareParams, params) {
	if (signPackage == '') {
		return;
	}
	let self = this;
	jweixin.config(JSON.parse(signPackage));

	let url_params = self.getShareUrlParams(params);

	jweixin.ready(function(res) {
		jweixin.updateAppMessageShareData({
			title: shareParams.title,
			desc: shareParams.desc,
			link: self.websiteUrl + self.h5_addr + shareParams.link + '?' + url_params,
			imgUrl: shareParams.imgUrl,
			success: function() {

			}
		});
		jweixin.updateTimelineShareData({
			title: shareParams.title,
			desc: shareParams.desc,
			link: self.websiteUrl + self.h5_addr + shareParams.link + '?' + url_params,
			imgUrl: shareParams.imgUrl,
			success: function() {

			}
		});
	});
};
//#endif

/**
 * 获取当前平台
 */
Vue.prototype.getPlatform = function(params) {
	let platform = 'wx';
	// #ifdef  H5
	if(this.isWeixn()){
		platform = 'mp';
	}else{
		platform = 'h5';
	}
	// #endif
	return platform;
};

/**
 * 订阅通知,目前仅小程序
 */
Vue.prototype.subMessage = function(temlIds, callback) {
	let self = this;
	// #ifdef  MP-WEIXIN
	//小程序订阅消息
	const version = wx.getSystemInfoSync().SDKVersion;
	console.log(temlIds,version)
	if (temlIds && temlIds.length != 0 && self.compareVersion(version, '2.8.2') >= 0) {
		uni.hideLoading()
		wx.requestSubscribeMessage({
			tmplIds: temlIds,
			success(res) {},
				fail(res) {},
			complete(res) {
				callback();
			},
		});
	} else {
		callback();
	}
	// #endif
	// #ifndef MP-WEIXIN
	callback();
	// #endif
};

Vue.prototype.isWeixn = function(){
    var ua = navigator.userAgent.toLowerCase();
    if(ua.match(/MicroMessenger/i)=="micromessenger") {
        return true;
    } else {
        return false;
    }
};
```



### config.js

```js
//var app_url = 'http://www.jjj-shop.com';
let extConfig = wx.getExtConfigSync? wx.getExtConfigSync(): {}
var app_url = extConfig.siteinfo[extConfig.env];
// if(process.env.NODE_ENV === 'development'){
//     //#ifdef H5
// 	app_url = '';
// 	//#endif
// }

export default {
	/*服务器地址*/
	app_url: app_url,
	/*appid*/
	env:extConfig.env,
	wxapp:extConfig.wxapp,
	app_id: extConfig.app_id,
	app_name: extConfig.app_name,
	default_store_id:extConfig.default_store_id,
	//h5发布路径
	h5_addr: '/h5',
} 
```

### ext.json

```json
{
  "extEnable": true,
  "extAppid": "wxc69adf814fe9ee5d",
  "ext":{
	"wxapp":"client",
    "app_id":10002,
	"app_name":"善语",
	"default_store_id":10010,
	"env":"test",
	"siteinfo":{
		"test":"http://sasstest.minstech.cn",
		"prod":"https://sass.minstech.cn",
		"dev":"https://sass.minstech.cn"
	}
  }
}
```

