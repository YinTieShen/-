# 前端学习整理

标签（空格分隔）： html css js

---

###**内部div水平垂直居中**
```html
<div class="box">
    <div class="content"></div>
</div>
```
>1.使用定位，外部设定`position: relative`，内部设定`position: absolute`.
```css
.box{
        position: relative;
        width: 200px;
        height: 200px;
        background: blue;
    }
    .content{
        position: absolute;
        margin: auto;
        width: 100px;
        height: 100px;
        background-color: red;
        top: 0; 
        left: 0; 
        bottom: 0; 
        right: 0; 
    }
```
>2.使用table布局，外部设置： `display:table-cell`  `vertical-align:middle` `text-align: center` 内部设置：display:inline-block
```css
.box{
    display:table-cell; 
    vertical-align:middle; 
    text-align: center;
    width: 200px;
    height: 200px;
    background: blue;
}
.content{
    display: inline-block;
    width: 100px;
    height: 100px;
    background-color: red;
}
```
>3.flex布局,外部设置flex，`justify-content:center` `align-items:center;`
```css
.box{
    display:flex;
    justify-content:center;
    align-items:center;
    width: 200px;
    height: 200px;
    background: blue;
}
.content{
    width: 100px;
    height: 100px;
    background-color: red;
}
```
>4.设置定位，50%，50%，-一半像素
```css
.box{
    position: relative;
    width: 200px;
    height: 200px;
    background: blue;
}
.content{
    position: absolute;
    top:50%;
    left: 50%;
    margin-left: -50px;
    margin-top: -50px;
    width: 100px;
    height: 100px;
    background-color: red;
}
```


----------
###**div内部文字水平垂直居中**
```html
<div class="box">我是居中文字</div>
```
>单行文本：
1.外部div设置 `text-align: center;` `line-height: div高度;`
```css
//line-height
.box{
    width: 200px;
    height: 200px;
    text-align: center;
    line-height: 200px;
    background: blue;
}
//table
.box{
    width: 200px;
    height: 200px;
    display: table-cell;
    text-align: center;
    vertical-align: middle;
    background: blue;
}
```
>多行文本：
```html
<div class="box">
    我是居中文字
    <br>
    我也是！！
</div>
```

1. 外部div设置 `display:table-cell`，`vertical-align: middle;`  `text-align: center; `
```css
.box{
    width: 200px;
    height: 200px;
    display: table-cell;
    text-align: center;
    vertical-align: middle;
    background: blue;
}
```
2.父元素使用`display:table`,子元素（所有文字p包裹）使用`display:table-cell`属性来模拟表格，子元素设置`vertical-align: middle; text-align: center;`即可水平垂直居中。(vertical-align只作用在inline-block或者inline，还有table-cell等元素内)
```css
.box{
    width: 200px;
    height: 200px;
    display: table;
    text-align: center;
    vertical-align: middle;
    background: blue;
}
p{
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
```
3.子元素设置`display:inline-block`，转化成行内块元素，模拟成单行文本。父元素设置对应的height和line-height。子元素设置`vertical-align:middle`，添加line-height属性，覆盖继承自父元素的行高。缺点：文本的高度不能超过外部盒子的高度。
```css
.box{
    width: 200px;
    height: 200px;
    text-align: center;
    line-height: 200px;
    background: blue;
}
p{
    display: inline-block;
    vertical-align: middle;
    line-height: 20px; 
}
```
4.采用内部div居中的方式（略）


----------
###**三栏布局（两侧固定中间自适应**）
>1.绝对定位布局：position + margin
```html
<div class="container">
    <div class="left">Left</div>
    <div class="right">Right</div>
    <div class="main">Main</div> //注意main在第三位
</div>
```
```css
.container{
    position: relative;
    width: 100%;
    height: 200px;
}
.left{
    position: absolute;
    background-color: rebeccapurple;
    height: 100%;
    width: 100px;
    left: 0;
}
.right{
    position: absolute;
    background-color: aqua;
    height: 100%;
    width: 200px;
    right: 0;
}
.main{
    background-color: brown;
    height: 100%;
    margin: 0 200px 0 100px;
}
```
>2.浮动布局：float+margin
```css
.container{
    /* position: relative; */
    width: 100%;
    height: 200px;
}
.left{
    float: left;
    background-color: rebeccapurple;
    height: 100%;
    width: 100px;
    
}
.right{
    
    background-color: aqua;
    height: 100%;
    width: 200px;
    float: right;
}
.main{
    background-color: brown;
    height: 100%;
    margin: 0 200px 0 100px;
}
```
>3.flex布局（此时div.main在第二个位置）
```css
.container{
    display: flex;
    width: 100%;
    height: 200px;
}
.left{
    background-color: rebeccapurple;
    height: 100%;
    width: 100px;
    
}
.right{
    
    background-color: aqua;
    height: 100%;
    width: 200px;
}
.main{
    background-color: brown;
    height: 100%;
    flex: 1;
}
```
4.table布局，外部`display: table;`,内部`display: table-cell;`(此时高度根据内容决定，main在第二w)
```css
.container{
    display: table;
    width: 100%;
    height: 200px;
}
.left{
    display: table-cell;
    background-color: rebeccapurple;
    height: 100%;
    width: 100px;
}
.right{
    display: table-cell;
    background-color: aqua;
    height: 100%;
    width: 200px;
}
.main{
    display: table-cell;
    background-color: brown;
    height: 100%;
}
```
>5.圣杯布局(主题为大，main为第一位）
```
<div id="container">
  <div id="center" class="column"></div>
  <div id="left" class="column"></div>
  <div id="right" class="column"></div>
</div>
```
```css
.container{
    padding-left: 100px;
    padding-right: 200px;
    height: 200px;
}
.container>div{
    float: left;
}
.left{
    margin-left: -100%;
    position: relative;
    background-color: rebeccapurple;
    right: 100px;
    height: 100%;
    width: 100px;
    
}
.right{
    margin-right: -200px;
    background-color: aqua;
    height: 100%;
    width: 200px;
}
.main{
    width: 100%;
    background-color: brown;
    height: 100%;
}
```
>6.双飞翼布局（双飞翼布局的DOM结构与圣杯布局的区别是用container仅包裹住center。）
```
<div class="container">
    <div class="main">Main</div>          
</div>
<div class="left">Left</div> 
<div class="right">Right</div>
```
```css
.container{
    width: 100%;
    height: 200px;
    float: left;
}

.left{
    float: left;
    margin-left: -100%;      
    background-color: rebeccapurple;
    height: 200px;
    width: 100px;   
}
.right{
    float: left;
    margin-left: -200px;
    background-color: aqua;
    height: 200px;
    width: 200px;
}
.main{
    margin-left: 100px;
    margin-right: 200px;
    background-color: brown;
    height: 100%;
}
```




    

