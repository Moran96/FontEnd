### 文档
[ECMAScript6 Promise对象](http://es6.ruanyifeng.com/#docs/promise)

## 示例: 异步代码的优化过程

### 1. 异步流程的"回调地狱"

```JavaScript
// ajax 
$.ajax({
    url:'http://netease2.bluej.cn/search',
    data:{
        keywords: '海阔天空'
    },
    success(res){   // 回调
        console.log('list', res.result.songs);
        let song = res.result.songs[0];
        let id = song.artists[0].id;
        /* ajax嵌套 */
        $.ajax({    // 回调
            url:'http://netease2.bluej.cn/artist/desc',
            data:{id : id},
            success(res2){
                console.log('歌手描述', res2);
            }
        })
    }
})
```

### 2. 使用Promise对象进行初步化简
```JavaScript
function render(data) {
    console.log(data);
}

var p2 = new Promise( (resolve)=>{
    $.ajax({
        url:'http://netease2.bluej.cn/search',
        data:{ keywords: '海阔天空'},
        success(res){
            resolve(res);
        }
    })
})

p2.then( (res)=>{
    return new Promise((resolve)=>{
        let id = res.result.songs[0].artists[0].id;
        $.ajax({    // 回调
            url:'http://netease2.bluej.cn/artist/desc',
            data:{id : id},
            success(res2){
                console.log('歌手描述', res2);
                resolve(res2);
            }
        })
    })
}).then( (res2)=>{
    console.log(res2);
    render(res2);
})
```

### 3. 封装与流程化控制

```JavaScript
/* 渲染函数 */
function render(data) {
    console.log(data);
}
/* step1 */
function get_songs() {
    return new Promise( (resolve, reject)=>{
        $.ajax({
            url:'http://netease2.bluej.cn/search2',
            data:{ keywords: '海阔天空'},
            success(res){
                resolve(res);   // 成功回调
            },
            error(err){
                reject(err);    // 失败回调
            }
        })
    })
}
/* step2 */
function get_desc(res) {
    return new Promise((resolve)=>{
        let id = res.result.songs[0].artists[0].id;
        $.ajax({    // 回调
            url:'http://netease2.bluej.cn/artist/desc',
            data:{id : id},
            success(res2){
                resolve(res2);
            }
        })
    })
}
/* 流程化执行 */
get_songs().then( get_desc ).then( render )
```
### 4. 总结
- Promise: 用同步想法去写异步代码.
- 函数作为参数: 外部函数不会考虑内部函数的构成,只决定内部函数的执行时机.
  - 例如: xxx = new Promise( (resolve, reject)=>{} )
- 封装实现思路:
  - 每个step()函数 按照异步过程封装.
  - 直接返回Promise对象, 以便后续 step1().then( step2 ).then( step3 ). ...
  - 每一步准备向下传递的数据存在当步的resolve(data)/reject(data)中.
  - 每一步准备接收上一步的数据直接写在当步函数的形参内.

### 5. 补充
关于promise与promise.then()函数:
  promise对象的创建是同步过程,立即执行; 而一些异步代码端通常装进promise容器里.
  promise.then()函数是 **异步** 的, 可视为一个监听事件,实时监听promise的执行情况(而非监听promise的状态属性,promise的三个状态是根据内部代码进行改变的),当 promise内部代码全部执行完毕且状态为'成功'后, then()函数再执行.
  
  promise的状态只能改变一次(进行中-->已成功 或 进行中-->已失败), 不可逆.
  
  任务队列: 若多个异步同时执行,则先执行微任务(如then)后执行宏任务(如定时器).