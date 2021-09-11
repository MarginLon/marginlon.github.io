---
title: "JavaScript 数据类型及转换"
date: 2021-09-07T20:00:00+08:00
draft: false
---

- [1. 数据类型及转换](#1-数据类型及转换)
    - [1.1. 数据类型](#11-数据类型)
  - [- 函数对象 function fn(){}](#--函数对象-function-fn)
    - [1.2. 类型检测&&转换](#12-类型检测转换)

# 1. 数据类型及转换

---

### 1.1. 数据类型

- 原始值  
    - number:
  ```js
  // NaN 非有效数字、Infinity 无穷大
  // Number.MIN_SAFE_INTERGER
  // NaN == NaN false
  // NaN === NaN false  => NaN !== NaN
  // => isNaN([value]); 
  // Object.is(NaN,NaN); true
  ```
    - string: '', "",``
    - boolean: true、false
    - null
    - undefined
    - BigInt
    - Symbol:
  ```js
  // 1. 对象的唯一属性名 (属性名类型：string && symbol)
  //console.log(Symbol()===Symbol()); // false

  let key = Symbol();
  let obj1 = {
        [key]:100
      };
  console.log(obj[key]);

  let obj2 = {
        [Symbol()]:100
      };
  let arr = Object.getOwnPropertySymbols(obj);
    arr.forEach(key=>{
      console.log(obj[key]);
    });

  //2. redux/vuex公共状态管理，派发的行为标识基于symbol类型宏管理

  //3. Symbol.hasInstance\toStringTag\toPrimitive\iterator
  ``` 
  
- 对象类型
  - 标准普通 {num:100}
  - 标准特殊 [10,20] /\d+/ new Date()
  - 非标准特殊 new Number(10) 
  - 函数对象 function fn(){}
---

### 1.2. 类型检测&&转换

- 检测
  - typeof 运算符
    - 返回字符串
    - null -> 'object'
    - 实现CALL的对象[函数、箭头函数、生成器函数、构造函数] -> 'function'
    - 未实现CALL的对象 -> 'object'
      - 需求：验证val是否为一个对象
      ```js
      if(val!==null && /^(object|function)$/i.test(typeof val)){

      }
      ``` 
    - 未声明的变量 -> 'undefined'
      - 用于封装
    
- [example] instanceof [class]  实例是否属于类
- [example].constructor===[class] 获取构造函数
  - Object.prototype.toString.call([value])
  - Array.isArray([value])