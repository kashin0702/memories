**前端**

```html
<!--通过src发送请求-->
<script>
    //提前定义要执行的函数
    function getData(data){
        const res = document.getElementById('result')
        res.innerHTML = data.name
    }
    //点击时触发，创建script发送请求给后端
    xx.onClick = function(){
        let script = document.creatElement('script')
        script.src = "http://xxxx/jsonp-server"
        document.body.append(script)
    }
</script>
```

**后端**

```js
app.all('/jsonp-server',(request,response) => {
    const data = {name: 'david'}
    let str = JSON.stringify(data)
    //后端返回前端函数的调用，就是jsonp的实现原理
    response.end(`getData(${data})`) 
})
```



