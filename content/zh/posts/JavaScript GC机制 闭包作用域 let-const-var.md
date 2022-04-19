---
title: "JavaScript GC机制 闭包作用域 let/const/var"
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
date: "2022-04-12T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false

---

- [1. GC机制](#1-gc机制)
- [2. 闭包作用域](#2-闭包作用域)
- [3.let/const/var](#3letconstvar)

## 1. GC机制

- GC：浏览器的垃圾回收机制（内存释放）
  - 栈内存：
    - 加载页面，形成全局context，只有页面关闭后，全局context释放
    - 函数执行私有context，进栈执行；函数中代码执行完成，大部分情况，出栈释放。
      - 特殊：如果当前context的某些内容（一般是一个堆），被context以外内容占用，则不能出栈释放。
  - 堆内存
    - （谷歌）：标记清除
      - 浏览器在空闲或指定时间，查看所有堆内存，把没有被任何东西占用的堆内存释放。
    - （IE低版本）：引用计数
      - 创建了堆内存，被占用一次，则浏览器计数+1，取消占用计数-1，计数=0释放内存； => 某些情况（循环占用）导致计数混乱，出现“内存泄漏”
  - 释放内存：
    - x=null => 手动取消占用

---

## 2. 闭包作用域

![闭包案例](https://github.com/MarginLon/MarginPostImage/blob/master/%E9%97%AD%E5%8C%85%E6%A1%88%E4%BE%8B.png?raw=true)

- EC(FN)：
  - 开辟的某个堆内存，被当前EC(FN)外的变量占用，此时当前context即EC(FN)不能被出栈释放
  - 闭包：(大函数执行返回小函数只是其中一种形式)
    - 函数执行产生一个私有context，保护内部私有变量不受污染。[保护]
    - 有可能形成不被释放的context，私有变量和一些值被保存，可供下级context中调取使用。[保存]
    - 闭包特点
      - 保护：保护私有context的私有变量和外界互不影响
      - 保存：context的私有变量和值保存起来
      - 弊端：栈内存太大，影响性能，需合理利用

---

## 3.let/const/var

- let VS const
  - let 变量储值可改
  - const 赋值不能再与其他赋值关联
- let VS var
  - 变量提升：var 存在变量提升 let 不存在变量提升
  - GO：全局context var相当于给GO增一个属性，随动；let和GO没有关系。
  - 重复声明：let不允许同context重复声明（不管先前何种方式声明，都不能let声明,词法解析阶段），var无所谓。
  - let/const/function产生块级私有context
    - context & 作用域
      - 全局context
      - 函数执行的私有context
      - 块级作用域（私有context）
        - 对象 function的大括号 判断，循环和代码块的大括号

                ```js
                    debugger;//开启断点调试,控制台基于F10/F11 逐过程/逐语句 控制执行
                    //代码块不会对n产生限制，n使全局context
                    {
                        var n = 12;
                        console.log(n);

                        let m = 13;
                    }
                    console.log(n);
                    console.log(m);// m is not defined


                    let i = 0;//不产生块级context
                    for (; i< 5;i++)
                    {
                        console.log(i);
                    }
                    console.log(i);//5
                ```
  - 暂时性死区（基于typeof检测一个未被声明的变量，不会报错，结果是'undefined'）
    - 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

        ```js
        console.log(typeof n);//undefined
        let n = 12;//使上一句报错 
        ```

    ![var](https://github.com/MarginLon/MarginPostImage/blob/master/var%E5%8F%98%E9%87%8F.png?raw=true)

---
