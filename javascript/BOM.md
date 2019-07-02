# I. BOM

## 1. 页面 window
```JavaScript
/* 关闭当前页面 */
window.close()
```
## 2. 地址栏 location
```javascript
/* 页面跳转 */
location.href ='https://www.baidu.com'
/* 刷新页面 */
location.reload()
/* 接收参数 */
location.search
```
## 3. 历史 history
```javascript
/* 后退 */
history.back()
/* 前进 */
history.forward()
/* 跳转 (取负值为后退) */
history.go(num)
```
## 4. 浏览器 navigator
- 常用于兼容性选择
```JavaScript
/* 获取浏览器信息(设备,系统,内核,版本...) */
console.log(navigator.userAgent)
```
