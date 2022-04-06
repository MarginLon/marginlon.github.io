---
title: "JavaScript 数据类型 类型检测 类型转换"
description:
toc: true
authors:
tags: 
  - JavaScript
  - 数据类型 
categories:
  - JavaScript
series:
  - JavaScript
date: "2021-09-24T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---

- [1. 数据类型](#1-数据类型)
- [2. 类型检测](#2-类型检测)
- [3. 类型转换](#3-类型转换)

---

## 1. 数据类型

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
  console.log(obj1[key]);

  let obj2 = {
        [Symbol()]:100
      };
  let arr = Object.getOwnPropertySymbols(obj);
    arr.forEach(key=>{
      console.log(obj2[key]);
    });

  //2. redux/vuex公共状态管理，派发的行为标识基于symbol类型宏管理

  //3. Symbol.hasInstance\toStringTag\toPrimitive\iterator
  ```
  
- 引用值(对象类型)
  - 标准普通 {num:100}
  - 标准特殊 [10,20] /\d+/ new Date()
  - 非标准特殊 new Number(10)
  - 函数对象(实现了call方法) function fn(){}

```js
new Symbol() // TypeError
new BigInt() // TypeError: Not a constructor
Object(Symbol()) // pass 
```

---

## 2. 类型检测

- typeof 运算符
  - 返回字符串
  - null -> 'object'
  - 实现CALL的对象[函数、箭头函数、生成器函数、构造函数] -> 'function'
  - 未实现CALL的对象 -> 'object'
  - 未声明的变量 -> 'undefined'
  - 需求：验证val是否为一个对象

    ```js
    if(val!==null && /^(object|function)$/i.test(typeof val)){

    }
    ```

- [example] instanceof [class]  实例是否属于类（通过原型链）
  - 返回true/false
  - 细分Object
  - 无法检测标准普通对象
  - 无法检测原始值类型
  - 运行：
    1. class[Symbol.hasInstance]:只支持ES6中的class static重构
    2. example原型链查找class直到Object.prototype
- [example].constructor===[class] 获取构造函数
- Object.prototype.toString.call([value])
  - 返回结果: "[object ?]"
    1. example[Symbol.toStringTag]
    2. 没有此属性，则找构造函数
  - 标准普通对象=>字符串
    - JSON.stringify
    - Qs.stringify
  - 自定义构造函数 : "[object Object]"

  ```js
  //需求：期望自定义构造函数："[object 构造函数]"
  const Fn = function Fn(){}
  Fn.prototype[Symbol.toStringTag] = "Fn";
  let f = new Fn;
  console.log(Object.prototype.toString.call(f));
  ```

- 快捷
  - Array.isArray([value])
  - isNaN

---

## 3. 类型转换

- <span style="color:#cc0033">Others => Number</span>
  - Number([val])
    - 隐式转换的调用方法（如```isNaN()```）
    - String:出现非有效数字字符即为```NaN```，空串为```0```
    - null:```0```
    - undefined:```NaN```
    - boolean:```true==1 false==0```
    - BigInt:去除```n```, 超过安全数字处理为科学计数法
    - Symbol->Uncaught TypeError: Cannot convert a Symbol value to a number
  - parseInt([val],[radix])/parseFloat([val])
    - [val] 必须是一个字符串，否则隐式转换
    - [radix] 不设置或为0，按照10处理，若字符串'0x'开头，按照16处理
    - 处理过程：在[val]中找到所有符合[radix]进制的内容，直到遇到不符合的字符，把找到的内容按照[radix]进制转换为10进制
    - [radix]范围：2~36,包括0,否则```NaN```
  - 隐式转换
    - inNaN()
    - <span style="color:#2a5caa">数学运算(特殊："+"的字符串拼接)</span>
    - "=="
- <span style="color:#cc0033">Object => Number/String</span>
  1. 检测```Symbol.toPrimitive```，获得原始值
  2. 第一步没有，```valueOf```获取原始值，不是原始值则继续第三步
  3. 第二步没有，调用```toString```，字符串
  4. 第三步后，若有需要，基于```Number()```，数字
- <span style="color:#cc0033">Others => String</span>
  - 规则:
    - 原始值，其他对象用引号包含
    - 标准普通对象 => ```"[object Object]"```
    - 标准普通对象 => 字符串
    - JSON.stringify <=> JSON.parse
    - Qs.stringify <=>  
  - 隐式转换：
    - '+':
      - 两边:出现字符串
      - 两边:出现对象
        - 按Object => Number/String处理, toString获取了字符串则拼接
      - 一边：左边为空
        - 数学运算
- <span style="color:#cc0033">Others => Boolean</span>
  - 规则：只有``` 0,NaN,null,undefined,"" ```为```false```
  - ! !!
  - Boolean([val])
  - 循环或条件判断（隐式转换）
  - A||B A&&B
    - A && B
      - 如果第一个操作数是对象，则返回第二个数
      - 如果第二个操作数是对象，则只有在第一个操作数的求值结果为true的情况下才会返回该对象。
      - 如果两个操作数都是对象，则返回第二个数操作数。
      - 如果有一个操作数是null，则返回null。
      - 如果有一个操作数是NaN，则返回NaN。
      - 如果第一个操作数是undefined，则返回undefined。
      - 对于逻辑与，如果第一个操作数是false，无论第二个操作数是什么，结果都不可能再是true。
    - A || B
      - 如果第一个操作数是对象，则返第一个操作数
      - 如果第一个操作数的求值结果为false，则返回第二个操作数
      - 如果两个操作数都是对象，则返回第一个操作数
      - 如果两个操作数是null，则返回null
      - 如果两个操作数是NaN，则返回NaN
      - 如果两个操作数是undefined，则返回undefined
- <span style="color:#cc0033">"=="的规则</span>
  - 类型相同
    - ```{}=={}``` :false 比较堆内存地址，地址相同则相等
    - ```[]==[]``` :false
    - ```NaN==NaN``` :false ```Object.is(NaN, NaN)``` :true
  - 类型不同
    - ```null==undefined``` :true ```===``` :false null/undefined与其他类型不相等
    - 字符串==对象 对象=>字符串
    - 两边类型不一致，先转换为数字再比较
- <span style="color:#cc0033">其它</span>
  - "+"
    - {} + 0 左边{}视为代码块 // +0 => 0
    - ({} + 0)  // "[object Object]0"
    - 0 + {} // "0[object Object]"
  - 模板字符串实现的是字符串拼接，对象转换为字符串，其余数学运算则对象=>数字

---

```js
//var a = ?; 

//方案一：
var a ={
  i:0
};
a[Symbol.toPrimitive] = function toPrimitive(){
  //this -> a
  return ++this.i;
}

//方案二：
var a =[1,2,3];
a.toString = a.shift;

//方案三：
var i = 0;
Object.defineProperty(window,'a',{
  get(){
    return ++i;
  }
});

if(a==1 && a==2 && a==3){
  console.log('OK');
}
```
