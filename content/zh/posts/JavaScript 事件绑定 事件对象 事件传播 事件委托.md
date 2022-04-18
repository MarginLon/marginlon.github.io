---
title: "JavaScript 事件绑定 事件对象 事件传播 事件委托"
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

- [1. 事件绑定](#1-事件绑定)
- [2. 事件对象](#2-事件对象)
- [3. 事件传播](#3-事件传播)
- [4. 事件委托](#4-事件委托)

## 1. 事件绑定

- DOM0: DOM对象.事件 = 函数
- DOM2: ```element.addEventListener('click',function(){},false)```
  - true:捕获阶段触发 / false：冒泡阶段触发(默认)
  - this: 事件对象
  - ie8兼容:```element.attachEvent('onclick',function(){})``` 后绑定的先执行，this:window

## 2. 事件对象

- event:```event||window.event;```
  - timeStamp
  - bubbles:boolean
  - button
  - pageX/Y: ```event.pageX || scroll().left + event.clientX;```
  - clientX/Y:
  - screenX/Y:
  - target
  - type

## 3. 事件传播

- 捕获 目标 冒泡

## 4. 事件委托
