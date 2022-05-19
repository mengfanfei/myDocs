## 1. 谈谈你对Vue的理解
* 渐进式JavaScript框架
* MVVM模式
* 代码体积小
* 只关注视图层

## 2. 什么是MVVM模式
> 全称：`Model-View-ViewModel`，Model表示数据模型层，view表示试图层，View-Model是连接Model和View的桥梁，数据绑定到ViewModel层并自动渲染到页面，视图发生变化通知ViewModel更新数据。

## 3. Vue实现双向绑定的原理是什么

> 通过数据劫持和发布者-订阅者模式的方式来实现的。通过Object.defineProperty()来劫持各个属性的setter，getter。在数据变动时发布消息给订阅者，触发相应的监听回调。

## 4. Vue中如何检测数组的变化
> 重写了数组的七个方法：push pop shift unshift splice sort reverse,这些方法都会改变数组本身

## 5. v-model的语法糖是怎么实现的
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- v-model 只是语法糖而已 -->
    <!-- v-model 在内部为不同的输入元素使用不同的property并抛出不同的事件 -->
    <!-- text和textarea 元素使用value property 和 input事件 -->
    <!-- checkbox 和radio使用checked  property 和 change事件-->
    <!-- select 字段将value 作为prop 并将change 作为事件 -->
    <!-- 注意：对于需要使用输入法(如中文、日文、韩文等)的语言，你将会发现v-model不会再输入法
    组合文字过程中得到更新 -->
    <!-- 再普通标签上 -->
    <input v-model="sth" />  //这一行等于下一行
    <input v-bind:value="sth" v-on:input="sth = $event.target.value" />
    <!-- 再组件上 -->
    <currency-input v-model="price"></currentcy-input>
        <!--上行代码是下行的语法糖
         <currency-input :value="price" @input="price = arguments[0]"></currency-input>
        --> 
        <!-- 子组件定义 -->
        Vue.component('currency-input', {
         template: `
          <span>
           <input
            ref="input"
            :value="value"
            @input="$emit('input', $event.target.value)"
           >
          </span>
         `,
         props: ['value'],
        })   
</body>
</html>
```
## 6. 对nextTick的理解
> nextTick是一个微任务。
> * nextTick中的回调是在下次DOM更新完成后执行的
> * 可以用于获取更新的DOM
> * Vue中的数据更新是异步的，使用nextTick可以保证逻辑在DOM更新之后执行

## 7. 组件data为什么必须是一个函数
> 是一个函数的话会给每个组件data分配一个空间，这样两者就不会互相干扰，保证准确性。如果是对象的话，组件中的data会互相干扰，根实例不会被复用，所以不存在引用问题
## 8. Vue的生命周期
> * `beforeCreate`: 在实例初始化之后，数据观测之前调用
> * `created`: 实例创建完成之后调用，在这一阶段实例完成：数据观测，属性和方法的运算，watch/event事件回调，还没有$el
> * `beforeMount`: 挂载之前调用，相关render函数首次被调用
> * `mounted`: 实例挂载之后被调用，此时可以操作DOM
> * `beforeUpdate`: 数据更新之前调用,可以进一步修改状态，不会触发重渲染
> * `updated`: 数据更新之后调用，避免更改状态，可能导致无限渲染
> * `beforeDestroy`: 实例销毁前调用
> * `destroy`: 实例销毁后调用
> * activated & deactivated
> #### Vue3
> * setup
> * onBeforeMount
> * onMounted
> * onBeforeUpdate
> * onUpdated
> * onBeforeUnmount
> * onUnmounted
> * onActivated
> * onDeactivated

## 9. 父子组件生命周期顺序
* 加载渲染过程

> 父 beforeCreate->父 created->父 beforeMount->子 beforeCreate->子 created->子 beforeMount->子 mounted->父 mounted

* 子组件更新过程

> 父 beforeUpdate->子 beforeUpdate->子 updated->父 updated

* 父组件更新过程

> 父 beforeUpdate->父 updated

* 销毁过程

> 父 beforeDestroy->子 beforeDestroy->子 destroyed->父 destroyed

## 10. Vue组件之间的通信方式
* props/$emit
* $parent/$children(vue3移除¥children)
* $refs
* $emit/$on bus (vue3移除$on)
* vuex
* $attrs/$listeners(vue3移除$listeners)
* provide/inject

## 11. 对Vuex的理解
> * vuex是专门为vue.js应用程序开发的状态管理工具
> * vuex的状态是响应式的
> * 改变store的唯一方式是显示的提交mutation，方便跟踪记录每一个状态的变化
> * vuex不是可持久化的
> * state: 定义了应用状态的数据结构，可以设置默认状态
> * getter
> * Mutation：是唯一更改 store 中状态的方法，且必须是同步函数。
> * Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。
> * Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中

## 12. hash和history的区别
1. hash带#，history没有，history更加美观
2. history相对较新，兼容性较hash差
3. 跳转的时候hash只能改变#后面的东西，history只要同源就行
4. history必须要和后端保持一致，路由全覆盖，不然容易出现404问题
5. 原理：
    * hash：是通过监听浏览器的onhashchange()事件变化，查找对应的路由规则
    * history：利用H5的history中新增的两个API pushState（）和replaceState（）和一个事件onpopstate监听URL变化
    * pushState（）和replaceState（）可以用于浏览器的历史记录栈，通过back、forward、go可以对当前浏览器进行修改，当他们发生修改的时候，尽管url变化了，但是不会立即向后端服务器发送请求，除非点击刷新

## 13. v-show 和 v-if的区别
> * v-if: 组件或者DOM根据条件适当的进行销毁和重建，也是惰性的
> * v-show: 不管初始条件是什么，都会渲染，只是简单的基于CSS样式：display进行切换
> * 频繁切换使用v-show

## 14. computed和watch的区别
> * computed: 计算属性，依赖其他属性，值具有缓存，只有它依赖的属性值发生变化，computed才会重新计算
> * watch: 监听，观察，每当监听的属性发生变化时都会执行回调函数进行后续操作
