---
title: "JavaScript Promise Async/Await"
date: 2021-09-20T20:00:00+08:00
draft: false
---

- [1. Promise](#1-promise)
- [2. Async/Await](#2-asyncawait)
 
# 1. Promise

- Promise
  ```js
  // PromiseState: pending/fulfilled/rejected
  // PromiseResult: undefined
  // new Promise 立即执行传递的executor函数
  /*
    实例.then(onfulfilled,onrejected)
      1. 观察实例状态，如果已知成功或者失败
        + 创建一个异步的“微任务”[放在webapi监听，监听时知道状态，所以直接把执行的哪个方法，挪至EventQueue中排队等待]
        + 状态成功执行onfulfilled
        + 状态失败执行onrejected
      2. 如果此时状态是pending
        + 把onfulfilled/onrejected存储到指定容器中 [放在WebAPI监听状态改变]
        + 执行resolve/reject时
          + 立即修改实例的状态和值[同步]
          + 创建一个异步微任务,让指定容器中的方法执行 [挪至EventQueue中等待]

    new Promise产生的实例，状态是成功还是失败，由 executor执行是否报错{执行报错,则实例是失败态}、以及resolve还是reject执行决定
    Promise.resolve(100)：直接创建状态是成功的实例
    Promise.reject(100)：直接创建状态是失败的实例

    "实例.then"会返回一个全新的promise实例p2,这个实例的成功失败，由p1管控的onfulfilled或者onrejected不论哪个方法执行决定:
      + 方法执行是否返回新的promise实例，如果没有返回:
        + 方法执行不报错，p2成功，值是返回值
        + 报错，p2失败，值是报错原因
      + 如果返回新的promise，则新promise[p3]的状态和值决定了p2的状态和值
  */
  let p1 = new Promise(function executor(resolve, reject){
    resolve(100);
    // reject(0);
  });
  p1.then(function onfulfilled(value){
      console.log('OK',value);
  }, function onrejected(reason){
      console.log('No',reason);
  });
  
  p1.then(value =>{}).catch(reason =>{});
  ```

---

# 2. Async/Await

```js
/*
 * - async/await : promise + generator
 *  + async修饰的函数，默认返回值会变成一个promise实例
 *  + 必须在函数中使用await，当前函数需要async修饰
*/
(async function(){
    // await 后面放置 promise实例 [如果不是，则默认变为状态成功，值为此值的promise]
    // 把当前上下文中，await下面的代码作为一个异步微任务，放在webAPI中监听，监听实例状态
    // 如果状态成功，则把下面的代码挪至EventQueue中排队
    // 如果状态失败，则下面代码不会执行
    let value = await p1;
})();

(async function () {
    // 捕获await后面实例是失败的?
    try {
        let value = await p1;
        console.log('成功:', value);
    } catch (err) {
        console.log('失败:', err);
    }
})(); 
```