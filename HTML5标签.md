# I.图片资源引入

---

**scr**: 图片地址 引入资源文件  
**alt**: 文本内容 地址失效时显示

* 相对路径加载
  ```html
  <img src="./img/002.jpg" alt="图片无法显示"  width="300">
  ```
* 绝对路径加载
  ```html
  <img src="http://127.0.0.1:8850/bj_test0/img/001.jpg" width="300">
  <img src="https://www.baidu.com/img/superlogo_c4dec.png?qua=high&where=super" >
  ```

  # 行内元素&置换元素

---

### 行内元素特点：

* 宽度只能靠内容撑开,不可以自定义宽度
* 不会独占一行
  ### 置换元素\(以图片元素为例\)
* 不会独占一行
* 宽度可以自行设置

# a标签

---

* a标签用于跳转,属于**行内元素**

# ul\(UNordered List\)

---

type = square/disc/circle""  
 ul&gt;li{sss}_3   
 ul&gt;li{sss$$}_4   
 {} 文本内容  
  $ 序号 可以一个或多个

list\_style 列表样式  
        去除列表头部圆点 list\_style: none;

```html
<ul type="square">
    <li>一级01</li>
    <li>
        一级02
        <ul>
            <li>二级01</li>
            <li>二级02</li>
            <li>二级03</li>
            <li>二级04</li>
        </ul>
    </li>
    <li>一级03</li>
    <li>一级04</li>
</ul>
```



