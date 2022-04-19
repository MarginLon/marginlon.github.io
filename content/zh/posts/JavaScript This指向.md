---
title: "JavaScript This指向"
description:
toc: true
authors:
tags:
- JavaScript
- GC机制
- 闭包作用域
- let/const/var
categories:
- JavaScript
series:
- JavaScript
date: "2022-04-13T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false
---
- [1. This指向: 函数执行的主体](#1-this指向-函数执行的主体)
  - [1.1. 事件绑定](#11-事件绑定)
  - [1.2. 函数执行 [普通/成员访问/匿名函数/回调函数]](#12-函数执行-普通成员访问匿名函数回调函数)
  - [1.3. 构造函数](#13-构造函数)
  - [1.4. 箭头函数 [生成器generator]](#14-箭头函数-生成器generator)
  - [1.5. call/apply/bind强制修改this指向](#15-callapplybind强制修改this指向)

## 1. This指向: 函数执行的主体

- 全局context: this -> window
- 块级context: this -> 继承上级context(包括箭头函数)

---

### 1.1. 事件绑定

- DOM0: ```xxx.onxxx = function(){}```
- DOM2:
  - ```xxx.addEventListener('xxx',function(){})```  
  - ```xxx.attachEvent('onxxx',function(){})```
- 给当前元素的某个事件行为绑定方法，当事件触发、方法即执行，方法中的this->当前元素本身
- 特殊性：attachEvent -> window

```js
  document.body.onclick = function () {
    console.log(this);//body
};
```

### 1.2. 函数执行 [普通/成员访问/匿名函数/回调函数]

- 普通函数执行：函数执行前是否有点，没有点，this即window（严格模式下undefined）；有点，谁点this谁
- 匿名函数（自执行函数/回调函数）如果没有特殊处理，则this一般都是window/undefined
  - 函数表达式：等同于普通函数或事件绑定
  - 自执行函数: 一般是window/undefined
  - 回调函数：一般是window/undefined，如果另外函数执行中特殊处理，特殊为主。
  - 括号表达式：小括号中包含"多项"，取最后一项，但this受到影响（一般是window/undefined）

```js
function fn(){
  console.log(this);
}
let obj = {
  name: 'Margin',
  fn
  // fn: fn
};

fn(); //this->window
obj.fn();// this -> obj
(obj.fn)(); //this -> obj
//括号表达式
(10, obj.fn)(); // this -> window/undefined

// 函数表达式
var fn = function () {
  console.log(this);
};
fn();

// 自执行函数
(function(x){
  console.log(this); // this -> window/undefined
  })(10);

// 回调函数：把一个函数A作为实参，传递给另外一个执行的函数B [B执行中把A执行]
function fn(callback) {
  //callback -> 匿名函数
  //callback(); -> window
  callback.call(obj);  // this -> obj
}
fn(function(){
  console.log(this);
});

// 
let arr = [10,20,30];
arr.forEach(function(item,index){
    console.log(this); 
    // obj 触发回调函数执行的时候,
    // forEach内部把回调函数的this改为传递的第二个参数值obj
},obj);

//
setTimeout(function(){
    console.log(this); //window
},1000);
setTimeout(function(x){
    console.log(this, x); //window 10
},1000, 10);
```

### 1.3. 构造函数

- 初始this，把this指向当前的实例对象。

---

### 1.4. 箭头函数 [生成器generator]

- 使用上级context的this

---

### 1.5. call/apply/bind强制修改this指向

- ```Function.prototype```

---
