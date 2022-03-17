---
title: "JavaScript Iterator Generator"
date: 2021-09-22T20:00:00+08:00
draft: false
---

- [1. Iterator](#1-iterator)
- [2. Generator](#2-generator)
 
# 1. Iterator

- Iterator是一种机制，遍历各种不同的数据，依次处理成员
    - next方法遍历
    - 每次遍历返回一个对象 { done:false, value: xxx}
        + done：记录是否完成
        + value：当前遍历的结果

- 拥有Symbol.iterator，基于for of可遍历
    - 对象默认不具备
    - 数组
    - 部分类数组 arguments/ Nodelist/ HTMLCollection...
    - String
    - Set
    - Map
    - generator object
    - ...(展开运算符)
```js
// for of 
// 首先调用Symbol.iterator，得到一个迭代器对象itor
// 每一轮循环itor.next()
    // value 
    // done: true结束循环
arr[Symbol.iterator] = function () {
    let index = 0,
        self = this;
    return {
        next() {
            if (index > self.length - 1) {
                return {
                    done: true,
                    value: undefined
                };
            }
            let result= {
                done: false,
                value: self[index++]
            };
            return result;
        }
    }
}
//基于for...of... 遍历对象
// 法1：手动设置Symbol.iterator

// 法2： [Symbol.iterator]= Array.prototype[Symbol.iterator]

// 法3： Object.prototype[Symbol.iterator]
Object.prototype[Symbol.iterator] = function () {
    let self = this,
        keys = [
            ...Object.getOwnPropertyNames(self),
            ...Object.getOwnPropertySymbols(self)
        ],
        index = 0;
    return {
        next() {
            return index > keys.length - 1 ? {
                done: true,
                value: undefined
            } : {
                    done: false,
                    value: self[keys[index++]]
                };
        }
    };
};

```
# 2. Generator
```js
// 创建函数时，如果在function后面设置 * ，则为生成器函数
//      + 生成器函数执行，函数体代表未立即执行，创建了[fn]的一个实例
//         itor.__proto__ ===fn.prototype
//      + itor符合迭代器规范
//          + next() 执行函数体代码
//          + return() 提前执行结束
//          + throw() 抛出异常
//      + Object.prototype.toString.call() => [object Generator]
//      + yield: 每次执行next()，遇到yield结束，yield的值给对象中的value;
function *fn(){

}
fn.prototype.name = "Margin";
let itor = fn();
console.log(itor);

// =====================
function* generator(){
    let x1 = yield 10;
    console.log(x1);
    let x2 = yield 20;
    console.log(x2);
    return 30;
}
let itor = generator();
// 每一次next传递的值，是给上一个yield赋值的返回值
console.log(itor.next('AA')); // 10 false   --
console.log(itor.next('BB')); // 20 false  BB
console.log(itor.next('CC')); // 30 true   CC
console.log(itor.next('DD')); // undefined true --

// =======================
function* func1() {
    yield 1;
    yield 2;
}

function* func2() {
    yield 3;
    yield* func1();
    yield 4;
}
let iterator = func2();
console.log(iterator.next()); //3
console.log(iterator.next()); //1
console.log(iterator.next()); //2
console.log(iterator.next()); //4
console.log(iterator.next()); // undefined
```