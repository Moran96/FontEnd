## I. 变量声明

### 1. var and let

#### 1.1. var
- 声明提前(或变量提升) 即可以先使用后声明.
- 可重复声明.
- 可在全局或函数作用域下声明.

```javascript
console.log( value );   // undefined
var value = 1;          // 1
var value = 2;          // 2
function fun1() {
    var value = 10;
}
fun1()                  // 10
```

#### 1.2. let
- 不支持声明提前.

```javascript
console.log(i);         // ReferenceError
let i = 2;
```

- 在同一作用域下 不可重复声明.

```javascript
let i =2;
let i = 3;                 // SyntaxError.
```

- 仅在所声明的块级作用域内有效.
- for(let...){} 可在for的()声明在{}使用.

```javascript
function fun2() {
    let i = 3;
    console.log(i);
}
```

- es5需利用闭包保存变量,隔绝全局访问,才可实现目标效果;let+for 自带块级作用域, 直接实现.
 
```JavaScript
for (let index = 0; index < 5; index++) {
    setTimeout(function(){
        console.log(index);
    }, 100)
}
// var: 5 5 5 5 5 
// let: 0 1 2 3 4 
```

### 2. const

#### 2.1. const 常量声明 
- 不支持声明提前
- 不可重复定义(常量)
- 不可重复赋值(仅在声明时可赋值)

```javascript
const MAX = 100;
console.log(MAX);   // 100
MAX = 200;          // Uncaught TypeError: Assignment to constant variable.
```

- 使用方式: 声明同时赋值.
- 同let 存在块级作用域.

```javascript
{
    const MIN = 0;
    console.log(MIN);
}
console.log(MIN);   // Uncaught ReferenceError: MIN is not defined
```



#### 2.2. const 实际应用
- 避免变量名冲突可使用let或const.
- 对于不需要改变只提供方法的对象,可以使用const声明.


## II. 解构

### 1. 函数传参

```javascript
/* 原始方式 */
console.log( Math.max(1,2,3) ); // 3
/* 使用appy(null-改变函数内部指向, []-函数的参数列表) */
console.log( Math.max.apply( null, [1, 2, 3] ) );   // 3
/* 解构 - 传参 */
console.log(Math.max( ...[1, 2, 3] ));  // 3
```

### 2. 变量对位赋值

```javascript
var [a, b, c] = [10, 20];
console.log(a, b, c);  // 10 20 undefined
```

- 一般情况一个变量对应一个值.
- 只有当最后一个变量使用...时, 该变量可以接收多个值.
```javascript
var [d, e, ...f] = [30, 40, 50, 60];
console.log(d, e, f);   // 30 40 [50, 60]
```

### 3. 对象解构

简化对象属性访问-变量名和对象的属性名相同即可 
```javascript
var song = {
    id: "33323",
    "name": "song name",
    "artists":
        [{
            id: "8888888",
            name: "art name"
        }]
}
```
```javascript
function render(obj) {
    let { id, name, artist } = obj;
    mod = `
        <p>${id}</p><p>${name}</p><p>${artist}</p>
    `
}
```
```JavaScript
function render(obj) {
    let { id, name, artists } = obj;
    // 访问artists内部name属性会和song命名冲突.
    // 声明artName对应artist的name属性.
    let { name: artName } = artists[0];
    let mod = `
            <p>${name}</p>
            <p>${artName}</p>
        `
    console.log(mod);   // 
}
```

### 4. 定义函数缺省值

```javascript
function ajax(option) {
    /* 默认值写法1 */
    /* var type = option.type || 'get' */
    let {
        type = 'get', /* 默认值写法2 */
        url,
    } = option;

    console.log(type);
}

ajax({ type: 'post' });    // post
ajax({});    // get
```

默认值写法3 注意冒号和等号

```javascript
function fun({ name = 'hehe' }) {
    console.log(name);
}
fun({})             // hehe
fun({ name: 'haha' })  // haha
```

## III. Symbol

### 1. Symbol 数据类型

JS基本数据类型有6种:
- 简单类型: undefined null number string boolean
- 引用类型: object

ES6新增类型: symbol
- 表示独一无二的值. 它是 JavaScript 语言的第七种数据类型.
- [参考文档](http://es6.ruanyifeng.com/#docs/symbol)

```javascript
let s = Symbol('name');
console.log(s, typeof s);   // Symbol(name) "symbol"
let s2 = Symbol('name');
console.log( s==s2 );       // false
```

### 2. 应用: symbol作为属性名

两个属性字面上相同, 但可以共存

```JavaScript
var obj = {}
obj[s] = 'haha'
obj[s2] = 'hehe'
console.log( obj, obj[s] );
```

**注意: Symbol 值作为对象属性名时，不能用点运算符**

```JavaScript
console.log( obj, obj.Symbol(name) );   // TypeError
console.log( obj, obj.s );              // Undefined
```

### 3. 遍历方式

symbol无法使用for-in遍历.

```JavaScript
for( var k in obj ){
    console.log(k);
}
```

可以使用对象的getOwnPropertySymbols()方法获取属性.返回一个数组.

```JavaScript
/* 获取obj上的用Symbol数据类型表示的属性名 (key) */
var syb_keys = Object.getOwnPropertySymbols(obj);
console.log(syb_keys);
/* 重新获取symbol值 (key) */
var syb_key0 = syb_keys[0];
/* 通过symbol值访问对象上的属性值(value) */
console.log( obj[ syb_keys[0] ] );
```

### 4. symbol总结

- 第七数据类型
- 表示独一无二的值
- 设计初衷:
    - 解决对象的属性名(key)冲突的问题.
    - 可以将字面相同的属性名添加到同一对象中.
- symbol属性名不可被for..in遍历, 需使用`Object.getOwnPropertySymbols(obj)`获取.


## IV. Set

### 1. Set 集合特征
类似js数组, 但是1.不重复.2.size表示长度.
类似python集合, 但是可以通过位置下标访问.


### 2. set和array相互转化
set对象 转 array对象: 使用解析方式
array对象 转 set对象: 直接将数组作为构造参数调用
```javascript
var set1 = new Set([1, 2, 3, 1]);
console.log('set1 :', set1);    // set对象
console.log( [...set1] );
```
### 3. 应用实例:数组去重

```JavaScript
function get_differ(arr) {
    let set = new Set(arr)
    return [...set]
}
```
- 注意: 若set的成员包含多个对象,则视为互不重复;若成员包含多个NaN, 则视为重复,仅保留一个

### 4. set 方法
|方法|简介|
|:---|:---|
|set.delete()  |                                |
|set.add()     |                                |
|set.has()     | 类似数组includes(), 返回布尔类型  |
|set.clear()   |  清空全部,无返回值                 |

## V. Promise
详见专题: _**ES6 Promise对象**_

## VI. 参考文档
- [ES6参考手册](http://es6.ruanyifeng.com)