---
title: "CSS 属性 选择器 浮动"
description:
toc: true
authors:
tags:
- 属性
- 选择器
- 浮动
- CSS
categories:
- CSS
series:
- CSS
date: '2020-10-01T22:02:39+08:00'
lastmode: "2020-10-01T22:02:39+08:00"
featuredImage:
featuredVideo:
draft: false

---
- [1.字体，文本，背景](#1字体文本背景)
- [2.CSS变量](#2css变量)
- [3.选择器](#3选择器)
- [4.浮动](#4浮动)

## 1.字体，文本，背景

```css
p{
 font-size: 50px;   /*字体大小*/
 line-height: 30px;      /*行高*/
 font-family: 幼圆,黑体;  /*字体类型：如果没有幼圆就显示黑体，没有黑体就显示默认*/
 font-style: italic ;  /*italic表示斜体，normal表示不倾斜*/
 font-weight: bold; /*粗体*/
 font-variant: small-caps;  /*小写变大写*/
}
vertical-align: middle; /*指定行级元素的垂直对齐方式。 内元素（inline）、行内块元素（inline-block）、表格的单元格（table-cell） */

div{
  letter-spacing:1px;
  word-spacing:1px;
  text-decoration:none;
  text-transform:lowercase;
  color:red;
  text-align:center;
}
```

```css
body {
  background-color:#ff99ff;
  background-image:url(images/2.gif);
  background-repeat:no-repeat;
  background-position:center top;
  background-attachment:scroll|fixed;
  background-origin:padding-box|border-box|content-box;
  background-clip:border-box|padding-box|content-box;
  background-size:1px 1px;
  background-image: linear-gradient(to direction | angle, color-stop1, color-stop2...);background-image: radial-gradient(shape size at position,start-color,...,last-color);
}
```

## 2.CSS变量

 全局/局部

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
  - 如果变量值是⼀个字符串，可以与其他字符串拼接；
  - 如果变量值是数值，不能与数值单位直接连⽤，使用calc()函数；
  - 如果变量值带有单位，就不能写成字符串;

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

## 3.选择器

- 基础选择器：#id, .class, element, *
- 组合选择器：并列, 后代(e1 e2, e1>e2), 兄弟(e1+e2, e1~e2)
- 属性选择器
  - E[attr]
  - E[attr=val]
  - E[attr~=val]:其中一个等于"val"
  - E[attr|=val]:其中之一以"val"开头
  - E[attr^="val"]:以val开头
  - E[attr$="val"]:以val结尾
  - E[attr*="val"]：包含val
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
- 伪元素

- ::after

  ```css
    <!-- 清除浮动 -->
    .clearfix::after{
      content:'';
      display:block;
      clear:both;
    }
  ```  

## 4.浮动

- 标准文档流
  - 1) 空白折叠
  - 2) 底边对齐
  - 3) 自动换行
- 行内元素和块级元素：
  - 行内元素：
    - 并排行内元素
    - 不能设置宽高，默认文字宽度
  - 块级元素：
    - 占一行
    - 可以设置宽高，默认宽度为父的100%
- 浮动：脱标，贴靠，字围，收缩
- 清除浮动
  1. 给浮动元素的祖先加高度
  2. ```clear:both;```;
     - margin失效
  3. 隔墙（外墙，内墙）
  4. ```overflow:hidden;```;（偏方）
     - ie6: ```_zoom:1;```
     - ie6微盒子: ```height:6px;_font-size:0;```  
