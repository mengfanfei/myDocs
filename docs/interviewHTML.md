## 1. cookies、sessionStorage、localStorage的区别
> #### 存储大小
> * cookies数据大小不能超过4k
> * sessionStorage和localStorage虽然也有限制，但是要大的多，可达到5M或更大
> #### 有效时长
> * localStorage 存储持久数据，关闭浏览器数据不会丢失，除非主动删除
> * sessionStorage 只在会话中存在，页面会话结束数据就会被清除
> * cookies 在设置的有效时间内一直有效，即使窗口或浏览器关闭
> #### 作用域
> * sessionStorage 只在同源同窗口（或标签页）中共享数据，也就是在当前会话中
> * localStorage 在所有同源窗口中共享
> * cookies 在所有同源窗口中共享

[同源](interviewBrowser.md??id=_1-什么是同源策略？什么是跨域？怎么解决跨域？)
