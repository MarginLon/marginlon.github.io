---
title: "Node"
date: 2021-09-12T20:00:00+08:00
draft: false
---
- [1. 模块化规范](#1-模块化规范)
- [2. Node 内置包](#2-node-内置包)

# 1. 模块化规范
- CommonJS
  - require exports module __dirname __filename
  - 导入：= require('')
  - 导出：module.exports =
- ES6Module
  - ```<script type='module'></script>```
  - 导入： import {} from '' /import xxx from ''
  - 导出： export {let var const function class}/export default

--- 
# 2. Node 内置包
- 全局变量 : global process 
- commander.js
  - npm i commander
- path
- url
- fs
- http
- Buffer
  - 缓冲区，暂时存放输入输出数据的一段内存 
- 流
- egg