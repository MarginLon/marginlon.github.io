---
title: "JavaScript 函数防抖 函数节流"
date: 2021-09-13T20:00:00+08:00
draft: false
---
- [1. 函数防抖](#1-函数防抖)
- [2. 函数节流](#2-函数节流)

# 1. 函数防抖

- 防抖：只识别一次  --点击事件
```js
let submit = document.querySelector("#submit");

//简易处理:加标识 不适于通用
submit.isLoading = false;
submit.onclick = function () {
    if (this.isLoading) return;
    this.isLoading = true;
    console.log('数据请求发送');
    setTimeout(() => {
        console.log('数据请求成功');
        this.isLoading = false;
    }, 5000);
};

/*
debounce:函数防抖
  @params
    func [funcion,required] : 最后要执行的函数
    wait [number] 触发的频率时间
    immediate [boolean] 设置是否开始边界触发
  @return
    func执行的返回结果
*/
const clearTimer = function clearTimer(timer) {
    if (timer) clearTimeout(timer);
    return null;
};

const debounce = function debounce(func, wait, immediate) {
    if(typeof func!=="function") throw new TypeError('func must be an function');
    if(typeof wait ==="boolean") {
        immediate = wait;
        wait = 300;
    }
    if(typeof wait !=="number") wait =300;
    if(typeof immediate !=="boolean") immediate = false;

    let timer = null;
    return function operate(...params) {
        let now = !timer && immediate,
            result;
        timer = clearTimer(timer);
        timer = setTimeout(() => {
            // This是实参没有debounce前一致的
            // 结束边界
            if(!immediate) func.call(this, ...params);
            //清最后一次设定的定时器
            timer = clearTimer(timer);
        }, wait);
        // 立即执行，开始边界
        if (now) {
            result = func.call(this, ...params);
        }
        return result;
    };
};
```
---

# 2. 函数节流
- 节流：控制触发频率，能识别‘多次’ --键盘 滚动条
```js
/*
throttle:函数节流
  @params
    func [funcion,required] : 最后要执行的函数
    wait [number] 触发的频率时间
  @return
    func执行的返回结果
*/
const clearTimer = function clearTimer(timer) {
    if (timer) clearTimeout(timer);
    return null;
};


const throttle = function throttle(func, wait) {
    if (typeof func !== "function") throw new TypeError('func must be an function');
    if (typeof wait !== "number") wait = 300;
    let timer = null,
        previous = 0;
    return function operate(...params) {
        let now = +new Date(),
            remaining = wait - (now - previous),
            result;
        if (remaining <= 0) {
            // 两次触发的间隔超过wait，执行函数
            result = func.call(this, ...params);
            // 记录当前触发时间
            previous = +new Date();
            timer = clearTimer(timer);
        } else if (!timer) {
            //如果没有设置过定时器，两次触发的间隔也不足wait，此时设置定时器，等待remaining后执行一次
            timer = setTimeout(() => {
                func.call(this, ...params);
                previous = +new Date();
                timer = clearTimer(timer);
            }, remaining);
        }

        return result;
    };
};

```
---
