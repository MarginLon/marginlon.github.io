---
title: "CSS 定位和层叠上下文原理"
date: 2021-10-02T22:02:39+08:00
draft: false
---
- [1.定位](#1定位)


# 1.定位

- position
  - static(default)
  - relative
    - 参照物：自身
    - 不脱标
    - 使用情形：
      1. 微调元素，方位和z-index能够生效
      2. 做absolute的参考，子绝父相 
  - absolute
    - 参照物：父
    - 脱标
      - top:参照页面左上角
      - bottom：参照首屏窗口左下角
    - 父元素如果是static，则相对于html
    - 在父元素里居中
      - ```left:50%;margin-left:-width/2;```
      - ```transform: translate(-50%); ```
  - fixed
  - sticky(2017)
    - 动态固定
