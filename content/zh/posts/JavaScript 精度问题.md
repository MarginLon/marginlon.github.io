---
title: "JavaScript 精度问题"
description:
toc: true
authors:
tags: 
  - JavaScript
  - 精度问题 
categories:
  - 精度问题 
series:
  - JavaScript
date: "2022-04-11T16:27:39+08:00"
lastmode: "2022-04-08T16:28:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---

- [1. 精度问题](#1-精度问题)

---

## 1. 精度问题

解决方法：

- 扩大系数
- 第三方库 `Math.js decimal.js big.js`
  
JS中有关于小数的计算会出现精准度丢失的问题

- JS中所有值以2进制存在底层，会出现无限循环的情况
- 底层存储最多64位

浮点数计算的解决方案：

1. `toFixed`保留小数点N位，会自动四舍五入
2. 扩大系数

```js
const coefficient = function coefficient(num){
  num = num + '';
  let [, char = ''] = num.split(','),
      len = char.length;
  return Math.pow(10,len);
};
const plus = function plus(num1,num2){
  num1 = +num1;
  num2 = +num2;
  if(isNaN(num1)||isNaN(num2)) return NaN;
  let max = Math.max(coefficient(num1),coefficient(num2));
  return (num1 * max + num2 * max)/max;
};
```
