---

title: "JavaScript client offset相关属性和对比"
description:
toc: true
authors:
tags:
- JavaScript
- 事件
categories:
- JavaScript
series:
- JavaScript
date: "2022-04-17T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false

---

- [1.client](#1client)
- [2.offset](#2offset)
- [3.offset/scroll/client对比](#3offsetscrollclient对比)

## 1.client

- ```clientWidth```: ```width+padding```;
- ```clientHeight```: ```width+padding```;
  - 属性只读，不可修改
- ```clientX```
- ```clientY```
  - event调用
- ```clientTop```
- ```clientLeft```
  - 盒子的border

## 2.offset

- ```offsetWidth```: ```width+padding+border```;
- ```offsetHeight```: ```width+padding+border```;
- ```offsetParent```:获取定位父元素/ body
- ```offsetLeft/offsetTop```:相对于父的偏移(从父的padding开始，不算border)

## 3.offset/scroll/client对比

1.
   - offset = width + padding + border
   - scroll = 不包含border的内容宽度
   - client = width + padding
2.
   - offsetTop/Left:
     - 调用:盒子
     - 作用:距离父元素的偏移距离
   - scrollTop/scrollLeft:
     - 调用:document.body.scrollTop(window调用)
     - 作用:浏览器无法显示的部分
   - clientY/X:
     - 调用:event
     - 作用:鼠标距离浏览器的距离
