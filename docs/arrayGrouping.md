### 情景
    将获取的到数组根据数量进行分组，几个一组几个一组变成二维数组
    比如：手机首页的金刚区，一页8个，多了分页
### 举例
```javascript
const arr = [1,2,3,4,5,6,7,8,9,10,11,12,13,24,25]
const newArr = []
for (let i = 0; i < arr.length; i += 8) {
  newArr.push(arr.slice(i, i + 8))
}
console.log(newArr)
// [[1, 2, 3, 4, 5, 6, 7, 8],[9, 10, 11, 12, 13, 24, 25]]
```
### 总结
```js
// 1. 数组分组函数
function arrayGrouping(arr, num) {
  const newArr = []
  for (let i = 0; i < arr.length; i += num) {
    newArr.push(arr.slice(i, i + num)) // // slice() 方法不会改变原始数组
  }
  return newArr
}
```
