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

src/App.vue：

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

# 路由重定向

通过redirect属性重定向`'/'`路径到首页`/path/to/main`

@/router/index.js：

```js
const router={
    
    routes:[
        {path:'/', redirect:'/paht/to/main'},
        {},
        {}
    ]
}
```





# 嵌套路由

通过children属性声明子路由

@/router/index.js：

```js
const router={
    
    routes:[
        {path:'/', redirect:'/paht/to/main'},
        {path:'/father', component: Father, 
         	children: [
          		{path:'children', component: Children},
          		{},
            ]
        }
    ]
}
```





# 动态路由

通过`:`-`冒号`声明路径参数，实现动态路由。

例如`/path/to/user/:id`。

```js
const router={
    
    routes:[
        {path:'/path/to/user/:id', component: User},
        {},
        
    ]
    
}
```

`/path/to/user/1`

`/path/to/user/2`

都会路由到User组件

在User组件中，可通过`this.$route.param`对象获取这个路径参数。



## 属性传参

以属性`prop`的的方式接受路由参数。 主要解决`$router`对象产生的耦合

第一步

将路由映射对象的`props`属性设置为`true`

```js
const router={
    
    routes:[
        {path:'/path/to/user/:id', component: User,props:true},
        {},
        
    ]
    
}
```

第二步

组件设置自定义属性。

User.vue：

```vue
<template>
	<h1>
        your id is{{ id }}
    </h1>
</template>
<script>
	export default{
        //设置自定义属性id
       props: [ 'id' ]
    }
</script>
```

## 方法路由

| 声明式导航               | 编程式导航                  |
| ------------------------ | --------------------------- |
| `<router-link to='/..'>` | `this.$router.push('/...')` |

Test.vue：

```vue
<template>
	<button @click='to'>
        跳转
    </button>
</template>

<script>
	export default{
        methods:{
            to(){
                this.$router.push("/user/${id}")
            }
            
            
        }
    }
</script>
```



# 导航卫士

```js
router.beforeEach( (to, from, next)=>{

	if( to.paht='/..' && !isAuthenticated){
        next('/login')
    }
    else
    {
        next()//放行
    }
})
```



