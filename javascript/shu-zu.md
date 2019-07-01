# I.数组简介

## 1. 引用类型 & 简单类型

### 1.1. 引用类型 

- 传地址  堆内存
- 对于引用类型来说, "=" 赋值操作是浅拷贝,传递地址
- 可以存储不同数据类型
- 数组赋值给数组,并不是传值操作,而是对一个地址赋予了两个变量名.

```javascript
var arr1 = [1, 2];
var arr2 = arr1;
arr2[0] = 5;
console.log('1', arr1)      // [5,2]
console.log('2', arr2)      // [5,2]
```
### 1.2. 简单类型

- 传值 栈内存

```javascript
var str1 = 'hello'
var str2 = str1;
str2 = 'hehehehehe'
console.log('1', str1)      // 'hello'
console.log('2', str2)      // 'hehehehehe'
console.log( typeof arr)    //object
/* 数组是一个 引用类型 本质为一个对象 */
```

### 1.3. 对比
|数据类型|赋值操作符|内存使用|拷贝方式|
|---|---|---|---|
|简单类型|传值|栈内存|深拷贝|
|引用类型|传地址|堆内存|浅拷贝|

引用类型的比较 本质上是 内存地址的比较.
```javascript
 var arr3 = [1,2]
 var arr4 = [1,2]
console.log('3==4',arr3==arr4) //false
console.log('1==2',arr1==arr2) //true
```
## 2. 创建数组

- 创建数组的两种写法:**字面量** & **构造函数**. new方式参数为**数组长度**

```javascript
var arr5 = new Array(32);
console.log(arr5)
// [empty × 32] 
/* 默认每个数值为undefined. */
```
## 3. 数组与常规对象相比的特殊性(区别)
```javascript
var obj = {name:'hehe', age: 18}
```
- 访问对象属性的写法:
    常规写法 obj.name
    数组写法 obj['name']
- [2,1]和[1,2]的意义完全不同.
- 数组是**强调属性顺序**的特殊对象.其编号就是数组的属性,称之为下标,从0开始.

# II. 方法

## 1. 总览
|方法名称           |功能     |参数|"重载"|返回值|原始数据|
|---              |---      |:---:|---|---|---|
|**Array.push()**   |队尾增加  |增加内容|支持|数组长度|修改|
|**Array.unshift()** |队首增加 |-|-|-|-|
|**Array.pop()**     |队尾删除  |空|无|数组长度|修改|
|**Array.shift()**   |队首删除|空|无|数组长度|修改|
|**Array.concat()**  |数组拼接|-|支持|新数组|新建|
|**Array.splice()**  |万金油|(操作位置, 操作长度, 替换值)|支持|截取数组|修改|
|Array.indexOf()|元素查找|查找内容|否|首个下标|访问|
|Array.includes()|元素查找|查找内容|否|true/false|访问|
|**Array.join()**|**转字符串**|拼接方式|否|String|访问|
|Array.reverse()|顺序反转|空|无|Array|新建|
|Array.sort()|排序|空|无|Array|修改|
|Array.map()|遍历|function|no|新数组|新建|
|Array.filter()|过滤|function|no|新数组|新建|
|Array.forEach()|遍历|function|no|undefined|访问|
|Array.some()|循环|function|no|true/false|新建|


## 2. 增

### 2.1. *Array.push()*
```JavaScript
var arr6 = [0];
/* 从队尾增加 */
/* 1.返回值为数组更新后的长度. 2.可一次增加若干数值 */
/* 类似压栈操作 */
arr6.push(1);       
console.log('push(1)', arr6)
//push(1) (2) [0, 1]
```
### 2.2. *Array.unshift()*
```JavaScript
/* 从队首增加 */ 
/* 其余数组元素的位置(下标)依次向后增加) */
/* 1.返回值为更新后长度. 2.多值 */
arr6.unshift(-1, 7, 0);
console.log('unshift(-1)',arr6)
//unshift(-1) (5) [-1, 7, 0, 0, 1]
```
### 2.3. *Array.concat()*
```JavaScript
/* 数组拼接 */
/* 不改动原数组,返回新数组 */
var arr7 = ['33','77'];
console.log(arr6.concat(arr7, ['jijijij']) )
//(10) [-1, 7, 0, 0, 1, "33", "77", "jijijij", "33", "77"]
```

## 3. 删

### 3.1. *Array.pop()*
```JavaScript
/* 队尾删除 */
arr6.pop(); 
console.log('pop()', arr6)
//pop() (4) [-1, 7, 0, 0]
```
### 3.2. *Array.shift()*
```javaScript
/* 队首删除 */
arr6.shift();
console.log('shift()' ,arr6)
//shift() (3) [7, 0, 0]
```

## 4. 替换

### 4.1. *Array.splice()*

- *Array.splice(startIndex , lengh, value)* - (开始位置, 作用范围长度, 替换内容)
```JavaScript
arr6.splice(1,1,10)
console.log(arr6)   // [7, 10, 0]   -- 替换
arr6.splice(1,0,10)
console.log(arr6)   // [7, 10, 10, 0]   --增加效果
arr6.splice(1,2)
console.log(arr6)   // [7, 0]   --删除效果:只设置两个参数(起始位置和作用范围)
```

## 5. 其它

### 5.1. *Array.indexOf()*

- Array.indexOf()    返回首个存在下标;不存在返回-1
```JavaScript
console.log('indexOf',arr6.indexOf(3))    // -1
console.log('indexOf',arr6.indexOf(0))    // 1
```
### 5.2. *Array.includes()*
- Array.includes()     返回true/false
```JavaScript
console.log('includes',arr6.includes(3))   //false
console.log('includes',arr6.includes(0))   //true
```

### 5.3. *Array.join()*
- Array.join()     返回String,参数为拼接方式,默认用逗号拼接
```JavaScript
console.log( ['get','element','by','name'].join('-') )
```
### 5.4. *Array.reverse()*
- 逆序
```JavaScript
console.log( ['get','element','by','name'].reverse() )
```

### 5.5. *Array.sort()*
- 排序 默认升序(a-b);换(b-a)降序.
- 字符串排序按照编码表序号排序.
```javascript
 arr8.sort(function(a,b){
    console.log(a,b);
    return a-b;
});
```

## 6. 复杂类
大部分方法返回新数组的原因: 不破坏原始数据,为数据多次操作做保证
许多方法只是对于基础代码块的封装, 便于简化代码. 即便不使用这些方法也可以用基础的代码(例如for循环)来实现目标功能.

 
### 6.1. *Array.map()*
map 遍历. 通过return得到新数组
```javascript
var arr2 = arr1.map( function(item, index, array){
    // console.log(item, index, array)
    // item 数组成员; index 下标; array 原始数组;
    return item *= 10;
})
console.log(arr2)
```
### 6.2. *Array.filter()*
```JavaScript
// filter 过滤. 通过接收方法返回的布尔值, 添加item到新的数组
var arr3 = arr1.filter( function(item){
    if (item % 2 !==0 ) {
        return true;
    } else{
        return false;
    }
})
console.log(arr3)
```

### 6.3. *Array.forEach()*

```JavaScript
// forEach 只会返回undefined.不进行任何操作,只进行循环.常用于代替for循环.
// for(var i =0 ; i<arr1.length; i++){ }
var arr4 = arr1.forEach(function(item, index, arr){
    if( item == 2 ){
        arr.splice(index, 1);
    }
})
```

### 6.4 *Array.some()*
```JavaScript
// arr1.includes(1) 能判断存在性但不能处理.
// arr1.some( function(item, index, array){} ) 能判断存在性并进行处理.
var arr5 = arr1.some( function(item){
    if (item == 1){
        // do something.
        return true;
    }
} )
console.log(arr5);  // true / false
```