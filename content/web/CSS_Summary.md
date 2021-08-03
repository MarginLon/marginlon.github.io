---
title: "CSS Summary"
date: 2021-02-18T22:59:58+08:00
draft: false
---
- [Abstract](#abstract)
- [H5 & CSS3](#h5--css3)
  - [HTTP HyperText Transfer Protocol](#http-hypertext-transfer-protocol)
  - [HTML](#html)
    - [语义化](#语义化)
    - [规范](#规范)
    - [扩展](#扩展)
  - [!Entity](#)
  - [CSS](#css)
    - [At-rule](#at-rule)
    - [CSS变量](#css变量)
    - [文字](#文字)
    - [列表](#列表)
    - [链接](#链接)
    - [元素分类](#元素分类)
    - [边框](#边框)
    - [溢流](#溢流)
    - [背景](#背景)
    - [轮廓](#轮廓)
    - [阴影](#阴影)
  - [CSS3](#css3)
    - [浏览器引擎前缀](#浏览器引擎前缀)
    - [背景](#背景-1)
    - [边框](#边框-1)
    - [文字](#文字-1)
    - [动画&过渡](#动画过渡)
- [CSS选择器](#css选择器)
  - [选择器](#选择器)
    - [CSS3选择器](#css3选择器)
- [CSS预处理器](#css预处理器)
  - [- stylus](#--stylus)
- [CSS工程化模块化](#css工程化模块化)
- [布局](#布局)
  - [布局模型](#布局模型)
  - [布局](#布局-1)
- [移动端响应式适配](#移动端响应式适配)
- [Icon](#icon)
- [CSS工作原理](#css工作原理)
- [动画](#动画)
- [CSS渲染原理和性能优化](#css渲染原理和性能优化)
- [面试相关](#面试相关)
# Abstract
![HowCSSWork](https://github.com/MarginLon/MarginPostImage/blob/master/HowCSSWork.png?raw=true)

    1.浏览器加载HTML
    2.HTML->DOM
    3.浏览器获取HTML链接的大多数资源，包括CSS
    4.解析CSS，选择器类型将不同规则分类为不同的“存储桶”（渲染树）
    5.将渲染树放置在规则应用到其后应出现的结构中。
    6.页面的视觉显示（绘画）
---
# H5 & CSS3
## HTTP HyperText Transfer Protocol
- HTTP 状态码
    + 200 ：成功。
    + 400 ：客户端请求有语法错误，服务器端不能理解。
    + 401 ：该请求可能未经过授权。
    + 403 ：服务器端收到该请求，但是拒绝为它提供服务，可能是没有权限等等。
    + 404 ：该资源没找到。
    + 500 ：服务器端发生了一个不可预知的错误。
    + 503 ：服务器端当前还不能处理客户端的这个请求，可能过段时间之后才能恢复正常。
- Web Storage
  + cookie   
    1. 大小：4kb
    2. 带宽：访问消耗
    3. 安全风险
  + Web Storage
    1. 存储大小：5~10MB
    2. 不发送至服务器
    3. 更丰富的接口
    4. localStorage,sessionStorage
  + localStorage
    - 持久化的本地存储，不手动删除不会过期
    - 存储：localStorage.setItem(key,value);
    - 读取：localStorage.getItem(key);
    - 删除：localStorage.removeItem(key); / localStorage.clear();
## HTML
### 语义化
- 概念：对⽂本内容结构化（内容语义化），选择合乎语义的标签（代码语义化），便于开发者阅读，维护和写出更优雅的代码的同时，让浏览器的爬⾍和辅助技术更好的解析
- 优势：
   + 利于SEO优化
   + 在样式丢失的时候，仍可以⽐较好的呈现结构
   + 更好地⽀持各种终端
   + 利于团队开发和维护，提⾼效率
### 规范
- div和p，尽量⽤p，因为p在默认情况下有上下间距，对兼容特殊终端有利。
- 需要强调的⽂本，可以使⽤ strong 或 em 标签。
- 使⽤表格时，标题⽤ caption，表头⽤ thead，主体部分⽤ tbody 包围，尾部⽤ tfoot 包围。表头
和⼀般单元格要区分开，表头⽤ th，单元格⽤ td。
- 表单域要⽤ fieldset 标签包起来，并⽤ legend 标签说明表单的⽤途（可理解为表单标题）。
```html
<form>
<fieldset>
<legend>Personalia:</legend> <!-- legend 可理解为表单标题 -->
Name: <input type="text"><br>
Email: <input type="text"><br>
Date of birth: <input type="text">
</fieldset>
</form>
```
- 每个input标签对应的说明⽂本都需要使⽤label标签，并且通过为input设置id属性，在lable标签中
设置for=someld来让说明⽂本和相对应的input关联起来。或者直接在label中内嵌控件。
```html
<label for="username">请输⼊⽤户名: </label>
<input type="text" id="username" name="username">
<!-- OR -->
<label>请输⼊⽤户名<input type="text" id="username" name="usern
ame"></label>
```
- 嵌套规范：
   + 内联元素不能包含块元素
   + 块元素不能放在\<p\>内
   + h1~h6,dt不能包含块元素
- title, keywords, description精简全面
- a标签设置title;外部链接设置rel; rel="nofollow"
- 所有标题使用h1
- br只用于文本换行
- img设置alt
### 扩展
- meta
```html
<!-- 编码 -->
<meta charset="UTF-8" />
<!-- 关键词 -->
<meta name="keywords" content="" />
<!-- 描述 -->
<meta name="description" content=" " />
<!-- 视口-移动设备 -->
<meta name="viewport" content="width=device-width, initial-scale=
1">
<!-- 优先使用IE新版本和Chrome -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 刷新和重定向 -->
<meta http-equiv="refresh" content="0;url=" />
<!-- 禁⽌浏览器从本地计算机的缓存中访问⻚⾯内容 -->
<meta http-equiv="Pragma" content="no-cache">
<!-- 避免转码 -->
<meta http-equiv="Cache-Control" content="no-siteapp" />
<!-- 启用WebApp全屏模式 -->
<meta name="apple-mobile-web-app-capable" content="yes" />
```
- Entity
![Entity](https://github.com/MarginLon/MarginPostImage/blob/master/HTML%20Entity.png?raw=true)
---
## CSS
### At-rule
- 常规 ：@[KEYWORD] (RULE)
   + @charset
   + @import
   + @namespace
- 嵌套 : @[KEYWORD] { /* 嵌套语句 */ }
   + @document
   + @font-face
   + @keyframes
   + @media
   + @page
   + @supports

### CSS变量
- 全局/局部
```css
/*全局变量*/
:root { --color: blue; }
.box{color: var(--color)}

/*局部变量*/
.box{
--color: red;
color: var(--color);
}
```
- 拼接
   + 如果变量值是⼀个字符串，可以与其他字符串拼接；
   + 如果变量值是数值，不能与数值单位直接连⽤，使用calc()函数；
   + 如果变量值带有单位，就不能写成字符串;
```css
/*字符串拼接*/
.foo {
   --bar: 'hello';
   --concat: var(--bar) ' world';
}
/*变量为数值*/
.foo {
   --gap: 20;
   margintop: calc(var(--gap) * 1px);
}
/*带单位*/
.foo {
   --foo: 20px;
   font-size: var(--foo);
}
```
### 文字
```css
body {
    font-family:'Arial';
    font-size:40px;
    color:red;
    font-weight:bold;
    font-style:italic;
    text-decoration:line-through|underline|overline;
    text-indent:2em;
    line-height:30px;
    letter-spacing:2em;
    text-align:center|left|right;
    }
```
### 列表
```css
ul{
    list-style-type:circle|disc|square|decimal|lower-alpha|upper-alpha;
    list-style-position:outside|inside;
}
```
### 链接
```css
a:link{}
a:hover{}
a:active{}
a:visited{}
```
### 元素分类
- 块级元素
```css
<!--宽度继承父元素-->
<div>
  <p></p>
  <h1>
    ...
    <h6>
      <ol>
        <ul>
          <table>
            <address>
              <blockquote>
                <form></form>
              </blockquote>
            </address>
          </table>
        </ul>
      </ol>
    </h6>
  </h1>
</div>
```
- 行内元素
```css
<!--宽度为包含的内容的宽度，高度，行高，顶底边间距不可以设置-->
<a>
  <span>
    <br />
    <i>
      <em>
        <strong> <label></label></strong></em></i></span></a>
```
- 行内块元素
```css
<img /> <input />
```
### 边框
```css
div {
    border:2px solid|dashed|dotted red;
    border-radius:50%;
}

```
### 溢流
```css
p {

}
.one {
    overflow:auto;
}
.two {
    overflow:hidden;
}
.three {
    overflow:visible;
}
```
### 背景
```css
div {
    background-image: linear-gradient(to bottom, red, blue);
}
```
### 轮廓
```css
p{
    outline:red dotted thick;
}
```
### 阴影
```css
div {
    box-shadow:inset 2px 2px 5px #000000;
}
```
---

## CSS3
### 浏览器引擎前缀
- -moz- ：Firefox 等使用 Mozilla 引擎的浏览器。
- -webkit- ：Safari 、 Chrome 等使用 Webkit 引擎的浏览器。
- -o- ：Opera 浏览器早期。
- -ms- ：IE 和 Edge 等 。

### 背景
- ```background-size:length|percentage|cover|contain```;
- ```background-origin:padding-box|border-box|content-box```;
- ```background-clip:border-box|padding-box|content-box```;.

### 边框
- ```border-radius```
- ```border-image:source(url) slice width outset repeat```;
    - slice:图片边框向内偏移。
    - width:图片边框宽度。
    - outset:边框图像区域超出边框的量。
    - repeat:图像边框是否应该平铺（repeat）、铺满（round）或拉伸（stretch）。
- box-shadow:水平阴影 垂直阴影 模糊阴影 阴影尺寸 阴影颜色 inset;

### 文字
- RGBA
- text-shadow:水平阴影 垂直阴影 模糊阴影 阴影颜色;
- ```text-overflow:clip|ellipsis|string```;
  - text-overflow一定要和overflow: hidden;、white-space: nowrap; 一起使用，不能单独用。
- ```overflow-wrap:normal|break-word```;
- ```word-break:normal|break-all|keep-all|```;

### 动画&过渡
- 渐变
    - 线性 
        + ```background: linear-gradient(to direction | angle, color-stop1, color-stop2...);```
    - 径向
        + ```background-image: radial-gradient(shape size at position,start-color,...,last-color);```
- 过渡
    - ```transition: transition | transition-duration | transition-timing-function |
  transition-delay;```
        + cubic-bezier(n,n,n,n):在 cubic-bezier 函数中定义自己的值，取值范围是 0 ~ 1。
- 2D&3D
    - transform: 将 2D 或 3D 转换应用到元素上去
    - transform-origin：可以改变被转换元素的位置
    - transform-style：规定被嵌套元素如何在 3D 空间中显示
    - perspective：规定 3D 元素的透视效果，与 perspective-origin 结合使用，可以改变 3D 元素的底部位置
- 动画
    - @keyframes
---

# CSS选择器
## 选择器
- 基础选择器：#id, .class, element, *
- 组合选择器：并列, 后代(e1 e2, e1>e2), 兄弟(e1+e2, e1~e2)
- 属性选择器
    - E[attr]
    - E[attr=val]
    - E[attr~=val]:其中一个等于"val"
    - E[attr|=val]:其中之一以"val"开头
- 伪类选择器
    - a:
      - :link
      - :hover
      - :active
      - :visited
    - input:
      - :focus
      - :enabled
      - :disabled
      - :checked
- 选择器规则:  
   + 级联：两个规则，后者胜出

   + 特异性：内联>id>类>元素

   + 继承:inherit initial unset

### CSS3选择器
- 选择器
  - E~F E+F
  - E[attr^="val"]:以val开头
  - E[attr$="val"]:以val结尾
  - E[attr*="val"]：包含val
  - E:root 设置html
  - E:nth-child(n) 第n个子元素
  - E:nth-last-child(n)
  - E:nth-of-type() 
  - E:nth-last-of-type(n)
  - E:last-child
  - E:first-of-type
  - E:last-of-type
  - E:first-of-type
  - E:only-child
  - E:only-of-type
  - E:empty
  - E:target
  - E:not(s)
  - E:enabled & E:disabled
  - E:checked
- 伪元素
  - ::
  - ::after 
    - ```css
      <!-- 清除浮动 -->
      .clearfix::after{
        content:'';
        display:block;
        clear:both;
      }
      ``` 

---
# CSS预处理器
- less
- sass
  - Variable
    - !global
    - !default
  - mixins
    - @mixin
    - @include
  - extend
    - @extend selector
  - nesting
    - <font color = "red">&</font>:hover { ... }
    - 群组嵌套
    - \> + ~
    - ```
      nav {
          border: {
            style: solid;
            width: 1px;
            color: #ccc;
        }
      }
      ```
  - Interpolation
    - #{}
  - Function  
- stylus
---
# CSS工程化模块化

---
# 布局
## 布局模型
- Flow
    1. 块状元素在所处的包含元素内，自上而下按顺序垂直延伸，默认宽度100%
    2. 行内元素从左至右
- Float
    1. 伪元素清除浮动
```css
.clearfix:after {
  content: '';
  /*设置内容为空*/
  height: 0;
  /*高度为0*/
  line-height: 0;
  /* 行高为0*/
  display: block;
  /*将文本转为块级元素*/
  visibility: hidden;
  /*将元素隐藏*/
  clear: both; /*清除浮动*/
}

.clearfix {
  zoom: 1;
  /*为了兼容IE*/
}
```
- Layer
```css
/* 1. 绝对定位 */
/* + 以浏览器左上角为基准
   + 一个盒子包含在另一个盒子里，父不定位，子以浏览器左上角定位；父定位，子以父左上角定位
   + 绝对定位不占空间
*/
div {
    position:absolute;
    left: 100px; /*相对于浏览器向左偏移100像素*/
    top: 80px; /*相对于浏览器向上偏移80像素*/
    }


/* 2. 相对定位 */
/* + 以元素自身的位置为基准
   + 占空间位置
   + 子绝对父相对
*/
.box1 {
        width: 200px;
        height: 100px;
        position: relative;
        border: 1px dashed green;
      }
.box2 {
        width: 100px;
        height: 50px;
        position: absolute;
        border: 1px dashed blue;
        top: 20px;
        left: 20px;
      }

/* 3. 固定定位 */
/* + 类似于绝对定位，但固定定位相对于浏览器视口本身，而非<html>或祖先元素*/
p {
        position: fixed;
        top: 200px;
        left: 100px;
      }
```
## 布局
- Flex
    - 容器属性
        + flex-direction
            + row|row-reverse|column|column-reverse
        + flex-warp
            + nowrap|wrap|warp-reverse
        + flex-flow
            + flex-direction和flex-warp的简写
        + justify-context
            + flex-start|flex-end|center|space-between|space-around 
        + align-items
            + flex-start|flex-end|center|baseline|stretch
        + align-content
            + flex-start|flex-end|center|space-between|space-around 
    - 子元素属性
        + flex
            + flex-grow || flex-shrink || flex-basis;
        + align-self

---
---

# 移动端响应式适配

---
# Icon

---
# CSS工作原理

---


# 动画

---
# CSS渲染原理和性能优化

---

# 面试相关
- 选择器:
  - css3新增选择器 新增伪类 优先级
    - 选择器：  
      - E~F E+F
      - E[attr^="val"]:以val开头
      - E[attr$="val"]:以val结尾
      - E[attr*="val"]：包含val
    - 伪类：-child -of-type
    - 选择器规则:  
       + 层叠：两个规则，后者胜出

       + 优先级：!important>内联>id>类>元素>*>继承>默认

       + 继承:inherited:字体 文本 可见性 列表 声音 initial unset
  - 伪类伪元素区别
    - 伪类操作dom树中已有的元素，伪元素创建dom树外的元素。
    - 语法:和::。
    - 可以使用多个伪类，但同时只能使用1个伪元素。
    - 伪类弥补常规选择器的不足，便于获取用户操作等额外信息，伪元素创建了一个有内容的虚拟容器。
  - 属性继承 
    - inherited:字体 文本 可见性 列表 声音
  - 浏览器解析选择器
  - ```css
    :not() <!-- 权重：0，()内权重取决于内容 -->
    ``` 
---