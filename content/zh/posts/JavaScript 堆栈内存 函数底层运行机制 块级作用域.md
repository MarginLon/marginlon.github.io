---
title: "JavaScript 堆栈内存 函数底层运行机制 块级作用域"
description:
toc: true
authors:
tags:

- JavaScript
- 堆栈内存
categories:
- JavaScript
series:
- JavaScript
date: "2022-04-11T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false

---

- [1. 堆栈内存](#1-堆栈内存)
- [2. 函数底层运行机制](#2-函数底层运行机制)
- [3. 块级作用域](#3-块级作用域)

## 1. 堆栈内存

![堆栈内存(例)](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%A0%86%E6%A0%88%E5%86%85%E5%AD%98(%E4%BE%8B).png?raw=true)

- 栈内存(Stack)
  - 执行环境栈 EC stack (execution context stack)
    - EC(G) 全局执行上下文，全局代码执行
      - VO(G) 全局变量对象[存储全局上下文中声明的变量]
        - 特殊性：var/function声明的变量存于GO中, let/const存于VO(G).
    - EC(X) 某函数的执行上下文

- 堆内存(Heap)
  - 赋予一个16进制地址，对象中的键值依次存储，内存地址放在栈中供变量引用。
  - GO(global object) 全局对象 [window]

- 等号赋值
  1. 创建值  
  原始值类型的值，储存在栈内，对象数据类型单独开辟堆内存  
  2. 声明变量  
  把声明的变量存储到当前上下文的变量对象中，如EC(G)
  3. 赋值  
  指针的指向，对象类型赋值内存地址

- 全局上下文遇到变量
  - VO(G) => GO => Error
  - window.xxx => GO => undefined

---

## 2. 函数底层运行机制

![函数底层运行机制](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%87%BD%E6%95%B0%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.png?raw=true)

- 正常的函数创建 ```function fn(y){...}```
- 匿名函数之函数表达式 ```var fn = function(y){...};```
- 创建步骤：
    1. 开辟堆内存空间
    2. 声明函数作用域scope，存储代码（字符串），存储对象键值对（函数名，形参个数）。
    3. 16进制地址存入栈中以供引用
- 执行步骤
    1. 形成一个全新的私有context => EC(FN) AO(FN)&emsp;进栈 （AO:ActiveObject 变量对象）
    2. 初始化作用域链[Scope-chain]
        - <当前自己的私有context，函数的作用域> => 链的右侧是当前context的“上级”context
    3. 初始化this
    4. 初始化arguments
    5. 形参赋值：当前上下文声明一个形参变量，并传递实参值，非严格模式建立映射机制：集合中的每一项和对应的形参变量绑定（只在代码执行之前）。
    6. 变量提升
       - var/function 提前声明/定义
       - 基于var/function 全局context 声明的变量映射到GO(window)一份，并随动。
       - if 无论条件是否成立，都变量提升（条件中带function在新浏览器只提前声明，不提前赋值）  

       ```js
       var func = function AAA() {
        // 函数表达式匿名函数“具名化”，不能外部访问
        // 函数内部私有context会把名字作为context的变量
        // AAA(); 递归调用 而非arguments.callee
       }
       ```

    7. 代码执行
        - 变量：私有则只操作自己，和外界无关；如果不是自己私有，则基于chain向context查找，是否为上级私有，如果不是，继续向上查找，直到EC(G)，即<span style="color:red">作用域链查找机制</span>
    8. 出栈 or 不出栈

---

## 3. 块级作用域

- 除“函数和对象”的大括号外,出现了 let/const/function/class 等关键词声明变量，产生一个“块级私有context”
- 块级作用域内默认变量
  - 不带var，let，const，只有执行过定义的变量的代码后才可以访问，之前会报错,window对象上为undefined
  - 块内默认变量依旧是全局变量
  - 在块内的默认变量没执行之前不可以访问这个变量
- 块级作用域函数声明
  - <span style="color:red">块内函数声明会提升到块顶部，也会在全局作用域用var声明一个同名的undefined变量</span>
  - 块外的全局同名变量的赋值时机是执行完块内函数声明语句
  - <span style="color:red">执行到函数声明时，这行代码以前的所有操作同步给全局一份，之后的操作只与私有context有关。</span>
- 块内同时有同名的默认变量和函数声明
  - 块内的函数声明每次执行的时候都会给全局那个同名的变量赋值一次，并且，只有执行那个定义函数声明的代码才会触发赋值，你写的函数声明就相当于setter,每执行一次就给外部的那个同名的变量赋值一次
  - 如果块内同时有同名的函数声明和默认的变量声明，那给默认的变量赋值时其实相当于赋值给那个同名的函数，因为查找块内的作用域链时找到了,就不会往全局声明了。
  ![块级作用域_变量](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F_%E5%8F%98%E9%87%8F.png?raw=true)
  ![块级作用域_函数](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F_%E5%87%BD%E6%95%B0.png?raw=true)
