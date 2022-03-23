---
title: "QSS文档"
descripition: QSS文档
toc: true
authors:
tags:
categories:
series:
date: '2022-03-07'
lastmod: '2022-03-07'
draft: false
---
QSS，即Qt样式表（Qt Style Sheet），Qt 样式表是允许定制 Widget 外观的强大机制，Qt 样式表的概念、术语和语法深受HTML CSS (级联样式表)启发，其语法与CSS相似。  
  

Qt 提供的 widget 的默认外观很多时候都不符合项目的界面需求，必须要改，修改一个 widget 的外观（Look and Feel）有以下的方法：  
	&emsp;&emsp;- 继承 Widget，然后在 paintEvent() 里绘制  
	&emsp;&emsp;- 继承 QStyle  
	&emsp;&emsp;- 使用 QSS（Qt Style Sheet）  
	&emsp;&emsp;- 对于 item view 来说还有一种方式，还可以使用 Delegate  
- Referrence  
[1] https://qtdebug.com/all/  
[2] 《Qt Style Sheet 开发总结》 --张涛
