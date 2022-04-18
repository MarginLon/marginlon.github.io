---
title: "GitHub Pages绑定个人域名"
description:
toc: true
authors:
tags: 
  - github
  - github page
categories:
  - github page
series:
  - github
date: '2020-09-18T16:28:39+08:00'
lastmode: "2020-09-18T16:28:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---

简单叙述一下Github Pages换域名的流程，以供日后使用。

--------

## 域名解析配置

- 注册域名，实名认证<br/>
- 域名解析：<br/>

>`增加新纪录`<br/>
>`记录类型：CNAME`<br/>
>`主机记录：@`<br/>
>`记录值:username.github.io`<br/>

## 部署Github流程<br/>

>`Settings => GitHub Pages => Custom domain => 域名`<br/>
>`# 稍等片刻即可通过域名访问GitHub Pages`<br/>
