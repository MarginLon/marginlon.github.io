---
title: "JavaScript 面向对象[构造函数 原型和原型链 面向对象基础]"
date: 2021-09-14T20:00:00+08:00
draft: false
---
- [1. 构造函数](#1-构造函数)
- [2. 原型和原型链](#2-原型和原型链)
- [3. 面向对象基础](#3-面向对象基础)

# 1. 构造函数

- JS内置类
  - Number
  - String
  - Boolean
  - Symbol
  - BigInt
  - Object
  - Function
  - ...
  - DOM
    - HTML标签 -> HTMLXXXElement -> HTMLElement -> Element -> Node -> EventTarget -> Object
    - HTMLDocument -> Document -> Node -> ...
    - Window -> WindowProperties -> EventTarget -> Object
    - 元素集合 -> HTMLCollection -> Object
    - 节点集合 -> NodeList -> Object
- 自定义类（类 <=> 构造函数）
  - 构造函数 VS 普通函数
    1. 产生私有context及后续步骤
    2. <span style="color:#cc0033">构造函数创建一个空实例对象</span>
    3. 初始this，把this指向当前的实例对象。
    4. 执行完，return时
       - 没有return/return 基本数据类型值，浏览器默认把创建的实例对象返回
       - return 引用类型，返回自己的返回值。
  - Fn VS Fn()：Fn代表函数本身（堆内存），Fn()函数执行，获取返回值。
  - new Fn VS new Fn()：执行，第一个不传递实参，第二个传递实参。 优先级 19 VS 20
![构造函数和普通函数](https://github.com/MarginLon/MarginPostImage/blob/master/%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E5%92%8C%E6%99%AE%E9%80%9A%E5%87%BD%E6%95%B0%E6%89%A7%E8%A1%8C%E7%9A%84%E5%8C%BA%E5%88%AB.png?raw=true)

---
# 2. 原型和原型链
- function内置prototype属性，属性值是一个对象[堆内存]，对象中存储属性和方法是供当前类所属实例调用的公共属性和方法
  - <span style="color:#cc0033">箭头函数</span>和<span style="color:#cc0033">基于ES6给对象某个成员赋值函数值的快捷操作</span>没有prototype
  - Function.prototype是函数
  - 原型对象上有一个内置的属性 constructor（构造器），属性值是当前函数本身
- object都具备：\_\_proto\_\_，属性值是当前实例所属类的原型prototype
    * Object.protptype的\_\_proto\_\_值是null，Object是所有对象的“基类” 
- 原型链查找机制
    * f1.say
      * 先找私有属性
      * 没有，基于\_\_proto\_\_找所属类原型的公共属性方法
      * 没有，基于原型对象的\_\_proto\_\_查找，直到Object.prototype
    * f1.\_\_proto\_\_.say
      * 跳过私有的，找所属类原型的公有属性方法
      * Fn.prototype.say  基于原型对象查找
![原型和原型链](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%8E%9F%E5%9E%8B%E5%92%8C%E5%8E%9F%E5%9E%8B%E9%93%BE.png?raw=true)

---
# 3. 面向对象基础
+ JS创建值（实例）两种方案: 
  + 字面量
  + 构造函数new 
  + 对于对象和函数类型，无区别；<span style="color:red">对于原始值，字面量返回原始值类型，构造函数方式返回对象类型，但都是所属类的实例。</span> 
  + Symbol和BigInt 不允许new ```Object()``` 
  + 装箱: ```(10).toFixed(2)``` 原始值变为对象类型实例再操作
  + 拆箱： ```let n = new Number(10); n+10;``` 
+ 检测一个属性是否为当前对象的成员
  - 属性名 in 对象：无论公私有，有就是true
  - 对象.hasOwnProperty(属性名)：只有私有true 
+ 验证实例属于类 instanceof 
  + ```1 instanceof Number // => false``` 
+ 检测公有属性
  ```js
  function hasPubProperty(obj,attr){
  //弊端：既私有又公有
  return (attr in obj) && !obj.hasOwnProperty(attr);
  }
  ```
+ 基于"for key in obj"循环遍历 
  + 优先遍历数字属性
  + 不会遍历到Symbol
  + 会把自己扩展到“类原型”上的公共属性方法也遍历 [可枚举的] ```if(!obj.hasOwnProperty(key)) break;//遍历到公共属性，停止遍历```
  + Object.keys(obj):非Symbol私有属性 [数组]
  + Object.getOwnPropertyNames(obj): 同上
  + Object.getOwnPropertySymbols(obj):Symbol私有属性 [数组]
  ```js
  let keys = [...Object.keys(obj),...Object.getOwnPropertySymbols(obj)];
  keys.forEach(key=> {
              console.log(`属性名:$(),属性值:$()`);.
  });
  ```