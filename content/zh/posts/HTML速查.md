---
title: "HTML速查"
description:
toc: true
authors:
tags: 
  - html
categories:
  - html
series:
  - html
date: '2020-10-01T22:02:39+08:00'
lastmode: "2020-10-01T22:02:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---
- [1.HTML速查](#1html速查)
- [2.规范](#2规范)
- [3.html5骨架](#3html5骨架)

## 1.HTML速查

```html
<!-- 文档 -->
<!DOCTYPE html>
<html>
<head>
<title>文档标题</title>
</head>
<body>
可见文本...
</body>
</html>
```

```html
<!-- 基本标签 -->
<h1>最大的标题</h1>
<h2> . . . </h2>
<h3> . . . </h3>
<h4> . . . </h4>
<h5> . . . </h5>
<h6>最小的标题</h6>
 
<p>这是一个段落。</p>
<br> （换行）
<hr> （水平线）
```

```html
<!-- 文本 -->
<b>粗体文本</b>
<code>计算机代码</code>
<em>强调文本</em>
<i>斜体文本</i>
<kbd>键盘输入</kbd> 
<pre>预格式化文本</pre>
<small>更小的文本</small>
<strong>重要的文本</strong>
 
<abbr> （缩写）
<address> （联系信息）
<bdo> （文字方向）
<blockquote> （从另一个源引用的部分）
<cite> （工作的名称）
<del> （删除的文本）
<ins> （插入的文本）
<sub> （下标文本）
<sup> （上标文本）
```

```html
<!-- 链接 -->
普通的链接：<a href="http://www.example.com/">链接文本</a>
图像链接： <a href="http://www.example.com/"><img src="URL" alt="替换文本"></a>
邮件链接： <a href="mailto:webmaster@example.com">发送e-mail</a>
书签：
<a id="tips">提示部分</a>
<a href="#tips">跳到提示部分</a>
```

```html
<!-- 图片 -->
<img loading="lazy" src="URL" alt="替换文本" height="42" width="42">
```

```html
<!-- 列表 表格-->
<ul>
    <li>项目</li>
    <li>项目</li>
</ul>

<ol>
    <li>第一项</li>
    <li>第二项</li>
</ol>

<dl>
  <dt>项目 1</dt>
    <dd>描述项目 1</dd>
  <dt>项目 2</dt>
    <dd>描述项目 2</dd>
</dl>

<table border="1">
  <tr>
    <th>表格标题</th>
    <th>表格标题</th>
  </tr>
  <tr>
    <td>表格数据</td>
    <td>表格数据</td>
  </tr>
</table>
```

```html
<!-- 框架 表单 -->
<iframe src="demo_iframe.htm"></iframe>

<form action="demo_form.php" method="post/get">
<input type="text" name="email" size="40" maxlength="50">
<input type="password">
<input type="checkbox" checked="checked">
<input type="radio" checked="checked">
<input type="submit" value="Send">
<input type="reset">
<input type="hidden">
<select>
<option>苹果</option>
<option selected="selected">香蕉</option>
<option>樱桃</option>
</select>
<textarea name="comment" rows="60" cols="20"></textarea>

</form>
```

```html
<!-- 实体 -->
&lt; 等同于 <
&gt; 等同于 >
&#169; 等同于 ©
&nbsp; 空格
```

```html
<!-- h5元素 -->
<canvas>
<audio>
<video>
<source>
<embed>
<track>
<datalist>
<keygen>
<output>

<article>：独立的内容
<aside>：侧边栏
<bdi>
<command>：命令按钮
<details>
<dialog>：对话框
<summary>
<figure>：独立的图表、图像等
<figcaption>
<footer>：section或document的页脚
<header>：文档头部
<mark>
<meter>
<nav>:导航链接
<progress>：进度
<ruby>
<rt>
<rp>
<section>:文档中的节
<time>:定义日期时间
<wbr>
```

## 2.规范

- div和p，尽量⽤p，因为p在默认情况下有上下间距，对兼容特殊终端有利。
- 需要强调的⽂本，可以使⽤ strong 或 em 标签。
- 使⽤表格时，标题⽤ caption，表头⽤ thead，主体部分⽤ tbody 包围，尾部⽤ tfoot 包围。表头
和⼀般单元格要区分开，表头⽤ th，单元格⽤ td。
- 表单域要⽤ fieldset 标签包起来，并⽤ legend 标签说明表单的⽤途（可理解为表单标题）。

```html
<form>
<fieldset>
<legend>Personalia:</legend> <!-- legend 可理解为表单标题 -->
Name: <input type="text"><br>
Email: <input type="text"><br>
Date of birth: <input type="text">
</fieldset>
</form>
```

- 每个input标签对应的说明⽂本都需要使⽤label标签，并且通过为input设置id属性，在lable标签中
设置for来让说明⽂本和相对应的input关联起来。或者直接在label中内嵌控件。

```html
<label for="username">请输⼊⽤户名: </label>
<input type="text" id="username" name="username">
<!-- OR -->
<label>请输⼊⽤户名<input type="text" id="username" name="usern
ame"></label>
```

- 嵌套规范：
  - 内联元素不能包含块元素
  - 块元素不能放在\<p\>内
  - h1~h6,dt不能包含块元素
- title, keywords, description精简全面
- a标签设置title;外部链接设置rel; rel="nofollow"
- 所有标题使用h1
- br只用于文本换行
- img设置alt
- meta

```html
<!-- 编码 -->
<meta charset="UTF-8" />
<!-- 关键词 -->
<meta name="keywords" content="" />
<!-- 描述 -->
<meta name="description" content=" " />
<!-- 视口-移动设备 -->
<meta name="viewport" content="width=device-width, initial-scale=
1">
<!-- 优先使用IE新版本和Chrome -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 刷新和重定向 -->
<meta http-equiv="refresh" content="0;url=" />
<!-- 禁⽌浏览器从本地计算机的缓存中访问⻚⾯内容 -->
<meta http-equiv="Pragma" content="no-cache">
<!-- 避免转码 -->
<meta http-equiv="Cache-Control" content="no-siteapp" />
<!-- 启用WebApp全屏模式 -->
<meta name="apple-mobile-web-app-capable" content="yes" />
```

## 3.html5骨架

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
       <meta name="Author" content="">
    <meta name="Keywords" content="Margin" />
    <meta name="Description" content="Description Words." />
    <title>Document</title>
</head>
<body>

</body>
</html>

```
