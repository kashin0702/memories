###  global.(j|t)sx?

全局前置脚本文件。

**Umi 区别于其他前端框架，没有显式的程序主入口（如 `src/index.ts`）**，所以当你有需要在应用前置、全局运行的逻辑时，优先考虑写入 `global.ts` 。

当你需要添加全局 Context 、修改应用运行时，请使用 [`app.tsx`](https://umijs.org/docs/guides/directory-structure#apptstsx) 。



### 配置

对于 umi 中能使用的自定义配置，你可以使用项目根目录的 `.umirc.ts` 文件或者 `config/config.ts`，值得注意的是这两个文件功能一致，仅仅是存在目录不同，2 选 1 ，`.umirc.ts` 文件优先级较高。

umi 的配置文件是一个正常的 node 模块，它在执行 umi [命令行](https://umijs.org/docs/api/commands)的时候使用，**并且不包含在浏览器端构建中。**

> 关于浏览器端构建需要用到的一些配置，还有一些在样式表现上产生作用的一些配置，在 umi 中被统一称为“运行时配置”，你可以在[运行时配置](https://umijs.org/docs/api/runtime-config)看到更多关于它的说明。

这里有一个最简单的 umi 配置文件的范例：

```ts
import { defineConfig } from 'umi';
export default defineConfig({  outputPath: 'dist',});
```

主要配置项：alias/base 等等



## UMI-MAX

我们将布局通过 Umi 插件的方式内置，只需通过简单的配置即可拥有 Ant Design 的 Layout（[ProLayout](https://procomponents.ant.design/components/layout)），包括导航以及侧边栏。从而做到用户无需关心布局。

- 默认为 Ant Design 的 Layout [@ant-design/pro-layout](https://www.npmjs.com/package/@ant-design/pro-layout)，支持它全部配置项。
- 顶部导航/侧边栏菜单根据路由中的配置自动生成。
- 默认支持对路由的 403/404 处理和 Error Boundary。
- 搭配 `access` [插件](https://umijs.org/docs/max/access)一起使用，可以完成对路由权限的控制。
- 搭配 `initial-state` [插件](https://github.com/umijs/umi/blob/master/packages/plugins/src/initial-state.ts) 和 [数据流](https://umijs.org/docs/max/data-flow) 插件一起使用，可以拥有默认用户登陆信息的展示。

## 布局与菜单

### 构建时配置

可以通过配置文件 `config/config.ts` 中的 `layout` 属性开启插件。

```js
import { defineConfig } from 'umi';

export default defineConfig({
  layout: {
    title: 'Ant Design',
    locale: false, // 默认开启，如无需菜单国际化可关闭
  },
});
```



### 运行时配置

运行时配置写在 `src/app.tsx` 中，key 为 `layout`。

```tsx
import { RunTimeLayoutConfig } from '@umijs/max';

export const layout: RunTimeLayoutConfig = (initialState) => {
  return {
    // 常用属性
    title: 'Ant Design',
    logo: 'https://img.alicdn.com/tfs/TB1YHEpwUT1gK0jSZFhXXaAtVXa-28-27.svg',

    // 默认布局调整
    rightContentRender: () => <RightContent />,
    footerRender: () => <Footer />,
    menuHeaderRender: undefined,

    // 其他属性见：https://procomponents.ant.design/components/layout#prolayout
  };
};
```





### 什么是运行时配置

运行时配置和配置的区别是他跑在浏览器端，基于此，我们可以在这里写函数、tsx、import 浏览器端依赖等等，注意不要引入 node 依赖。

**配置方式：约定 `src/app.tsx` 为运行时配置。**

主要配置项：dva, 数据流，layout





### 请求

`@umijs/max` 内置了插件方案。它基于 [axios](https://axios-http.com/) 和 [ahooks](https://ahooks-v2.surge.sh/) 的 `useRequest` 提供了一套统一的网络请求和错误处理方案。

```js
import { request, useRequest } from 'umi';
request;
useRequest;
```

####  构建时配置

```js
export default {

  request: {

    dataField: 'data'

  },

};
```

构建时配置可以为 useRequest 配置 `dataField` ，该配置的默认值是 `data`。该配置的主要目的是方便 useRequest 直接消费数据。如果你想要在消费数据时拿到后端的原始数据，需要在这里配置 `dataField` 为 `''` 。

比如你的后端返回的数据格式如下。

```js
{

  success: true,

  data: 123,

  code: 1,

}
```

那么 useRequest 就可以直接消费 `data`。其值为 123，而不是 `{ success, data, code }` 。



####  运行时配置

在 `src/app.ts` 中你可以通过配置 request 项，来为你的项目进行统一的个性化的请求设定。

```ts
import type { RequestConfig } from 'umi';
export const request: RequestConfig = { 
    timeout: 1000,  // other axios options you want  
    errorConfig: {    
        errorHandler(){    
        },    
        errorThrower(){    
        }  
    },  
    requestInterceptors: [],  
    responseInterceptors: []
};
```