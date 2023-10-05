# 安装与使用

Vue2 只能使用 VueRouter3，Vue3 只能使用VueRouter4。

通过npm 安装：

```shell
 npm add vue-router@3
 
 npm add vue-router@4
```



第一步，独立维护一个路由配置文件。

src/router/index.js:

```js
//导入VueRouter路由与Vue
import VueRouter from 'vue-router'
import Vue from 'vue'

//导入自定义组件
import Test from '@/components/Test.vue'
import My from '@/components/My.vue'
import Work from '@/components/Work.vue'

//Vue挂载路由
Vue.use(VueRouter)

//创建路由对象
//建立路径hash与组件的映射
const router=new VueRouter({
    routes:[
        { path:'/' , redirect:'/path/to/component1'}
        { path:'/path/to/component1' , component: Test},
        { path:'/path/to/component2' , component: My },
		{ path:'/path/to/component3' , component: Work }
    ]
})

//导出路由对象
export default router
```



第二步，在main.js 里面，通过router属性，挂载路由。

src/main.js:

```js
import Vue from 'Vue'
impory App from '@/App.vue'
import router from '@/router'


new Vue({
    render:(h)=>h(App),
    router
}).mount('#app')
```



第三部，创建显示路由：

src/App.vue

```vue
<template>
	<div>
        <router-link to='/path/to/component1'>组件1</router-link>
        <router-link to='/path/to/component2'>组件2</router-link>
        <router-link to='/path/to/component3'>组件3</router-link>
       	<!--路由出口 -->
        <router-view></router-view>
    </div>
</template>
```



# 嵌套路由





# 动态路由



## 属性路由



## 声明路由



## 方法路由



# 导航卫士

