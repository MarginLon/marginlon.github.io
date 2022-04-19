---
title: "JavaScript 数据结构"
description:
toc: true
authors:
tags:
- JavaScript
- 数据结构
categories:
- 数据结构
series:
- JavaScript
date: "2022-04-09T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false
---

- [1. 数组 Array](#1-数组-array)
- [2. 栈](#2-栈)
- [3. 队列](#3-队列)
- [4. 排序](#4-排序)

## 1. 数组 Array

- JS数组可以存储不同数据类型
- 容量随存储内容自动缩放
- Array.prototype的API

```js
//封装数组

```

---

## 2. 栈

```js
class Stack{
    container = [];
    enter(item){
        this.container.push(item);
    }
    leave(){
        return this.container.pop();
    }
    size(){
        return this.container.length;
    }
    value(){
        return this.container.slice(0);
    }
}

// 十进制转二进制

Number.prototype.decimal2binary = function decimal2binary (){
    //this-> new Number()
    let decimal = +this;
    if(/\./.test(decimal)){
        //包含小数
        throw new RangeError('应传入整数');1
    }
    let sk = new Stack;
    if(decimal===0) return '0';
    while(decimal>0){}
        sk.enter(decimal%2);
        decimal = Math.floor(decimal/2);
    }
    return sk.value().reverse().join('');
};

```

---

## 3. 队列

```js
class Queue{
    container=[];
    enter(item){
        this.container.unshift(item);
    }
    leave(){
        return this.container.pop();
    }
    size(){
        return this.container.length();
    }
    value(){
        return this.container.slice(0);
    }
}

//击鼓传花
function game(n,m){
    let qe = new Queue;
    for(let i = 1; i<=n ; i++){
        qe.enter(i);
    }

    //队列中存在两个及以上的人，重复
   while (qe.size() > 1) {
        for (let i = 1; i < m; i++) {
            //前m-1个人出队列后进去队列
            qe.enter(qe.leave());
        }
        //第m个人出队列
        qe.leave();
    }

    return qe.value()[0];
}
console.log(game(8, 5));
```

---

## 4. 排序

```js

let arr = [1,7,8,6,5];

//Bubble Sort
function bubbleSort(arr){
    for(let i =0;i<arr.length;i++){
        for(let j=0;j<arr.length-1;j++){
            if(arr[j]>arr[j+1]){
                [arr[j],arr[j+1]]=[arr[j+1],arr[j]];
            }
        }
    }
}

//Insert Sort
function insertSortArray(arr,i,t){
    let p = i-1;
    while(p>=0 && arr[p]>t){
        arr[p+1] = arr[p];
        p--;
    }
    arr[p+1] = t;
}

function insertSort(arr){
    var j;
    for(let i = 1;i <arr.length;i++){
        insertSortArray(arr,i,arr[i]);
    }
}
```

---
