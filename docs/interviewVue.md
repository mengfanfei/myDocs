## 1. Vue实现双向绑定的原理是什么

    通过数据劫持和发布者-订阅者模式的方式来实现的。通过Object.defineProperty()来劫持各个属性的setter，getter。在数据变动时发布消息给订阅者，触发相应的监听回调。

## 2. v-model的语法糖是怎么实现的
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
## 3. hash和history的区别
1. hash带#，history没有，history更加美观
2. history相对较新，兼容性较hash差
3. 跳转的时候hash只能改变#后面的东西，history只要同源就行
4. history必须要和后端保持一致，路由全覆盖，不然容易出现404问题
5. 原理：
    * hash：是通过监听浏览器的onhashchange()事件变化，查找对应的路由规则
    * history：利用H5的history中新增的两个API pushState（）和replaceState（）和一个事件onpopstate监听URL变化
    * pushState（）和replaceState（）可以用于浏览器的历史记录栈，通过back、forward、go可以对当前浏览器进行修改，当他们发生修改的时候，尽管url变化了，但是不会立即向后端服务器发送请求，除非点击刷新
