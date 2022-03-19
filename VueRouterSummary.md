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



# 4. 多级路由

## demo：

src / router / index.js：

```javascript
// 引入VueRouter
import VueRouter from "vue-router";

// 引入路由组件
import One from '../pages/One'
import Two from '../pages/Two'
import A from '../pages/A'
import B from '../pages/B'

// 创建router实例对象，管理一组一组的路由规则
export default new VueRouter({
    routes: [{
        path: '/one',
        component: One
    }, {
        path: '/two',
        component: Two,
        children: [{
            path: 'a',
            component: A
        }, {
            path: 'b',
            component: B
        }]
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

<style scoped>
.box {
    margin: 00px auto;
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
    background-color: peru;
}
</style>
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

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
</style>
```

src / pages / Two.vue：

```vue
<template>
    <div class="box">
        <div class="nav">
            <router-link class="a" active-class="active" to="/Two/A">to A</router-link>
            <router-link class="b" active-class="active" to="/Two/B">to B</router-link>
        </div>
        <div class="content">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Two',
    mounted () {
        console.log(this);
    }
}
</script>

<style scoped>
.box {
    width: 100%;
    height: 100%;
}
.nav {
    height: 30%;
    display: flex;
    justify-content: space-around;
    background-color: aqua;
}
.a,
.b {
    width: 40%;
    height: 30%;
    margin: 10px 0;
    font-size: 24px;
    color: blueviolet;
    text-align: center;
    line-height: 20px;
    cursor: pointer;
}
.content {
    width: 100%;
    height: 70%;
    background-color: green;
}
</style>
```

src / pages / A.vue：

```vue
<template>
    <ul>
        <li>A</li>
        <li>AA</li>
        <li>AAA</li>
    </ul>
</template>

<script>
export default {
    name: 'A'
}
</script>

<style scoped>
li {
    list-style: none;
}
</style>
```

src / pages / B.vue：

```vue
<template>
    <ul>
        <li>B</li>
        <li>BB</li>
        <li>BBB</li>
    </ul>
</template>

<script>
export default {
    name: 'B'
}
</script>

<style scoped>
li {
    list-style: none;
}
</style>
```

## summary：

1. 配置路由规则，使用children配置项：src / router / index.js

   ```javascript
   // 引入VueRouter
   import VueRouter from "vue-router";
   
   // 引入路由组件
   import One from '../pages/One'
   import Two from '../pages/Two'
   import A from '../pages/A'
   import B from '../pages/B'
   
   // 创建router实例对象，管理一组一组的路由规则
   export default new VueRouter({
       routes: [{
           path: '/one',
           component: One
       }, {
           path: '/two',
           component: Two,
           children: [{ // 通过children配置子路由
               path: 'a', // 此处不能写成/a
               component: A
           }, {
               path: 'b', // 此处不能写成/b
               component: B
           }]
       },]
   })
   ```

2. 跳转 ( 要写完整路径 )

   ```html
   <router-link to='/two/a'> </router-link>
   ```




# 5. 路由的query参数

## demo：

src / router / index.js：

```javascript
// 引入VueRouter
import VueRouter from "vue-router";

// 引入路由组件
import One from '../pages/One'
import Two from '../pages/Two'
import A from '../pages/A'
import B from '../pages/B'
import Detail from '../pages/Detail'

// 创建router实例对象，管理一组一组的路由规则
export default new VueRouter({
    routes: [{
        path: '/one',
        component: One
    }, {
        path: '/two',
        component: Two,
        children: [{
            path: 'a',
            component: A
        }, {
            path: 'b',
            component: B,
            children: [{
                path: 'detail',
                component: Detail,
            }]
        }]
    }]
})
```

src / pages / B.vue：

```vue
<template>
    <ul>
        <li v-for="item in itemList" :key="item.id">
            <!-- 路由跳转并携带query参数，to的字符串写法 -->
            <router-link :to="`/two/b/detail?id=${item.id}&massage=${item.massage}`">
                {{ item.massage }}
            </router-link>

            <!-- 路由跳转并携带query参数，to的对象写法 -->
            <router-link :to="{ 
                path: '/two/b/detail', 
                query: { 
                    id: item.id, 
                    massage: item.massage 
                    } 
                }">
                {{ item.massage }}
            </router-link>
        </li>
        <router-view></router-view>
    </ul>
</template>

<script>
export default {
    name: 'B',
    data () {
        return {
            itemList: [{
                id: '001',
                massage: 'B',
            }, {
                id: '002',
                massage: 'BB',
            }, {
                id: '003',
                massage: 'BBB',
            }]
        }
    }
}
</script>

<style scoped>
li {
    list-style: none;
}
</style>
```

src / pages / detail.vue：

```vue
<template>
    <ul>
        <li>id：{{ $route.query.id }}</li>
        <li>massage：{{ $route.query.massage }}</li>
    </ul>
</template>

<script>
export default {
    name: 'Detail'
}
</script>
```

## summary：

1. 传递参数：

	```html
	<!-- 路由跳转并携带query参数，to的字符串写法 -->
	<router-link 
	    :to="`/two/b/detail?id=${item.id}&massage=${item.massage}`"
	>
	    {{ item.massage }}
	</router-link>
	
	<!-- 路由跳转并携带query参数，to的对象写法 -->
	<router-link 
	    :to="{ 
	             path: '/two/b/detail', 
	             query: { 
	                        id: item.id, 
	                        massage: item.massage 
	                    } 
	         }"
	>
	    {{ item.massage }}
	</router-link>
	```

2. 接收参数：

	```javascript
	$route.query.id
	$route.query.massage
	```




# 6. 命名路由

1. 作用：可以简化路由的跳转。

2. 使用：

   src / router / index.js：

   ```json
   {
       path: '/a',
       component: A,
       children: [{
           path: 'b',
           component: B,
           children: [{
               path: 'c',
               name: 'toC', // 给路由命名
               component: C
           }]
       }]
   }
   ```

   template：

   ```html
   <!-- 简化前，需要写完整的路径 -->
   <router-link to='/a/b/c'> skip to c </router-link>
   
   <!-- 简化后，直接通过name跳转 -->
   <router-link :to='{name:toC}'> skip to c </router-link>
   ```



# 7. 路由的params参数

## demo：

src / router  / index.js：

```javascript
// 引入VueRouter
import VueRouter from "vue-router";

// 引入路由组件
import One from '../pages/One'
import Two from '../pages/Two'
import A from '../pages/A'
import B from '../pages/B'
import Detail from '../pages/Detail'

// 创建router实例对象，管理一组一组的路由规则
export default new VueRouter({
    routes: [{
        path: '/one',
        component: One
    }, {
        path: '/two',
        component: Two,
        children: [{
            path: 'a',
            component: A
        }, {
            path: 'b',
            component: B,
            children: [{
                name: 'toDetail',
                path: 'detail/:id/:massage',
                component: Detail,
            }]
        }]
    }]
})
```

src / pages / B.vue：

```vue
<template>
    <ul>
        <li v-for="item in itemList" :key="item.id">
            <!-- 路由跳转并携带params参数，to的字符串写法 -->
            <router-link :to="`/two/b/detail/${item.id}/${item.massage}`">
                {{ item.massage }}
            </router-link>

            <!-- 路由跳转并携带params参数，to的对象写法 -->
            <router-link :to="{ 
                name:'toDetail', 
                params: { 
                    id: item.id, 
                    massage: item.massage 
                    }
                }">
                {{ item.massage }}
            </router-link>
        </li>
        <router-view></router-view>
    </ul>
</template>

<script>
export default {
    name: 'B',
    data () {
        return {
            itemList: [{
                id: '001',
                massage: 'B',
            }, {
                id: '002',
                massage: 'BB',
            }, {
                id: '003',
                massage: 'BBB',
            }]
        }
    }
}
</script>

<style scoped>
li {
    list-style: none;
}
</style>
```

src / pages / detail.vue：

```vue
<template>
    <ul>
        <li>id：{{ $route.params.id }}</li>
        <li>massage：{{ $route.params.massage }}</li>
    </ul>
</template>

<script>
export default {
    name: 'Detail',
    mounted () {
        console.log(this.$route);
    }
}
</script>
<style scoped>
li {
    list-style: none;
}
</style>
```

## summary：

1. 配置路由，声明接收params参数：

   ```json
   children: [{
       name: 'toDetail',
       path: 'detail/:id/:massage',
       component: Detail,
   }]
   ```

2. 传递参数：

   ```html
   <!-- 路由跳转并携带params参数，to的字符串写法 -->
   <router-link :to="`/two/b/detail/${item.id}/${item.massage}`">
       {{ item.massage }}
   </router-link>
   
   <!-- 路由跳转并携带params参数，to的对象写法 -->
   <router-link :to="{ 
                     	name:'toDetail',  // 必须使用name，不能使用path
                     	params: { 
                     		id: item.id, 
                     		massage: item.massage 
                     		}
                     }">
       {{ item.massage }}
   </router-link>
   ```

3. 接收参数：

   ```javascript
   $route.params.id
   $route.params.massage
   ```

   



