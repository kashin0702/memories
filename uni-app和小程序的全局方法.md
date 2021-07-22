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



## 27 -原生小程序配置文件和全局方法

### siteInfo.js

```js
//上线后修改 这个ENV的值就行了 一般为dev（本地调试） 或test（测试环境） 或 prod（生产环境）
//上线之前 修改下面三个常量 其他不用修改
const ENV = "test";
const VERSION = '1.1.0';
const UPTATE_TIME = '2020-09-24';
const UNIACID = '10001' //集团id
const WXAPP_ID = '10001' //具体门店id 
// 10001 wxa8d3ec604f01906a   27
// 20001 wx09e94933c9b6efd7   金泰兰
const NAME = 'Hello27用户端'
const WSS_URL = 'wss://mp3wss.minstech.cn/wss'
// const WSS_URL = 'ws://127.0.0.1:7001'
let siteinfo = {}
if (ENV == 'prod') {
  siteinfo = {
    'api_root': 'https://mp3.minstech.cn/ex/php/web/',
    'version': VERSION,
  }
} else if (ENV == 'test') {
  siteinfo = {
    'api_root': 'https://mp3test.minstech.cn/ex/php/web/',
    'version': "测试环境" + VERSION,
  }
} else {
  siteinfo = {
    'api_root': 'http://ex.cn/',
    'version': "开发环境" + VERSION,
  }
}
siteinfo.wss_url = WSS_URL
siteinfo.update_time = UPTATE_TIME
siteinfo.uniacid = UNIACID
siteinfo.imageUrl ='https://minstech-catering.oss-cn-hangzhou.aliyuncs.com/10002/img/'
siteinfo.wxapp_id=WXAPP_ID;
module.exports = siteinfo;

```

### app.js

```js

/**
 * tabBar页面路径列表 (用于链接跳转时判断)
 * tabBarLinks为常量, 无需修改
 */

const tabBarLinks = [
  'pages/index/index',
  'pages/category/index',
  'pages/flow/index',
  'pages/user/index'
];

// 站点配置文件
import siteinfo from './siteinfo.js';
var wss_url = siteinfo.wss_url
// 工具类
import util from './utils/util.js';
let lastRequestInfo = {
  lastClickTime: 0,
  lastUrl: ''
}
const log = require('./utils/log.js');
App({
  log: log,

  /**
   * 全局变量
   */
  util,
  globalData: {
    user_id: null,
  },

  // api地址
  api_root: siteinfo.api_root + 'index.php?s=/api/',

  /**
   * 生命周期函数--监听小程序初始化
   */
  onLaunch(e) {
    let _this = this;
    // 小程序主动更新
    _this.updateManager();
    // 小程序启动场景
    _this.onStartupScene(e.query);
    _this.setNavBar();

  },
  getShopId(){
    return wx.getStorageSync('shop_id')
  },
    //贝塞尔曲线动画
    bezier: function (points, times) {
      console.log(points)
      // 0、以3个控制点为例，点A,B,C,AB上设置点D,BC上设置点E,DE连线上设置点F,则最终的贝塞尔曲线是点F的坐标轨迹。
      // 1、计算相邻控制点间距。
      // 2、根据完成时间,计算每次执行时D在AB方向上移动的距离，E在BC方向上移动的距离。
      // 3、时间每递增100ms，则D,E在指定方向上发生位移, F在DE上的位移则可通过AD/AB = DF/DE得出。
      // 4、根据DE的正余弦值和DE的值计算出F的坐标。
      // 邻控制AB点间距
      var bezier_points = [];
      var points_D = [];
      var points_E = [];
      const DIST_AB = Math.sqrt(Math.pow(points[1]['x'] - points[0]['x'], 2) + Math.pow(points[1]['y'] - points[0]['y'], 2));
      // 邻控制BC点间距
      const DIST_BC = Math.sqrt(Math.pow(points[2]['x'] - points[1]['x'], 2) + Math.pow(points[2]['y'] - points[1]['y'], 2));
      // D每次在AB方向上移动的距离
      const EACH_MOVE_AD = DIST_AB / times;
      // E每次在BC方向上移动的距离 
      const EACH_MOVE_BE = DIST_BC / times;
      // 点AB的正切
      const TAN_AB = (points[1]['y'] - points[0]['y']) / (points[1]['x'] - points[0]['x']);
      // 点BC的正切
      const TAN_BC = (points[2]['y'] - points[1]['y']) / (points[2]['x'] - points[1]['x']);
      // 点AB的弧度值
      const RADIUS_AB = Math.atan(TAN_AB);
      // 点BC的弧度值
      const RADIUS_BC = Math.atan(TAN_BC);
      // 每次执行
      for (var i = 1; i <= times; i++) {
        // AD的距离
        var dist_AD = EACH_MOVE_AD * i;
        // BE的距离
        var dist_BE = EACH_MOVE_BE * i;
        // D点的坐标
        var point_D = {};
        point_D['x'] = dist_AD * Math.cos(RADIUS_AB) + points[0]['x'];
        point_D['y'] = dist_AD * Math.sin(RADIUS_AB) + points[0]['y'];
        points_D.push(point_D);
        // E点的坐标
        var point_E = {};
        point_E['x'] = dist_BE * Math.cos(RADIUS_BC) + points[1]['x'];
        point_E['y'] = dist_BE * Math.sin(RADIUS_BC) + points[1]['y'];
        points_E.push(point_E);
        // 此时线段DE的正切值
        var tan_DE = (point_E['y'] - point_D['y']) / (point_E['x'] - point_D['x']);
        // tan_DE的弧度值
        var radius_DE = Math.atan(tan_DE);
        // 地市DE的间距
        var dist_DE = Math.sqrt(Math.pow((point_E['x'] - point_D['x']), 2) + Math.pow((point_E['y'] - point_D['y']), 2));
        // 此时DF的距离
        var dist_DF = (dist_AD / DIST_AB) * dist_DE;
        // 此时DF点的坐标
        var point_F = {};
        point_F['x'] = dist_DF * Math.cos(radius_DE) + point_D['x'];
        point_F['y'] = dist_DF * Math.sin(radius_DE) + point_D['y'];
        bezier_points.push(point_F);
      }
      return {
        'bezier_points': bezier_points
      };
    },
  /**
   * 小程序启动场景
   */
  onStartupScene(query) {
    // 获取场景值
    let scene = this.getSceneData(query);
    // 记录推荐人id
    let refereeId = query.referee_id ? query.referee_id : scene.uid;
    refereeId > 0 && (this.saveRefereeId(refereeId));
  },

  /**
   * 获取商城ID
   */
  getWxappId() {
    return wx.getStorageSync('wxapp_id') || siteinfo.wxapp_id;
  },
  
  sockectReConnect(currentSocket) {
    console.info("进入了重连");
    console.info(currentSocket);
    if (!currentSocket) {
      this.sockectInitConnect();
      return;
    }
    if (currentSocket.readyState !== 0 && currentSocket.readyState !== 1) {
      this.sockectInitConnect();
    }
  },
  sockectInitConnect() {
    let currentSocket;
    const _this = this;
    currentSocket = wx.connectSocket({
      url: wss_url,
      success: function (res) {
        console.log(res, 'successInit')
      },
      fail: function (err) {
        console.log(err, 'failInit')
      }
    })
    wx.onSocketMessage(function (res) {
      console.log(res, 'socketInfo')
      let data = JSON.parse(res.data)
      if(data.client_id){
        _this._get('user/bindClient', {
          client_id:data.client_id
        }, function (result) {
          console.log(result, 'bindClient')
        })
      }
    },)
    this.globalData.currentSocket = currentSocket
  },
  socketAddGroup() {
    let _this = this;
    var send = {
      type: 'group',
      group_id: 'ex_' + _this.getWxappId() + '_' + _this.getShopId() + '_seat_' + wx.getStorageSync("seat_id"),
      msg: 'add group'
    }
    send = JSON.stringify(send)
    wx.sendSocketMessage({
      data: send,
      complete: function (data) {
        console.log('加入分组', data)
      }
    })
  },
  socketSendGroupMsg() {
    let _this = this;
    var send = {
      type: 'message',
      group_id: 'ex_' + _this.getWxappId() + '_' + _this.getShopId() + '_seat_' + wx.getStorageSync("seat_id"),
      msg: 'change shopCar'
    }
    send = JSON.stringify(send)
    wx.sendSocketMessage({
      data: send,
      complete: function (data) {
        console.log(data)
      }
    })
  },
  socketSendServerGroup() {
    let _this = this;
    var send = {
      type: 'message',
      group_id:  'ex_' + 'server_11',
      msg: 'change order'
    }
    send = JSON.stringify(send)
    wx.sendSocketMessage({
      data: send,
      complete: function (data) {
        console.log(data)
      }
    })
  },
  setNavBar: function () {
    wx.getSystemInfo({
      success: (res) => {
        console.log(res, wx.getMenuButtonBoundingClientRect())
        let totalTopHeight = wx.getMenuButtonBoundingClientRect().bottom + wx.getMenuButtonBoundingClientRect().top;
        this.globalData.statusBarHeight = res.statusBarHeight;
        this.globalData.titleBarHeight = totalTopHeight - res.statusBarHeight * 2;
      },
      fail: () => {
        this.globalData.statusBarHeight = 20
        this.globalData.titleBarHeight = 44
      }
    })
  },
  /**
   * 记录推荐人id
   */
  saveRefereeId(refereeId) {
    if (!wx.getStorageSync('referee_id'))
      wx.setStorageSync('referee_id', refereeId);
  },

  /**
   * 获取场景值(scene)
   */
  getSceneData(query) {
    return query.scene ? util.scene_decode(query.scene) : {};
  },

  /**
   * 当小程序启动，或从后台进入前台显示，会触发 onShow
   */
  onShow(options) {
    // 获取小程序基础信息
    // this.getWxappBase();
  },

  /**
   * 执行用户登录
   */
  doLogin(delta) {
    // 保存当前页面
    let pages = getCurrentPages();
    if (pages.length) {
      let currentPage = pages[pages.length - 1];
      "pages/login/login" != currentPage.route &&
        wx.setStorageSync("currentPage", currentPage);
    }
    // 跳转授权页面
    wx.navigateTo({
      url: "/pages/login/login?delta=" + (delta || 1)
    });
  },

  /**
   * 当前用户id
   */
  getUserId() {
    return wx.getStorageSync('user_id');
  },

  /**
   * 显示成功提示框
   */
  showSuccess(msg, callback) {
    wx.showToast({
      title: msg,
      icon: 'success',
      mask: true,
      duration: 1500,
      success() {
        callback && (setTimeout(function() {
          callback();
        }, 1500));
      }
    });
  },

  /**
   * 显示失败提示框
   */
  showError(msg, callback) {
    wx.showModal({
      title: '友情提示',
      content: msg,
      showCancel: false,
      success(res) {
        // callback && (setTimeout(function() {
        //   callback();
        // }, 1500));
        callback && callback();
      }
    });
  },

  /**
   * get请求
   */
  _get(url, data, success, fail, complete, check_login) {
    wx.showNavigationBarLoading();
    let _this = this;
    // 构造请求参数
    data = data || {};
    data.wxapp_id = _this.getWxappId();

    // if (typeof check_login === 'undefined')
    //   check_login = true;

    // 构造get请求
    let request = function() {
      data.token = wx.getStorageSync('token');
      wx.request({
        url: _this.api_root + url,
        header: {
          'content-type': 'application/json'
        },
        data: data,
        success(res) {
          if (res.statusCode !== 200 || typeof res.data !== 'object') {
            console.log(res);
            if(url == 'user/bindClient'){
              _this.showError('服务器异常，请联系管理员');
            }else{
              _this.showError('网络请求出错');
            }
           
            return false;
          }
          if (res.data.code === -1) {
            // 登录态失效, 重新登录
            wx.hideNavigationBarLoading();
            _this.doLogin(2);
            //排除用户不存在的弹窗提示
          } else if (res.data.code === 0 && res.data.msg.indexOf('用户不存在') == -1) {
            _this.showError(res.data.msg, function() {
              fail && fail(res);
            });
            return false;
          } else {
            console.log(res)
            success && success(res.data);
          }
        },
        fail(res) {
          _this.showError(res.errMsg, function() {
            fail && fail(res);
          });
        },
        complete(res) {
          wx.hideNavigationBarLoading();
          complete && complete(res);
        },
      });
    };
    // 判断是否需要验证登录
    check_login ? _this.doLogin(request) : request();
  },

  /**
   * post提交
   */
  _post_form(url, data, success, fail, complete, isShowNavBarLoading) {
    let _this = this;

    isShowNavBarLoading || true;
    data.wxapp_id = _this.getWxappId();
    data.token = wx.getStorageSync('token');

    // 在当前页面显示导航条加载动画
    if (isShowNavBarLoading == true) {
      wx.showNavigationBarLoading();
    }
    wx.request({
      url: _this.api_root + url,
      header: {
        'content-type': 'application/x-www-form-urlencoded',
      },
      method: 'POST',
      data: data,
      success(res) {
        if (res.statusCode !== 200 || typeof res.data !== 'object') {
          _this.showError('网络请求出错');
          return false;
        }
        if (res.data.code === -1) {
          // 登录态失效, 重新登录
          wx.hideNavigationBarLoading();
          _this.doLogin(1);
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
        wx.hideNavigationBarLoading();
        // wx.hideLoading();
        complete && complete(res);
      }
    });
  },

  /**
   * 验证是否存在user_info
   */
  validateUserInfo() {
    let user_info = wx.getStorageSync('user_info');
    return !!wx.getStorageSync('user_info');
  },

  /**
   * 小程序主动更新
   */
  updateManager() {
    if (!wx.canIUse('getUpdateManager')) {
      return false;
    }
    const updateManager = wx.getUpdateManager();
    updateManager.onCheckForUpdate(function(res) {
      // 请求完新版本信息的回调
      // console.log(res.hasUpdate)
    });
    updateManager.onUpdateReady(function() {
      wx.showModal({
        title: '更新提示',
        content: '新版本已经准备好，即将重启应用',
        showCancel: false,
        success(res) {
          if (res.confirm) {
            // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
            updateManager.applyUpdate()
          }
        }
      });
    });
    updateManager.onUpdateFailed(function() {
      // 新的版本下载失败
      wx.showModal({
        title: '更新提示',
        content: '新版本下载失败',
        showCancel: false
      })
    });
  },

  /**
   * 获取tabBar页面路径列表
   */
  getTabBarLinks() {
    return tabBarLinks;
  },

  /**
   * 跳转到指定页面
   * 支持tabBar页面
   */
  navigationTo(url) {
    console.log(url)
    if (!url || url.length == 0) {
      return false;
    }
    let tabBarLinks = this.getTabBarLinks();
    // tabBar页面
    if (tabBarLinks.indexOf(url) > -1) {
      wx.switchTab({
        url: '/' + url
      });
    } else {
      // 普通页面
      wx.navigateTo({
        url: '/' + url
      });
    }
  },

  /**
   * 记录formId
   * (因微信模板消息已下线，所以formId取消不再收集)
   */
  saveFormId(formId) {
    return true;
    // let _this = this;
    // console.log('saveFormId');
    // if (formId === 'the formId is a mock one') {
    //   return false;
    // }
    // _this._post_form('wxapp.formId/save', {
    //   formId: formId
    // }, null, null, null, false);
  },

  /**
   * 生成转发的url参数
   */
  getShareUrlParams(params) {
    let _this = this;
    return util.urlEncode(Object.assign({
      referee_id: _this.getUserId()
    }, params));
  },

  /**
   * 发起微信支付
   */
  wxPayment(option) {
    let options = Object.assign({
      payment: {},
      success: () => {},
      fail: () => {},
      complete: () => {},
    }, option);
    wx.requestPayment({
      timeStamp: options.payment.timeStamp,
      nonceStr: options.payment.nonceStr,
      package: 'prepay_id=' + options.payment.prepay_id,
      signType: 'MD5',
      paySign: options.payment.paySign,
      success(res) {
        options.success(res);
      },
      fail(res) {
        options.fail(res);
      },
      complete(res) {
        options.complete(res);
      }
    });
  },

  /**
   * 验证登录
   */
  checkIsLogin() {
    return wx.getStorageSync('token') != '' && wx.getStorageSync('user_id') != '';
  },

  /**
   * 授权登录
   */
  getUserInfo(e, callback) {
    let App = this;
    if (e.detail.errMsg !== 'getUserInfo:ok') {
      return false;
    }
    console.log(e)
    wx.showLoading({
      title: "正在登录",
      mask: true
    });
    // 执行微信登录
    wx.login({
      success(res) {
        // 发送用户信息
        App._post_form('user/login', {
          code: res.code,
          user_info: e.detail.rawData,
          encrypted_data: e.detail.encryptedData,
          iv: e.detail.iv,
          signature: e.detail.signature,
          referee_id: wx.getStorageSync('referee_id')
        }, result => {
          // 记录token user_id
          wx.setStorageSync('token', result.data.token);
          wx.setStorageSync('user_id', result.data.user_id);
          // 执行回调函数
          callback && callback(result);
        }, false, () => {
          wx.hideLoading();
        });
      }
    });
  },
 /**
   * 上传文件
   */
  _upFile(url, data,formData, success, fail, complete, isShowNavBarLoading) {
    let _this = this;
    let header;
    isShowNavBarLoading || true;
    let authKey = wx.getStorageSync('authKey');
    let sessionId = wx.getStorageSync('sessionId');
    header = {
      'content-type': 'application/x-www-form-urlencoded',
      'authKey': authKey,
      'sessionId': sessionId,
    }
    // 在当前页面显示导航条加载动画
    if (isShowNavBarLoading == true) {
      wx.showNavigationBarLoading();
    }
    wx.uploadFile({
      url: _this.api_root + url,
      header: header,
      filePath: data,
      name: 'iFile',
      formData:formData,
      success(res) {
        res.data = JSON.parse(res.data);
        if (res.statusCode !== 200 || typeof res.data !== 'object') {
          console.log(res, '网络请求出错')
          _this.showError('网络请求出错');
          return false;
        }
        if (res.data.code === -1) {
          // 登录态失效, 重新登录
          wx.hideNavigationBarLoading();
          _this.doLogin();
          return false;
        } else if (res.data.code === 0) {
          _this.showError(res.data.msg, function () {
            fail && fail(res);
          });
          return false;
        }
        success && success(res.data);
      },
      fail(res) {
        // console.log(res);
        _this.showError(res.errMsg, function () {
          fail && fail(res);
        });
      },
      complete(res) {
        wx.hideNavigationBarLoading();
        // wx.hideLoading();
        complete && complete(res);
      }
    });
  },
  replaceDetail (details) {

    var texts = '';//待拼接的内容

    while (details.indexOf('<img') != -1) {//寻找img 循环

      texts += details.substring('0', details.indexOf('<img') + 4);//截取到<img前面的内容

      details = details.substring(details.indexOf('<img') + 4);//<img 后面的内容

      if (details.indexOf('style=') != -1 && details.indexOf('style=') < details.indexOf('>')) {

        texts += details.substring(0, details.indexOf('style="') + 7) + "max-width:100%;height:auto;margin:0 auto;";//从 <img 后面的内容 截取到style= 加上自己要加的内容

        details = details.substring(details.indexOf('style="') + 7); //style后面的内容拼接

      } else {

        texts += ' style="max-width:100%;height:auto;margin:0 auto;" ';

      }



    }

    texts += details;//最后拼接的内容

    return texts

  },
      //时分
      timestampToTime: function (timestamp) {
        var date = new Date(timestamp * 1000); //时间戳为10位需*1000，时间戳为13位的话不需乘1000
  
        var Y = date.getFullYear() + '-';
        var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
        var D = date.getDate() < 10 ? '0' + date.getDate() + ' ' : date.getDate() + ' '; // var h = date.getHours() + ':';
  
        var h = date.getHours() < 10 ? '0' + date.getHours() + ':' : date.getHours() + ':'; // var m = date.getMinutes() + ':';
  
        var m = date.getMinutes() < 10 ? '0' + date.getMinutes() + ':' : date.getMinutes() + ':';
        var s = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds();
        return Y + M + D + h + m + s;
      },
      isFastClick: function(url, interval = 5000) {
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
});
```

### utils.js

```js
/**
 * 工具类
 */
module.exports = {

  /**
   * scene解码
   //{scene: "aid%3A10013%2Cwxapp_id%3A10001"}
   */
  scene_decode(e) {
    if (e === undefined)
      return {};
    let scene = decodeURIComponent(e),
      params = scene.split(','), //["aid:10013", "wxapp_id:10001"]
      data = {};
    for (let i in params) {
      var val = params[i].split(':'); 
        //val[0] = ['aid', 10013] 
        //val[1] = ['wxapp',10001]
      val.length > 0 && val[0] && (data[val[0]] = val[1] || null)
    }
    return data;
  },

  /**
   * 格式化日期格式 (用于兼容ios Date对象)
   */
  format_date(time) {
    // 将xxxx-xx-xx的时间格式，转换为 xxxx/xx/xx的格式 
    return time.replace(/\-/g, "/");
  },

  /**
   * 对象转URL
   */
  urlEncode(data) {
    var _result = [];
    for (var key in data) {
      var value = data[key];
      if (value.constructor == Array) {
        value.forEach(_value => {
          _result.push(key + "=" + _value);
        });
      } else {
        _result.push(key + '=' + value);
      }
    }
    return _result.join('&');
  },

  /**
   * 遍历对象
   */
  objForEach(obj, callback) {
    Object.keys(obj).forEach((key) => {
      callback(obj[key], key);
    });
  },

  /**
   * 是否在数组内
   */
  inArray(search, array) {
    for (var i in array) {
      if (array[i] == search) {
        return true;
      }
    }
    return false;
  },

  /**
   * 判断是否为正整数
   */
  isPositiveInteger(value) {
    return /(^[0-9]\d*$)/.test(value);
  },

  /**
   * 对Date的扩展，将 Date 转化为指定格式的String
   * 月(Y)、月(m)、日(d)、小时(H)、分(M)、秒(S) 可以用 1-2 个占位符，
   * 例子：
   * dateFormat('YYYY-mm-dd HH:MM:SS', new Date()) ==> 2020-01-01 08:00:00
   */
  dateFormat(fmt, date) {
    const opt = {
      "Y+": date.getFullYear().toString(), // 年
      "m+": (date.getMonth() + 1).toString(), // 月
      "d+": date.getDate().toString(), // 日
      "H+": date.getHours().toString(), // 时
      "M+": date.getMinutes().toString(), // 分
      "S+": date.getSeconds().toString() // 秒
      // 有其他格式化字符需求可以继续添加，必须转化成字符串
    };
    let ret;
    for (let k in opt) {
      ret = new RegExp("(" + k + ")").exec(fmt);
      if (ret) {
        fmt = fmt.replace(ret[1], (ret[1].length == 1) ? (opt[k]) : (opt[k].padStart(ret[1].length, "0")))
      };
    };
    return fmt;
  },

};
```



