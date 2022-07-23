jsonP本身就是一个get请求，而script节点本身也是一个get请求，这个思想是通过后端的配合（后端输出的 response text必须符合js语法）更好的利用了get请求而已。  

 而前端封装一个方法，通过这个方法把请求注册的回调指向全局的一个具名函数，同时把具名函数的函数名和参数通过get请求传递给后端而已。



**前端**

```html
<!--通过src发送请求-->
<script>
    //提前定义要执行的函数 通过<script>标签回调执行指向的这个函数
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



