---
title: "JavaScript 内置对象"
date: 2021-09-01T20:00:00+08:00
draft: false
---

- [1. {String}](#1-string)
- [2. {Number}](#2-number)
- [3. {Array}](#3-array)
 
# 1. {String}

- 字符串的方法不会改变原串，会返回一个新值

```js
//查找
// indexOf()/lastIndexOf():
@params:(str,startIndex);
@return:index|-1;

// search():
@params:(Reg|str);
@return:index|-1;

// includes():
@params:(str,[position|0]);
@return:Boolean;

//startsWith()/endsWith():
@params:(str,[position|0]);
@return:Boolean;

//获取字符
// charAt(): = str[index]
@params:(index);
@return:str|'';

// charCodeAt():
@params:(index);
@return:str|'';

// 截取
// slice():
@params:(startIndex,endIndex);
@return:str;
@notes:包左不包右，负值表示倒数

// substring():
@params:(startIndex,endIndex);
@return:str;
@notes:包左不包右，右>左则交换，不接受负值

// substr():
@params:(startIndex,length);
@return:str;

// 其他：
// fromCharCode():
@params:(Unicode);
@return:str;

// concat():
@params:(str);
@return:str+str;

// split():
@params:(str| );
@return:Array;

// replace():
@params:(str1,str2);
@return:str;

// repeat():
@params:(num);
@return:str;

// trim(): 去除空白
// toLowerCase()
// toUpperCase()
```

# 2. {Number}

```js
// isInteger():
@params:(number);
@return:Boolean;

// toFixed():
@params:(number);
@return:

// Math 工具类
PI
abs()
random()
floor()
ceil()
round()
max()
min()
pow()
sqrt()
```

# 3. {Array}

```js
Array.isArray();
toString();
Array.from(arrayLike);
Array.of(val1,val2,val3);

// push():向数组最后插入N个元素
@params:(items)
@return:new length;
// pop():删除数组的最后一个元素 
@params:()
@return:delete item;

// unshift():向数组最前插入N个元素
@params:(items)
@return:new length;
// shift():删除数组中的第一个元素
@params:()
@return:delete item;

// slice():从数组中提取指定的一个或者多个元素
@params:(startIndex,endIndex)
@return:new Array;
@notes: arrayLike => Array
// 方式1
array = Array.prototype.slice.call(arrayLike);
// 方式2
array = [].slice.call(arrayLike);
// splice():从数组中删除指定的一个或多个元素，
@params:(startIndex,num,[item1,item2...])
@return:delete item;

// fill():
@params:(content,startIndex,endIndex)
@return:new Array;

// concat():
@params:(Array1,Array2...)
@return:new Array;
// join():
@params:(params)
@return:str;
// split():
// reverse():

// sort():
// 浏览器根据回调函数的返回值来决定元素的排序：（重要）

// 如果返回一个大于 0 的值，则元素会交换位置

// 如果返回一个小于 0 的值，则元素位置不变

// 如果返回一个等于 0 的值，则认为两个元素相等，则不交换位置
arr.sort((a, b) => a - b);

// indexOf/lastIndexOf/includes

// find():找出第一个满足「指定条件返回 true」的元素；如果没找到，则返回 undefined。
find((item, index, arr) => {
    return true;
});
// findIndex():找出第一个满足「指定条件返回 true」的元素的 index。

// every()/some()

// 遍历
// for
// forEach 用arr和index修改引用类型以修改原数组
// map map方法如果是修改整个item的值，则不会改变原数组。但如果是修改 item 里面的某个属性，那就会改变原数组。
// filter() 
// reduce(function (previous,current,currentIndex,arr){},initial)
```
