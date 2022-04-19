---
title: "CSS Less"
description:
toc: true
authors:
tags:
- Less
- CSS
categories:
- Less
series:
- CSS
date: '2020-10-01T22:02:39+08:00'
lastmode: "2020-10-01T22:02:39+08:00"
featuredImage:
featuredVideo:
draft: false
---

```less
// 变量
// 选择器调用 路径调用 属性调用 变量调用
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}

// Mixins
// () -> 本体不编译
// (@color:black;@margin:10px) 指定参数
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}

// extend 
.box1:extend(.block){

}
 
// 嵌套
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}

.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}

// @规则嵌套和冒泡
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```
