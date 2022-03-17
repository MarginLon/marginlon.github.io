---
title: "JavaScript 浏览器渲染机制 CRP优化 事件循环机制"
date: 2021-09-17T20:00:00+08:00
draft: false
---
- [1.浏览器渲染机制 CRP优化](#1浏览器渲染机制-crp优化)
- [2.事件循环机制](#2事件循环机制)

# 1.浏览器渲染机制 CRP优化
- 从服务器基于HTTP网络请求回来的数据
   - 16进制的文件流
   - 浏览器解析 => HTML字符串
   - 按照W3C规则识别为HTML标签
   - 节点Nodes
   - 生成DOM树 

- 浏览器线程：
  - GUI渲染线程
  - JS引擎线程
  - HTTP线程
  - 定时器监听线程
  - DOM监听线程
  - ...
 
- 访问页面，请求回来一个HTML文档，浏览器自上而下渲染    
    - 遇到style，GUI直接渲染
      - 少量CSS可以内嵌
    - 遇到link，HTTP线程请求资源文件信息，同时GUI继续向下渲染
      - HTTP请求有数量限制,越少越好，link放在头部，为了在没有渲染DOM时，通知HTTP请求CSS，提高渲染速度。
    - 遇到@import, HTTP线程请求资源文件信息，GUI暂时停止。
      - 避免使用@import
- 遇到```<script src = 'xxx.js'>```，阻碍GUI继续渲染。
    -  defer:请求JS资源是异步的，GUI渲染完，JS渲染
    -  async:请求JS资源是异步的，此时GUI继续渲染，JS请求回来，GUI暂停，JS渲染
    -  多个JS请求按顺序渲染，但设置async时，先请求回先渲染
    -  js放在底部，防止GUI被阻止，放在非底部尽量设置defer/async
![defer和async](https://github.com/MarginLon/MarginPostImage/blob/master/async%20defer.png?raw=true)
- Webkit浏览器预测解析：chrome的预加载扫描器html-preload-scanner通过扫描节点中的 “src” , “link”等属性，找到外部连接资源后进行预加载，避免了资源加载的等待时间，同样实现了提前加载以及加载和执行分离。
  
- DOM Tree(DOMContentLoaded事件触发) => 执行JS？ => CSSOM Tree -> RENDER TREE渲染树 -> Layout布局计算[回流/重排] -> Painting绘制[重绘]{ 分层绘制 }
  - 页面第一次渲染，必定一次回流和重绘
  - 如果元素的位置和大小改变，浏览器需要重新计算在视口中的位置和大小信息，重新计算的过程时回流重绘
  - 普通样式改变，位置和大小不变，只需要重绘
- 减少回流：
  - 基于vue/react
  - 分离读写操作
    - 新浏览器：渲染队列 => 1.修改样式的代码结束 2.遇到获取元素样式的代码 => 触发回流重绘 
  - 样式集中改变
  - 元素批量修改
    - createDocumentFragment() 
    - 模板字符串拼接
  - 动画影用到absoluted和fixed的元素上，脱离文档流

---
# 2.事件循环机制

JS：单线程异步编程
  - EventLoop
  - EventQueue
  - WebAPi
  - macrotask
    - 定时器
    - 事件队列
    - xmlHttpRequest/Fetch
    - setImmediate [Node]
    - ...
  - microtask 
    - requestAnimationFrame
    - Promise.then/catch/finally
    - async/await
    - queueMicrotask
    - process.nextTick [Node]
    - MutationObsever
    - ...
![宏任务微任务](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%BC%82%E6%AD%A5%E7%BA%BF%E7%A8%8B.png?raw=true)
