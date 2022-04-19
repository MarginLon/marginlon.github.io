---
title: "JavaScript scroll相关属性和缓动动画"
description:
toc: true
authors:
tags:
- JavaScript
- scroll
categories:
- scroll
series:
- JavaScript
date: "2021-04-14T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false

---

- [1.scroll](#1scroll)
- [2.缓动动画](#2缓动动画)

## 1.scroll

- ```window.onscroll()```:
  - 当我们用鼠标滚轮，滚动网页的时候，会触发 ```window.onscroll()``` 方法
- ```scrollWidth``` and ```scrollHeight```: ```width + padding```
- ```scrollTop``` and ```scrollLeft```:
  - 当某个元素满足```scrollHeight - scrollTop == clientHeight```时，说明垂直滚动条滚动到底了。
  - 当某个元素满足```scrollWidth - scrollLeft == clientWidth```时，说明水平滚动条滚动到底了。
  - 兼容写法:```window.pageYOffset || document.body.scrollTop || document.documentElement.scrollTop;```
  - 封装：

  ```js
  function scroll(){
      return {
          top:window.pageYOffset || document.body.scrollTop || document.documentElement.scrollTop,
          left:window.pageXOffset || document.body.scrollLeft || document.documentElement.scrollLeft
      }
  }
  ```

## 2.缓动动画

[缓动动画](#1scroll)
