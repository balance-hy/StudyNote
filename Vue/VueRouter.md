# Vue Router

## 快速入门

vue-router 是一个插件包，使用前先安装

```shell
npm install vue-router --save-dev

# npm install  vue-router@3.1.3 --save-dev
```

[--save 和 --save-dev 的作用和区别](https://blog.csdn.net/cvper/article/details/88728505)

    使用命令 --save 或者说不写命令 --save  ,都会把信息记录到 dependencies 中；
    dependencies 中记录的都是项目在运行时需要的文件；
    
    使用命令 --save-dev 则会把信息记录到 devDependencies  中；
    
    devDependencies 中记录的是项目在开发过程中需要使用的一些文件，在项目最终运行时是不需要的；
    
    也就是说我们开发完成后，最终的项目中是不需要这些文件的；
    
    -S 即--save（保存）
    -D 即--dev（生产）
### 项目结构

一个刚创建的vue-webpack项目如下图所示

![image-20231107131210721](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311071312176.png)

index.html内容如下

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

src目录下有两个文件，分别为APP.vue和main.js

main.js内容如下

```js
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
```

可以看到这里面挂载了index.html中class=app的div，并且导入了App组件（即App.vue），并且在template中使用了该组件。

App.vue中内容如下

```vue
<template>
  <div id="app">
    <h1>Vue Router</h1>
  </div>
</template>

<script>

export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

### 使用Router

先给出src目录图

![image-20231107132147694](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311071321012.png)

我们在components目录下创建了两个组件分别是 content.vue 和main.vue 

内容为

```vue
<script>
export default {
  name: "main"
  // name: "content"  
}
</script>

<template>
  <h1>main</h1> 
  <!--  <h1>content</h1>-->
</template>

<style scoped>
/* 这里的scoped 表明该样式仅在该组件生效 */
</style>
```

**新建router目录，创建index.js配置文件**

> 注意前端大部分配置主文件，是默认命名为index.js

在index.js中写入如下内容

```js
import Vue from 'vue'
import VueRouter from "vue-router";//使用VueRouter
import Content from "../components/content.vue";//导入跳转的组件
import Main from "../components/main.vue";//导入跳转的组件

//安装路由
Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    {
      //路由路径
      path:'/main',
      name:'main',
      //跳转的组件
      component:Main
    },
    {
      path:'/content',
      name:'content',
      component:Content
    }
  ]
})
```

配置完成后，在main.js 导入配置文件并使用

```js
import Vue from 'vue'
import App from './App'
import router from "./router";//自定扫描router文件夹下index.js配置文件

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

在App.vue中增加

- router-link标签：类似于之前的a标签
- router-view标签：路由匹配到的组件将渲染在这里

```vue
<template>
  <div id="app">
    <h1>Vue Router</h1>
    <router-link to="/main">main</router-link>
    <router-link to="/content">content</router-link>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

完成

### 去除url中的#号

要去掉井号#，只需在定义路由时配置mode为history即可。

> vue2和vue3不相同，详见https://blog.csdn.net/qq_38870145/article/details/132520250
>
> 参考官网文档为准

```js
import Vue from 'vue'
import VueRouter from "vue-router";//使用VueRouter
import Content from "../components/content.vue";//导入跳转的组件
import Main from "../components/main.vue";//导入跳转的组件

//安装路由
Vue.use(VueRouter);

export default new VueRouter({
  mode: 'history',
  routes:[
    {
      //路由路径
      path:'/main',
      name:'main',
      //跳转的组件
      component:Main
    },
    {
      path:'/content',
      name:'content',
      component:Content
    }
  ]
})
```

需要注意的是，使用history模式时，需要保证后端服务器能够正确响应路由请求。不然，会出现404错误或者页面无法正常显示的情况。因此，我们需要针对后端服务器进行相应的配置。

## 嵌套路由

实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

```
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

在 `VueRouter` 的参数中使用 `children` 配置：

```js
import Vue from "vue";
import VueRouter from "vue-router";
import Main from "../views/main.vue";
import Login from "../views/Login.vue";
import UserList from "../views/user/List.vue";
import UserProfile from "../views/user/Profile.vue";

Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    {
      path:'/main',
      component:Main,
      children:[
        {
          path:'/user/profile',//    请求链接
          component:UserProfile// 路由（转发）到此组件
        },
        {
          path:'/user/list',//    请求链接
          component:UserList// 路由（转发）到此组件
        }
      ]
    },
    {
      path:'/login',
      component:Login
    }
  ]
});
```

