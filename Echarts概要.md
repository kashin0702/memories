### 安装

npm install echarts --save



### 引入

```js
// 因为echarts导出使用了export {}  所以要使用*导入全部
import * as echarts from 'echarts'
```



### 使用

初始化echarts对象，并设置配置进行绘制

**记得绑定DOM要设置宽高！**

```js

setup(){
    // 获取dom
    const divRef = ref<HTMLElement>()
    onMounted(() => {
        // 1.基于dom创建echarts实例
        const echartsInstance = echarts.init(divRef.value)
        // 2. 设置配置对象
        const options = {
             xAxis: {
                type: 'category',
                data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  data: [150, 230, 224, 218, 135, 147, 260],
                  type: 'line'
                }
              ]
        }
        // 3. 执行setOption方法绘制图像
        options && echartsInstance.setOption(options);
    })
}

```

