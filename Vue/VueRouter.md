# Vue Router

## 快速入门

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

