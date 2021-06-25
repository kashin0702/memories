### 全局变量配置

1.配置environment环境

2.环境中配置api_root域名， 在请求中用{{api_root}}表示全局变量

配置token等动态变量

login请求的tests内编写脚本

```js
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
 
// 把responseBody转为js字符串
var data = JSON.parse(responseBody);
// 设置全局变量或者环境变量，供后面的接口引用
pm.globals.set("sessionId", data.data.info.sessionId);
pm.globals.set("authKey", data.data.info.authKey);
pm.globals.set("token", data.data.info.token);

pm.environment.set("sessionId", data.data.info.sessionId);
pm.environment.set("authKey", data.data.info.authKey);
pm.environment.set("token", data.data.info.token);
```



