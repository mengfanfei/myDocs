### 情景
    限制输入框只能输入数字并最多限制几位小数
### 总结
```js
oninput="if(isNaN(value)) {value = parseFloat(value)} if (value.indexOf('.') > 0) {value = value.slice(0, value.indexOf('.') + 3)}"
// 可以改变最后一个数字，值为需要几位小数加一
```
