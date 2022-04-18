---
title: "CSS 动画"
description:
toc: true
authors:
tags:
- 动画
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
- [1.动画](#1动画)

## 1.动画

- 过渡： transition
  - ```transition-property:all```
  - ```transition-duration:1s```;
  - ```transition-timing-function:linear||ease||ease-in||ease-out||ease-in-out```
  - ```transition-delay:1s;```
- 2D&3D： transform
  - ```transform: scale(x, y);```
  - ```transform: translate(-50%, -50%);```
  - ```transform: rotate(45deg);```
  - ```transform-style```
- 动画
  - @keyframes
    - 定义:```@keyframes xxx { from { } to { }}```
    - 调用:```animation: xxx 1s;```
