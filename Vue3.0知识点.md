#### setup

setup内不可使用**this**

使用组合式API的位置被称为setup 

```js
/* setup 选项是一个接收 props 和 context 的函数，我们将 setup 返回的所有内容都暴露给组件的其余部分 (计算属性、方法、生命周期钩子等等) 以及组件的模板 */

// src/components/UserRepositories.vue
// 第一步， setup基础用法
export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    console.log(props) // { user: '' }

    return {} // 这里返回的任何内容都可以用于组件的其余部分
  }
  // 组件的“其余部分”
}

// 第二步，接上面的 从假定的外部 API 获取该用户的仓库，并在用户有任何更改时进行刷新
import { fetchUserRepositories } from '@/api/repositories'

// 在我们的组件内
setup (props) {
  let repositories = []
  const getUserRepositories = async () => {
    repositories = await fetchUserRepositories(props.user)
  }

  return {
    repositories,  //注意！ 此处保存的repositories并非响应式，页面不会刷新
    getUserRepositories // 返回的函数与方法的行为相同
  }
}
```

#### setup参数

```js
//context对象包含3个属性{emit,attrs,slots}
//emit:发射自定义事件 
//attrs:所有非prop的attribute  
//slots:父组件传递过来的插槽 render函数使用,较少使用，一般用jsx更多
setup(props, {emit, attrs, slots}){
    
}
```



#### 带ref的响应式变量

将值封装在一个对象中，看似没有必要，但为了保持 JavaScript 中不同数据类型的行为统一，这是必须的。这是因为在 JavaScript 中，`Number` 或 `String` 等基本类型是通过值而非引用传递的：

![img](https://blog.penjee.com/wp-content/uploads/2015/02/pass-by-reference-vs-pass-by-value-animation.gif)

**ref 为我们的值创建了一个响应式引用。在整个组合式 API 中会经常使用引用的概念。**

```js
// 导入ref函数
import { ref } from 'vue'
// ref接收参数并将其包裹在一个带有 value property 的对象中返回，可以使用该 property 访问或更改响应式变量的值：
const counter = ref(0)

console.log(counter) // { value: 0 }
console.log(counter.value) // 0

counter.value++
console.log(counter.value) // 1
```

#### 利用ref()编写响应式setup

```js
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref } from 'vue'


setup (props) {
    // 1.对repositories进行响应式处理
  const repositories = ref([])
  const getUserRepositories = async () => {
    // 2.把返回数据传给ref的value属性
    repositories.value = await fetchUserRepositories(props.user)
  }

  return {
    repositories,  // 此时repositories才是响应式
    getUserRepositories
  }
}
```

#### setup内注册生命周期onMounted

```js
import {fetchUserRepositories} from '@/api/repositories'
// 1.导入onMounted函数
import {ref, onMounted} from 'vue'

setup (props) {
    const repositroies = ref([])
    const getUserRepositories = async () => {
        repositories.value = await fetchUserRepositories(props.user)
    }
    onMounted(getUserRepositories) // 2.在 `mounted` 时调用 `getUserRepositories`
    
    return {
        repositroies,
        getUserRepositories
    }
}
```



#### onUpdated生命周期

```js
setup(){
    onUpdated(() => {
    	// 只要界面发生更新，这里的代码就会执行
	}) 
}

```



#### watch函数-响应式更改

watch接收3个参数

- 一个想要侦听的**响应式引用**或 getter 函数
- 一个回调
- 可选的配置选项

```js
// watch()函数写法
import { ref, watch } from 'vue'

const counter = ref(0)
watch(counter, (newValue, oldValue) => {
  console.log('The new counter value is: ' + counter.value)
})

// 等效老写法
export default {
  data() {
    return {
      counter: 0
    }
  },
  watch: {
    counter(newValue, oldValue) {
      console.log('The new counter value is: ' + this.counter)
    }
  }
}
```

#### setup内导入watch

```js
import {fetchUserRepositories} from '@/api/repositories'
import {ref,onMounted,watch,toRefs} from 'vue'

props: {
    user: {
      type: String,
      required: true
    }
},
setup (props) {
    // 使用'toRefs'创建对prop的'user' property 的响应式引用
    const { user } = toRefs(props)
    const repositories = ref([])
    const getUserRepositories = async () => {
        repositories.value = await fetchUserRepositories(props.user)
    }
    
    onMounted(getUserRepositories)  //在 `mounted` 时调用 `getUserRepositories`
    // 在 user prop 的响应式引用上设置一个侦听器
    watch(user, getUserRepositories)
    
    return {
        repositories,
        getUserRepositories
    }
}
```

#### 独立的computed属性

```js
import { ref, computed } from 'vue'

const counter = ref(0)
const twiceTheCounter = computed(() => counter.value * 2)

counter.value++
console.log(counter.value) // 1
console.log(twiceTheCounter.value) // 2
```

#### setup内导入computed

```js
// src/components/UserRepositories.vue `setup` function

import { fetchUserRepositories } from '@/api/repositories'
import { ref,onMounted,watch,computed,toRefs } from 'vue'

setup(props) {
    // 使用toRefs创建对 props中的 user property的响应式引用
    const { user } = toRefs(props)
    const repositories = ref([])
    const getUserRepositories = async () => {
        repositories.value = await fetchUserRepositories(user.value)
    }
    
    onMounted(getUserRepositories) 
    // 监听user改变， 触发getUserRepositories回调
    watch(user, getUserRepositories)
    
    // 添加搜索功能
    const searchQuery = ref('')
    const repositoriesMatchingSearchQuery = computed(() => {
        return repositories.value.filter(repository => {
            return repository.value.includes(searchQuery.value)
        })
    })
    
    return {
        repositories,
        getUserRepositories,
        searchQuery,
        repositoriesMatchingSearchQuery
    }
}
```



#### 封装setup组合式函数

``` js
// 封装独立的userRepositories函数
// src/composables/useUserRepositories.js
import { fetchUserRepositories } from '@/api/repositories'
import { ref,onMounted,watch} from 'vue'

export default function userRepositories (user) {
    const repositories = ref([])
    const getUserRepositories = async () => {
        repositories.value = await fetchUserRepositories(user.value)
    }
    
    onMounted(getUserRepositories)
    watch(user, getUserRepositories)
    
    return {
        repositories,
        getUserRepositories
    }
}

// 封装搜索功能
// src/composables/useRepositoryNameSearch.js
import { ref, computed } from 'vue'
export default function useRepositoryNameSearch(repositories) {
    const searchQuery = ref('')
    const repositoriesMatchingSearchQuery = computed(() => {
        return repositories.value.filter((repository) => {
            return repository.name.includes(searchQuery.value)
        })
    })
    
    return {
        searchQuery,
        repositoriesMatchingSearchQuery
    }
}
```

#### 使用封装的组合式函数

```js
// src/components/UserRepositories.vue
import { toRefs } from 'vue'
import useUserRepositories from '@/composables/useUserRepositories'
import useRepositoryNameSearch from '@/composables/useRepositoryNameSearch'
import useRepositoryFilters from '@/composables/useRepositoryFilters'

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const { user } = toRefs(props)
	
    // 获取组合函数返回的值
    const { repositories, getUserRepositories } = useUserRepositories(user)
	// 获取组合函数返回的值
    const {
      searchQuery,
      repositoriesMatchingSearchQuery
    } = useRepositoryNameSearch(repositories) // 传入上面获得的repositories

    const {
      filters,
      updateFilters,
      filteredRepositories
    } = useRepositoryFilters(repositoriesMatchingSearchQuery)

    return {
      // 因为我们并不关心未经过滤的仓库
      // 我们可以在 `repositories` 名称下暴露过滤后的结果
      repositories: filteredRepositories,
      getUserRepositories,
      searchQuery,
      filters,
      updateFilters
    }
  }
}
```



## Teleport

vue内置组件，有2个属性

to: 指定将其中的内容移动到的目标元素，可以使用选择器

disabled: 是否禁用teleport功能

 ```html
<!--把button挂载到id='david'的对象上-->
<teleport to="#david">
    <button>按钮</button>
</teleport>
 ```



## 笔记

### defineComponent作用

defineComponent函数，只是对setup函数进行封装，返回options的对象；
defineComponent最重要的是：在TypeScript下，给予了组件 正确的参数类型推断 。

1.defineComponent可以给组件的setup方法准确的参数类型定义.
2.defineComponent 可以接受显式的自定义 props 接口或从属性验证对象中自动推断
3.defineComponent 可以正确适配无 props、数组 props 等形式
4.引入 defineComponent() 以正确推断 setup() 组件的参数类型



### reactive() API

vue2 data中的所有数据其实也是交给了reactive()做了响应式处理

reactiveAPI缺点: **必须传一个对象或数组**，若传基本类型会报警告

```vue
<template>
	<div>{{state.counter}}</div>
</template>
<script>
export default {
	setup(){
		const state = reactive({
            counter: 100
        })
        const increment = () => {
            state.counter++
        }
        return {
            counter,
            increment
        }
	}
}
</script>
```



### ref() API和解包

vue3 新api

```vue
<template>
	<!--此处不用写counter.value也能渲染  模板中 ref会自动解包-->
	<div>{{counter}}</div>
	<!--ref对象嵌套到其他对象中，counter不会自动解包-->
	<div>{{info.counter}}</div>
</template>
<script>
export default {
	setup(){
		const counter = ref(100)
        console.log(counter)  //{value: 100}
        const increment = () => {
            // 逻辑代码中ref不会自动解包，需要.value
            counter.value++
        }
        const info = {counter}
        return {
            counter,
            increment
        }
	}
}
</script>
```



### readOnly()

```vue
<script>
    import {reactive, readonly} from 'vue'
	export default {
    	setup(){
    		const info = reactive({
                name: 'david'
            })
            // 创建一个只读对象，无法被修改
            const readonlyObj = readonly(info)
            
            //执行该方法时报警告，无法修改
            const update = () => {
                info.name = 'xxx'
            }
            return {
                update
            }
    }
}
</script>
```



### toRefs() / toRef()

作用: 将reactive对象中的所有属性都转换成ref

```js
import {reactive,toRefs} from 'vue'
export default {
    setup(){
        const info = reactive({name: 'david',age: 19})
        //  let {name,age} = info 直接解构对象的属性会失去响应式效果
        // 1.toRefs() 把reactive对象内所有属性变成响应式ref对象
        let {name, age} = toRefs(info)
        // 2.toRef() 2个参数，一个reactive对象，一个要转换的属性 
        // 只转换一个指定属性为ref对象
        let name = toRef(info,'name')
    }
    return {
    	name,
    	age
	}
}
```

### watchEffect / watch

watchEffect : 自动收集响应式依赖 会自动执行1次 (自动执行的作用是收集依赖) 

watchEffect有2个参数：1.处理副作用的回调函数  2. flush侦听时机参数

watch:  指定要监听的依赖

watch可以侦听的范围:  data/props/computed

watch只能侦听数据本身的改变，不能侦听数据内部属性的改变(**要使用深度侦听**)

```js
watch: {
    //语法糖写法
    person(newValue,oldValue){
        console.log(newValue,oldValue)
    },
    //配置型写法, 可配置deep immediate
    person: {
        handler: function(newValue,oldValue){
            console.log(newValue,oldValue)
        },
        deep: true,     //深度侦听配置 可监听对象中属性改变
        immediate: true //立即执行
    }
}

// $watch写法 写在生命周期内
created() {
    //$watch有一个返回值，可用于取消侦听
   const unwatch =  this.$watch("person",(newVal,oldVal) => {
        console.log(newVal,oldVal)
    },{
        deep: true,
        immediate: true
    })
   
   // unwatch() 执行就可以取消侦听
}

```



```js
const name = ref('david')
const changeName = () => name.value = 'kashin'
// watchEffect不需要传入对象，内部回调函数会自动收集回调函数内的可响应式对象
watchEffect(() => {
    console.log("name:", name.value) // 不需要传入name，当name对象改变时自动触发
})


// 根据条件停止侦听  watchEffect有一个返回函数
const stop = watchEffect(() => {
    console.log(age.value)
})
if(age.value > 25) {
    stop() //当age>25 调用stop() 停止侦听

//副作用：在监听函数内发送网络请求，当监听对象改变时，会再次发送请求，清除上一次请求就是清除副作用
//watchEffect函数内可以传入一个参数，该参数也是一个回调函数，用于清除监听副作用
const stop = watchEffect((onInvalidate) => {
    onInvalidate(() => {
        //在这个函数内清除额外副作用 如清除网络请求伪代码：request.cancel()
        console.log('onInvalidate'执行)
    })
    console.log(name.value)
})
```

#### watchEffect监听dom中的ref

```vue
<template>
	<div ref="title">哈哈哈</div>
</template>

<script>
	import {ref, watchEffect} from 'vue'
    export default {
        setup(){
            // 定义一个ref(null) 会把ref='title'赋值过来
            const title = ref(null)
            // watchEffect会默认先执行一次，第一次还没挂载title肯定等于null
            // 第二次才会获得值 总共执行2次
            watchEffect(() => {
                console.log(title.value)  // 拿到整个dom: <div>哈哈哈</div>
            },{    
                //此处传入第二个参数post, 侦听会在DOM挂载后执行
                // flush有3个值 'pre'默认值, 'post', 'sync'强制同步很少用
                flush: 'post'  
            })
            
            return {
                title
            }
        }
    }
</script>
```



#### watch()

watch侦听数据源有2种类型

1. 一个getter函数，但是该getter函数必须引用可响应式对象(reactive或ref)
2. 直接写入一个可响应式对象，reactive或ref(常用ref)

```js
const info = reactive({name: 'david',age: 18})
// 情况1 侦听reactive对象的属性，传入一个getter函数 
watch(() => info.name, (newVal,oldVal) => {
    console.log(newVal,newVal)  
})
const changeData = () {
    info.name = 'yyy'  
}
// 
//情况2 侦听整个对象，获取到的newVal和oldVal也是reactive对象
watch(info, (newVal,oldVal) => {
    console.log(newVal,newVal) //Proxy{name: ,age:}对象
})
const changeData = () {
    info.name = 'yyy'  
}

//情况3 侦听ref对象并传入该对象，获取到的newVal和OldVal是一个普通的值
const name = ref('gyf')
watch(name,(newVal,oldVal) => {
    console.log(newVal,newVal)
})

const changeData = () => {
    name.value = 'xxx'
}
// 重点：将reactive对象解构后可，此时侦听返回的就是普通对象
// 通过返回一个解构的info 此时内部值都为普通值
watch(() => ({...info}), (newVal,oldVal) => {
    console.log(newVal,oldVal)
})

//其他知识点
const info = reactive({name: 'david', age: 20})
const name = ref('gyf')

// watch第一个参数也可以传数组,监听多个对象, 返回的newVal oldVal也变成数组,也可以解构
watch([info,name],([newInfo,newName],[oldInfo,oldName]) => {
    console.log(newInfo,newName,oldInfo,oldName)
})
```



### computed本质

```js
//vue2中computed可以配置成一个函数getter或者设置get/set (源码中会判断)
computed: {
    FullName() { //写成函数时默认就是一个get
        //return xxx
    }
}

computed: {
    fullName:{  //写成对象时，配置get/set方法
        get: function(){
            
        },
        set: function(){
            
        }
    }
}

// vue3中以函数形式,写在setup内
import {ref,computed} from 'vue'
setup(){
    const firstName = ref('david')
    const lastName = ref('billy')
    // 函数写法
    const fullName = computed(() => firstName.value + lastName.value)
    // 对象写法
    const fullName2 = computed({
        get: () => firstName.value + lastName.value,
        // changeName修改的值会传入newValue
        set: (newValue) => {
            const name = newValue
        }
    })
    const changeName = () => {
        fullName2.value = 'xxx' // computed是ref对象，要加.value
    }
    return {
        fullName //计算属性也是响应式对象(ref对象)
    }
}
// 通过计算属性修改值
setup(){
    const firstName = ref('david')
    const lastName = ref('billy')
    const fullName = computed({
        // 计算属性的对象形式，传入set,get方法
        get: () => firstName.value + lastName.value,
        // 通过set修改其他值
        set(newVal){
            const names = newVal.split(' ')
            firstName.value = names[0]
            lastName.value = names[1]
        }
    })
    
    const changeName = () => {
        fullName.value = 'kobe brant'
    }
    return {
        fullName //此时计算属性也是响应式对象(ref对象)
    }
}
```



### setup内的生命周期

setup生命周期内没有**beforeCreate**和**beforeCreate**生命周期，且不推荐在这两个生命周期内操作了

setup比**beforeCreate**和**beforeCreate**更早执行，直接在setup内执行即可

```js
// setup内可导入的生命周期函数
import {
    onBeforeMount,onMounted,
    onBeforeUpdate,onUpdate,
    onBeforeUnmount,onUnmounted,
	onActivated, onDeactivated } from 'vue'
```



### provide/inject

```js
// 提供数据的父组件
import { provide, readonly } from 'vue'
export default {
    setup(){
        const name = ref('david')
        // provide传入2个参数，第一个key名称，第二个要传的对象
        //readonly包裹，使子组件无法修改数据，单项数据流开发理念
        provide('name',readonly(name))
    }
}

// 接收数据的子组件 此时子组件就可以拿到name并在dom渲染
import {inject} from 'vue'
export default {
    setup(){
        // inject也有2个参数，第一个要接受的key名称，第二个可选 没有key时设置
        const name = inject('name')
        return {
            name
        }
	}
}
```



### setup内获取route/router

setup内不能使用this去获取当前路由，要用**useRoute**, **userRouter**获取

```js
import { useRoute, useRouter } from 'vue-router'
setup(){
    const route = useRoute()
    const router = useRouter()
    const jumpTo = () => {
        router.push('/about')
    }
}
```



### setup抽取概念

```js
// 定义useCounter.js一个功能模块,包含所有counter功能的相关代码
import { ref, computed} from 'vue'
export function useCounter() {
    const counter = ref(0)
    const doubleCounter = computed(() => counter.value * 2)
    const increment = () => counter.value++
    const decrement = () => counter.value--
    
    return {counter,doubleCounter, increment, decrement}
}

// setup内调用该模块 标准写法
import {userCounter} from './hooks/useCounter.js'
export default {
    setup(){
        // 直接解构获得所有要调用的数据 
        const {counter,doubleCounter,increment,decrement} = useCounter()
    }
    return {counter,doubleCounter, increment, decrement}
}

// 更简洁写法
setup(){
    return {
        ...useCounter() //直接执行函数并解构， 但这种写法阅读性很差
    }
}

```



### 案例：动态修改title

```js
// useTitle.js
import {ref,watch} from 'vue'
export function changeTitle(title='默认标题'){
    const titleRef = ref(title)
    watch(titleRef,(newVal)=> {
        document.title = newVal
    }, {
         //watch第2个参数，表示立即执行一次 因为watch是惰性的，只有变化才会执行
        immediate: true 
    })
    return titleRef
}

// 3秒后修改标题
import {useTitle} from './hooks/useTitle'
setup(){
    // 拿到useTitle模块内返回的titleRef
    const titleRef = useTitle('我是标题')
    setTimeout(() => {
        // 3秒后修改titleRef值 此时watch监听到后就修改了title值
        titleRef.value = '另一个标题'
    },3000)
}
```

### 案例：封装localStorage

```js
// useLocalStorage.js
import {ref, watch} from 'vue'
export default function(key,value){
    const data = ref(value)
    
    if(value){
        // setItem如果要保存的是一个对象或数组，必须用json.stringify序列化保存
        window.localStorage.setItem(key,JSON.stringify(value))
    }else {
        // 获取时也需要用json.parse解析
        data.value = JSON.parse(window.localStorage.getItem(key)) 
    }
    
    watch(data, (newVal) => {
        window.localStorage.setItem(key,JSON.stringify(newVal))
    })
    
    return data
}

// 父组件调用
import {useLocalStorage} from './hooks/useLocalStorage'

setup(){
    // 设置缓存
    const data = useLocalStorage("info",{name: 'david', age: 33})
    // 获取缓存
    const data2 = useLocalStorage("info")
    // 修改缓存
    const changeData = () => data.value = '哈哈哈'
    return {
        data
    }
}
```



###  axios请求失败

两种失败

1.http请求失败

2xx->成功

4xx->失败

5xx->失败

httpErrorCode一般都在responseCatch：err

2.失败(请求成功，但是返回错误码)

请求status:200 但是returnCode返回具体的错误码 如 -10000

{data: [],  returnCode: -1001}



### ts获取组件对象的类型

```typescript
setup(){
    //这样写其实是any类型，有安全隐患
    const accountRef = ref() 
    //标准写法，属于ts语法，对象实例的typeof 组件类型
    const accountRef = ref<InstanceType<typeof LoginAccount>>()
    
    const handler = () => {
        //这样调accountRef组件的方法就不会报错
        accountRef.value?.loginAction() 
    }                                    
                                        
}
```



### script标签内直接使用setup特性

新特性内props传参写法

```vue
<template>
	<div>
        <!--接收props传来的width, height-->
        <div :style="{width: width, height: height}">
        	    
    	</div>     
    </div>
</template>
<script lang="ts" setup>
import {ref, onMounted, defineProps, withDefaults} from 'vue'
//widthDefaults方法接受2个参数，第一个传defineProps函数，第二个传props默认值
//因为是ts,不能直接用js中的String类型， 给defineProps设置一个泛型，设置接受的参数类型
const props = widthDefaults(
    defineProps<{width?: string, height?: string}>(),
    {
        width: '100%',
        height: '360px'
    }
	)
</script>
```



## setup内使用vuex辅助函数

```js
//常规写法 使用useStore和computed获取store内的state
import { computed } from 'vue'
import { useStore } from 'vuex'
export default {
    setup(){
        const store = useStore()
        const sCounter = computed(()=> store.state.counter)
        return {
            sCounter
        }
    }
}

// mapState辅助函数写法
import { computed } from 'vue'
import { mapState, useStore } from 'vuex'
export default {
 	setup(){
        const store = useStore()
        const storeStateFn = mapState(['counter','name', 'age', 'height'])
        //storeStateFn其实是一个这样的对象:
        //{counter: function, name: function, age: function, height: function}
        const storeState = {}
        // 转化后的storeState: {counter: ref, name: ref ...}
        Object.keys(storeStateFn).forEach(Key => {
            const fn = storeStateFn[key].bind({$store: store}) //给方法绑定执行环境$store
            storeState[key] = computed(fn) // 通过computed把fn转成ref对象，保存给storeState
        })
    }   
}
```

### 封装mapState

```js
// 封装hooks/useMapState.js
import { computed } from 'vue'
import { mapState, useStore } from 'vuex'
export function useMapState(mapper){ // mapper就是外部传入的参数
    const store = useStore()
    const storeStateFn = mapState(mapper)
    const storestate = {}
    Object.keys(storeStateFn).forEach(key => {
        const fn = storeStateFn[key].bind({$store: store})
        storeState[key] = computed(fn)
    })
    return storeState
}

// 外部使用hooks
import {useMapState} from './hooks/useMapstate'
setup(){
    // 数组格式
    const storeState = useMapState(['counter', 'name', 'age'])
    // 对象格式
    const storeState2 = useMapState({
        sCounter: state => state.counter
    })
    return {
        ...storeState,
        ...storeState2
    }
}
```

### 封装mapGetters

```js
// 封装hooks/useMapGetters.js
import { computed } from 'vue'
import { mapGetters, useStore } from 'vuex'
export function useMapState(mapper){ // mapper就是外部传入的参数
    const store = useStore()
    const storeGettersFn = mapGetters(mapper)
    const storeGetters = {}
    Object.keys(storeGettersFn).forEach(key => {
        const fn = storeGettersFn[key].bind({$store: store})
        storeGetters[key] = computed(fn)
    })
    return storeGetters
}
```

### 二次封装mapState和mapGetters

mapState和mapGetters逻辑代码基本一样，可通过2次封装到一个useMapper中

```js
// useMapper.js
import {computed} from 'vue'
import {useStore} from 'vuex'
export function useMapper(mapper,mapperFn){
    const store = useStore()
    const storeFn = mapperFn(mapper) // 从外部传入要调用mapState还是mapGetters
    const storeState = {}
    Object.keys(storeFn).forEach(key => {
        const fn = storeFn[key].bind({$store: store})
        storeState[key] = computed(fn)
    })
    return storeState
}

// useState.js
import {useMapper} from 'hooks/useMapper'
import {mapState} from 'vuex'
export function useState(mapper){
    return useMapper(mapper,mapState)
}

// useGetters.js
import {useMapper} from 'hooks/useMapper'
import {mapGetters} from 'vuex'
export function useGetters(mapper){
    return useMapper(mapper, mapGetters)
}
```

### mapMutations

```js
// setup内使用mapMutations和methods内使用无差异，不需要转化
setup(){
    // 也支持2种写法
    const storeMutations = mapMutations(['increment','decrement'])
    const storeMutations2 = mapMutations({
        add: 'increment',
        sub: 'decrement'
    })
    return {
        ...storeMutations,
        ...storeMutations2
    }
}
```

### mapActions

```js
setup(){
    const storeActions = mapActions(['increment','decrement'])
    const storeActions2 = mapActions({
        add:'increment',
        sub:'decrement'
    })
    return {
        ...storeActions,
        ...storeActions2
    }
}
```

### module有命名空间时的辅助函数

```js
methods: {
    // 写法1
    ...mapstate({
        counter: state => state.home.counter
    })
    ...mapGetters({
        doubleCounter: 'home/doubleCounter' //home模块下的doubleCounter函数
    })
    // 写法2
    ...mapState("home", ['counter'])
    ...mapGetters("home", ['doubleCounter'])
}

// 写法3  导入命名空间辅助函数
import {createNameSpacedHelpers} from 'vuex'
// createNameSpacedHelpers返回一个该home模块的辅助函数对象
const {mapState, mapGetters, mapActions,mapMutations} = createNameSpacedHelpers('home')
export default {
   methods:{
    ...mapState(['counter'])
    ...mapGetters(['doubleCounter'])
    ...mapMutations(['increment', 'decrement'])
    } 
}

```

### module内重新封装有moduleName的useState和useGetters

```js
// useMapper.js
import {computed} from 'vue'
import {useStore} from 'vuex'
export function useMapper(mapper,mapperFn){
    const store = useStore()
    const storeFn = mapperFn(mapper) // 从外部传入要调用mapState还是mapGetters
    const storeState = {}
    Object.keys(storeFn).forEach(key => {
        const fn = storeFn[key].bind({$store: store})
        storeState[key] = computed(fn)
    })
    return storeState
}


// useState.js
import {mapState, createNamespacedHelpers} from 'vuex'
import {useMapper} from 'useMapper'
// 多传入一个模块名moduleName
export function useMapState(moduleName, mapper){
    let mapperFn = mapState
    if(typeof moduleName === 'string' && moduleName !== ''){
        //把mapperFn转化后传入useMapper 这样调用时就支持模块化了
        mapperFn = createNamespacedHelpers(moduleName).mapState
    }else{
        // 没传moduleName的情况，把moduleName作为mapper传给useMapper函数
        mapper = moduleName 
    }
    return useMapper(mapper, mapperFn) 
}

// 外部调用传入模块名即可，useState('home',['counter','name'])
```

## vue3全局属性globalProperties

```js
//main.js 注册全局过滤器
// app.use会把app传给里面的执行函数, 从而通过app进行注册操作
app.use(registerProperties)
app.use(pluginFunction)

// registerProperties.js
import moment from 'moment'
export default function registerProperties(app){ //这里拿到app.use传入的app
    app.config.globalProperties.$filters = {
        formatUTCTime(utcTime: any, format: string = DATA_FORMAT) {
          const local = moment.utc(utcTime)
          const localTime = moment(local).format(format)
          return localTime
        }
  	}
}

// 拿到app对象, 注册任何想要实现的插件
export function pluginFunction(app){
    app.component() //注册全局组件
    app.mixin()   //注册全局混入
}
```

## setup内拿全局属性

```js
import {getCurrentInstance} from 'vue'
setup(){
    const instance = getCurrentInstance()
    console.log(instantce.appConetxt.config.globalProperties.$filters.formatUTCTime)
}
```



## v-model对组件双向绑定 

### 子组件事件emit("update:modelValue")

```vue
<!--父组件-->
<!-- v-model默认绑定一个接收事件： @update:model-value = "message = $event"  -->
<my-component v-model="message"></my-component>


<!--子组件接收-->
<template>
	//...code
</template>
<script>
	export default {
        props:{
            modelValue: {  //v-model传过来的数据自动保存到modelValue中
                type: Object,
                required: true
            }
        },
        setup(props,{emit}){
            // 实现单向数据流，拷贝对象，不会对父组件产生影响，
            const mydata = ref({...props.modelValue})
            // 监听modelValue改变 发送给父组件进行更新
            watch(
                mydata,
                (newValue)=>{
                    // 子组件触发update事件, 传给父组件
      				emit('update:modelValue', newValue)
            }, {
                deep: true
            })
        }
    }
</script>
```

