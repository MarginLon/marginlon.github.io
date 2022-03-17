---
title: "JavaScript 闭包应用"
date: 2021-09-11T20:00:00+08:00
draft: false
---
- [1. 闭包应用：循环中的闭包处理方案](#1-闭包应用循环中的闭包处理方案)
  - [1.1. 循环事件绑定](#11-循环事件绑定)
  - [1.2. 循环定时器](#12-循环定时器)

# 1. 闭包应用：循环中的闭包处理方案   

---
## 1.1. 循环事件绑定 
```js
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) {
    // i=0 buttons[0] 第一个按钮
    // i=1 buttons[1] 第二个按钮
    // ....
    // 每一轮循环 i 变量的值 和 需要获取对应某个按钮的索引是一样的  buttons[i]
    buttons[i].onclick = function(){
       console.log('获取当前按钮索引:${i}');
       //不能实现
    };
  }

  //方案1.1：基于“闭包”
  // [每一轮循环都产生一个闭包，存储对应索引；
  // 点击事件触发，执行对应函数，让其上级context闭包]
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) {
      // 每一轮循环形成一个闭包，存储私有变量i的值
      //    + 自执行函数执行，产生EC(A) 私有形参i=0/1/2
      //    + EC（A）中有个小函数,让全局buttons中的某一项占用创建的函数
    (function(i) {
       buttons[i].onclick = function(){
        console.log('获取当前按钮索引:${i}');
       };
    })(i);
  }

  //方案1.2：
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) {
       buttons[i].onclick = (function(i){
        return function () {
            console.log('获取当前按钮索引:${i}');
            };
    })(i);
  }
  //方案1.2注解
  var obj = { //返回值赋值给fn
      fn: (function(){
          console.log('大函数');
          return function(){
              console.log('小函数');
          }
      })()
  };
  obj.fn();//执行返回的小函数

  //方案1.3 基于let也是闭包
  let buttons = document.querySelectorAll('button');
  for (let i = 0; i < buttons.length; i++) i{
       buttons[i].onclick = function(){
            console.log('获取当前按钮索引:${i}');
            };
  }

  //方案1.4 Nodelist forEach
  var buttons = document.querySelectorAll('button');
  buttons.forEach(function(item,index){
    item.onclick = function(){
      console.log('获取当前按钮索引:${i}');
    };
  });

  //方案2 自定义属性
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) i{
      // 每一轮循环给当前button设置自定义属性存索引
       buttons[i].myIndex = i;
       buttons[i].onclick = function(){
           // this -> 当前点击的按钮
            console.log('获取当前按钮索引:${this.myIndex}');
       };
  }

  //方案3 事件委托 html <button index='i'></button>;
  // 无论点击body中的谁，都会触发body的点击事件
  // ev.target事件源：具体点击的是谁
  document.body.onclick = function(ev) {
      var target = ev.target
          targetTag = target.tagName;
      if(targetTag==="BUTTON"){
          var index = target.getAttribute('index');
          console.log('获取当前按钮索引:${index}');
      }
  };
```

---
## 1.2. 循环定时器
```js
// 实现每间隔1秒输出 0 1 2
for (var i = 0; i < 3; i++) {
    setTimeout(function () {
        console.log(i);
    }, i * 1000);
}

// 1.
for (let i = 0; i < 3; i++) {
    setTimeout(function () {
        console.log(i);
    }, i * 1000);
}

//2.
let i = 0
for (; i < 3; i++) {
    (function(i){
      setTimeout(function () {
        console.log(i);
    }, i * 1000);
    })(i);
}

//3.
const fn = i => {
  return function () {
        console.log(i);
    };
};
let i = 0;
for (; i < 3; i++) {
      setTimeout(fn(i), i * 1000);
}

//4. 
let i = 0;
for (; i < 3; i++) {
      setTimeout(function (n) {
        console.log(n);
    }, i * 1000,i);
}
```
---
