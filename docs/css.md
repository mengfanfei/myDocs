## 修改input placeholder样式
```css
.placeholder-custom::-webkit-input-placeholder {  
  color: #babbc1;
  font-size: 12px;
}
```
## 巧用not选择器
```css
li:not(:last-child){
  border-bottom: 1px solid #ebedf0;
}
```
## 使用caret-color改变光标颜色
```html
<input type="text" class="caret-color" />
```
```css
.caret-color {
  /* 关键css */
  caret-color: #ffd476;
}
```
## 移除`type="number"`尾部的箭头
```html
<input type="number" class="no-arrow" />
```
```css
/* 关键css */
.no-arrow::-webkit-outer-spin-button,
.no-arrow::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
```
##  `outline:none`移除input状态线
```html
<input type="number" class="no-arrow" />
```
```css
.no-arrow{
  outline: none;
}
```
## 解决IOS滚动条卡顿
```css
body,html{   
  -webkit-overflow-scrolling: touch;
}
```
## 隐藏滚动条
```css
/* 关键代码 */
.box-hide-scrollbar::-webkit-scrollbar {
  display: none; /* Chrome Safari */
}
```
## 自定义文本选中的样式
```css
.box-custom::selection {
  color: #ffffff;
  background-color: #ff4c9f;
}
```
## 禁止文本选中
```css
.box p:last-child{
  user-select: none;
}
```
## 多行文本省略
```css
.more-line-ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  /* 设置n行，也包括1 */
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
```