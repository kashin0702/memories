### 组合式API

#### setup

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



