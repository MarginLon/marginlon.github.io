---
title: "JavaScript 工厂设计模式 数组和对象的深浅拷贝 深浅合并"
date: 2021-09-16T20:00:00+08:00
draft: false
---
- [1.工厂设计模式](#1工厂设计模式)
- [2.数组和对象的深浅拷贝](#2数组和对象的深浅拷贝)
- [3.深比较浅比较合并](#3深比较浅比较合并)
# 1.工厂设计模式
```js
    var jQuery = function( selector, context ) {
		return new jQuery.fn.init( selector, context );
        };
    jQuery.fn = jQuery.prototype = {
        jquery: "3.6.0",
        constructor: jQuery
    };
    init = jQuery.fn.init = function( selector, context, root ){};
    init.prototype = jQuery.fn;
```
![工厂设计模式](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%B7%A5%E5%8E%82%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.png?raw=true)

---
# 2.数组和对象的深浅拷贝
![深浅拷贝](https://github.com/MarginLon/MarginPostImage/blob/master/%E6%B7%B1%E6%B5%85%E5%85%8B%E9%9A%86.png?raw=true)
```js

//======================浅克隆：只克隆第一级信息，第二级及以后的信息共用
    let obj1 = {
        name:"Margin",
        course: {
            c1:'CSS',
            c2:'JS'
        }
        fn: function(){
            console.log(this.name);
        }
    };
    // 克隆1：循环遍历 【浅】
    let obj2 ={},
        keys = [
            ...Object.keys(obj1),
            ...Object.getOwnPropertySymbols(obj1)
        ];
    keys.forEach(key =>{
            obj2[key] = obj1[key];
        });
    // 克隆2：展开运算符 【浅】
    let obj2 = {
            ...obj1
        };
    // 克隆3：assign 【浅】
    // 原理 Object.assign([obj1],[obj2]); 
    // => 返回结果仍是obj1堆内存，只不过是把obj2中的键值对和obj1的键值对合在一起
    let obj = Object.assign({},obj1);
//======================数组浅拷贝 
    let arr = [10, 20, [30,40]];

    // 克隆1：循环 for each [浅]
    let arr2 = []; 
    arr1.forEach((item, index) => {
        arr2[index] = item;
    });

    arr2 = arr1.map(item => item);

    // 克隆2：展开运算符 Object.assign 【浅】

    // 克隆3：基于slice/concat 【浅】
    let arr2 = arr1.slice();

//======================深克隆：每一级信息都克隆
    // 克隆1：JSON.stringfy / JSON.parse

    // 原理：基于JSON.stringfy把原对象（数组）变为字符串，再基于JSON.parse把字符串转化为对象或者数组。
    // 问题: 1.正则，Math，ArrayBuffer对象 => 空对象； 
    //      2.函数/Symbol/undefined属性值的属性 => 消失；  
    //      3. BigInt => 不能处理； 
    //      4. Date => String 
    //      5. arrayBuffer等也存在问题
    let obj2 = JSON.parse(JSON.stringfy(obj1));
```
 
# 3.深比较浅比较合并