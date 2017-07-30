# Vue_secondDay
Vue搭建后台管理系统第二天-------组件传递
Vue 的组件作用域都是孤立的，不允许在子组件的模板内直接引用父组件的数据。必须使用特定的方法才能实现组件之间的数据传递。

首先用 vue-cli 创建一个项目，其中 App.vue 是父组件，components 文件夹下都是子组件。

**一、父组件向子组件传递数据**

在 Vue 中，可以使用 props 向子组件传递数据。

 
**子组件部分：**
```
<template>
 <header>
  <div id='logo'>{{logo}}</div>
   <ul class='nav'>
    <li v-for:"nav in navs">{{nav.li}}</li>
   </ul>
  </header>
</template>
```

这是 header.vue 的 HTML 部分，logo 是在 data 中定义的变量。

如果需要从父组件获取 logo 的值，就需要使用 props: ['logo']

```
<script>
 export default{
  data(){
   retrun{
    navs:[{li:'我是li'},{li:'我是li'},{li:'我是li'}]
   }
  },
  props:['logo']
 }
</script>
```


在 props 中添加了元素之后，就不需要在 data 中再添加变量了

**父组件部分：**

```
<template>
 <div id='app'>
   <HeaderDiv :logo='logoMsg'></HeaderDiv>
 </div>
</template>
```

在调用组件的时候，使用 v-bind 将 logo 的值绑定为 App.vue 中定义的变量 logoMsg


然后就能将App.vue中 logoMsg 的值传给 header.vue 了：


**二、子组件向父组件传递数据**

 子组件主要通过事件传递数据给父组件

 

**子组件部分：**

 

这是 login.vue 的 HTML 部分，当
```
<input>
```
的值发生变化的时候，将 username 传递给 App.vue

首先声明一个了方法 setUser，用 change 事件来调用 setUser



在 setUser 中，使用了 $emit 来遍历 transferUser 事件，并返回 this.username

其中 transferUser 是一个自定义的事件，功能类似于一个中转，this.username 将通过这个事件传递给父组件 

 

**父组件部分：**


在父组件 App.vue 中，声明了一个方法 getUser，用 transferUser 事件调用 getUser 方法，获取到从子组件传递过来的参数 username


getUser 方法中的参数 msg 就是从子组件传递过来的参数 username

**三、子组件向子组件传递数据**

Vue 没有直接子对子传参的方法，建议将需要传递数据的子组件，都合并为一个组件。如果一定需要子对子传参，可以先从传到父组件，再传到子组件。

为了便于开发，Vue 推出了一个状态管理工具 [Vuex](https://vuex.vuejs.org/zh-cn/)，可以很方便实现组件之间的参数传递