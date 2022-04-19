---
title: "JavaScript DOM"
description:
toc: true
authors:
tags:
- JavaScript
- DOM
categories:
- DOM
series:
- JavaScript
date: "2021-04-17T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false

---
- [1. {DOM}](#1-dom)

## 1. {DOM}

- 基本类型
  1. <font color="00cdcd">Document</font>
  2. <font color="00cdcd">DocumentType</font>:doctype标签
  3. <font color="00cdcd">Element</font>:H5标签
  4. <font color="00cdcd">Attr</font>:元素的属性
  5. <font color="00cdcd">Text</font>:标签内文本
  6. <font color="00cdcd">Comment</font>:注释
  7. <font color="00cdcd">DocumentFragment</font>:文档片段
- 获取方法
  1. <font color="00cdcd">document.getElementById()</font>:基于元素ID
  2. <font color="00cdcd">[context].getElementByTagName()</font>:基于标签名,获取集合
  3. <font color="00cdcd">[context].getElementByClassName()</font>:基于类名，获取集合
  4. <font color="00cdcd">document.getElementByName()</font>:基于NAME属性值，一般只用于表单（IE只认表单的NAME）
  5. <font color="00cdcd">document.head/document.body/document.documentElement</font>
  6. <font color="00ffff">[context].querySelector([selector])</font>
  7. <font color="00ffff">[context].querySelectorAll([selector])</font>
- 关系属性
  - 类型
    - Node
    - NodeList(ByTagName/ByClassName/querySelectorAll)
  - nodeType:元素1/属性2/文本3/注释8/文档9
  - nodeName:
  - nodeValue:
  - childNodes:获得所有子节点
  - children:获得所有元素子节点
  - firstChild/lastChild:
  - firstElementChild/lastElementChild:
  - previousSibling/nextSibing:
  - previousElementSibling/nextElementSibling:
- 增删改
  - createElement
  - createTextNode
  - appendChild
  - insertBefore
  - cloneNode: true|false
  - removeChild
  - setAttribute
- 事件
  - ```onclick```
  - ```ondblclick```
  - ```onkeyup```
  - ```onchange```
  - ```onfocus```
  - ```onblur```
  - ```onmouseover```
  - ```onmouseout```
  - ```onload```
  - ```onunload```
  - ```onsubmit```
  - ```onreset```

---
