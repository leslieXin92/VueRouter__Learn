# 1. 路由的简介

## 1.1 vue-router

vue的一个插件库，专门用来实现SPA应用。

## 1.2 SPA ( single page application )

1. 单页Web应用。
2. 整个应用只有一个完整的页面。
3. 点击页面中的导航链接，不会刷新页面，只做页面的局部更新。
4. 数据需要通过ajax请求获取。

## 1.3 route

### 1.3.1 什么是路由

1. 一个路由就是一种映射关系 ( key - value ) 。
2. key 为路径，value 可能是 function 或 component 。

### 1.3.2 路由的分类

1. 后端路由：
   1. 理解：value 是 function，用于处理客户端提交的请求。
   2. 工作过程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据。
2. 前端路由：
   1. 理解：value 是 component，用于展示页面内容。
   2. 工作过程：当浏览器的路径改变时，对应的组件就会显示。



# 2. 路由的基本使用

## demo：

src / main.js：

```javascript
import Vue from 'vue'
import App from './App.vue'

// 引入路由
import VueRouter from 'vue-router'
import router from './router'

Vue.config.productionTip = false

// 应用路由
Vue.use(VueRouter)

new Vue({
    render: h => h(App),
    router
}).$mount('#app')
```

src / router / index.js：

```javascript
// 引入VueRouter
import VueRouter from "vue-router";

// 引入路由组件
import One from '../pages/One'
import Two from '../pages/Two'

// 创建router实例对象，管理一组一组的路由规则
export default new VueRouter({
    routes: [{
        path: '/one',
        component: One
    }, {
        path: '/two',
        component: Two
    },]
})
```

src / App.vue：

```vue
<template>
    <div class="box">
        <Banner />
        <div class="main">
            <div class="nav">
                <router-link class="one" active-class="active" to="/One">to One</router-link>
                <router-link class="two" active-class="active" to="/Two">to Two</router-link>
            </div>
            <div class="content">
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import Banner from './components/Banner'

export default {
    name: 'App',
    components: { Banner }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.box {
    margin: 100px auto;
    width: 50%;
    height: 200px;
    background-color: springgreen;
}
.banner {
    height: 60px;
    font-size: 28px;
    text-align: center;
    line-height: 60px;
    background-color: brown;
}
.main {
    display: flex;
    height: 140px;
}
.nav {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    width: 30%;
    height: 140px;
}
.one,
.two {
    width: 80%;
    height: 50px;
    outline: 5px solid red;
    font-size: 24px;
    text-align: center;
    line-height: 50px;
    cursor: pointer;
}
.active {
    background-color: brown;
}
.content {
    width: 70%;
    font-size: 28px;
    text-align: center;
    line-height: 120px;
    background-color: peru;
}
</style>
```

src / components / Banner.vue：

```vue
<template>
    <div class="banner">banner</div>
</template>

<script>
export default {
    name: 'Banner'
}
</script>
```

src / pages / One.vue：

```vue
<template>
    <div>One component</div>
</template>

<script>
export default {
    name: 'One'
}
</script>
```

src / pages / Two.vue：

```vue
<template>
    <div>Two component</div>
</template>

<script>
export default {
    name: 'Two'
}
</script>
```

## summary：

1. 安装 Vue-router：

   ```bash
   npm i vue-router
   npm i vue-router@3 // vue2版本要安装router3
   ```

2. 应用插件：main.js

   ```javascript
   Vue.use(VueRouter)
   ```

3. 编写 router 配置项：src / router / index.js

   ```javascript
   // 引入VueRouter
   import VueRouter from "vue-router";
   
   // 引入路由组件
   import One from '../pages/One'
   import Two from '../pages/Two'
   
   // 创建router实例对象，管理一组一组的路由规则
   export default new VueRouter({
       routes: [{
           path: '/one',
           component: One
       }, {
           path: '/two',
           component: Two
       },]
   })
   ```

4. 实现切换：( active-class )

   ```html
   <router-link active-class='active' to='/One' > skip to One </router-link>
   ```

5. 指定位置展示：

   ```html
   <router-view> </router-view>
   ```




# 3. 几个注意点

1. 【路由组件】通常存放在 pages 文件夹，【一般组件】通常放在 components 文件夹。
2. 通过切换，“ 隐藏 ” 了的路由组件，通常是被销毁的，需要的时候再去挂载。
3. 每个组件都有自己的 $route 属性，里边存储着自己的路由信息。
4. 整个应用只有一个 router，可以通过组件的 $router 属性来获取。