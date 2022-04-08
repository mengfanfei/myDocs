## 纯数据字段
> 纯数据字段是一些不用于界面渲染的 data 字段，可以用于提升页面更新性能。从小程序基础库版本 2.8.2 开始支持。



```javascript
Component({
  options: {
    pureDataPattern: /^_/ // 指定所有 _ 开头的数据字段为纯数据字段
  },
  data: {
    a: true, // 普通数据字段
    _b: true, // 纯数据字段
  },
  methods: {
    myMethod() {
      this.data._b // 纯数据字段可以在 this.data 中获取
      this.setData({
        c: true, // 普通数据字段
        _d: true, // 纯数据字段
      })
    }
  }
})
```
从小程序基础库版本 [2.10.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，也可以在页面或自定义组件的 json 文件中配置 `pureDataPattern` （这样就不需在 js 文件的 `options` 中再配置）。此时，其值应当写成字符串形式：
```json
{
  "pureDataPattern": "^_"
}
```
## 简易双向绑定
```xml
<input model:value="{{value}}" />
```

## 小程序实现Tab栏（position: sticky）切换时固定在顶部
```javascript
const query = wx.createSelectorQuery()
let top = 0
let scrollTop = 0
// #thegoodsList为列表的id
query.select('#thegoodsList').boundingClientRect(res => {
  top = res.top // 节点的上边界坐标
})
query.selectViewport().scrollOffset(res => {
  scrollTop = res.scrollTop
})
query.exec(() => {
  if (top >= this.data.barHeight + 50) {
  } else {
    // 只需要获取到最开始thegoodsList距离上边的高度， 然后减去头部和tabs的高度，就是需要滚动的距离
    // 获取thegoodsList距离上边的高度，通过页面滑动的距离scrollTop加上实时hegoodsList距离上边的高度top
    wx.pageScrollTo({
      duration: 0,
      scrollTop: scrollTop + top - this.data.barHeight - 48 // 48 = 50 -2px的下边框
    })
  }
})
```
