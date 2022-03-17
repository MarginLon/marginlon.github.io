---
title: "JavaScript AJAX"
date: 2021-09-23T20:00:00+08:00
draft: false
---

- [1. AJAX](#1-ajax)

# 1. AJAX
- AJAX解决网页异步刷新
    - 同步刷新
    - 异步刷新
        1. 浏览器可以从服务器同时请求多项内容；
        2. 浏览器请求返回的速度会快得多；
        3. 只有页面中真正改变的部分得到更新；
        4. 能够减少服务器数据流量；
        5. 用户可以在页面更新的同时继续工作；
        6. 有些改变无须与服务器往返通信就可以处理。
```js
// AJAX :基于XMLHttpRequest创建HTTP请求
//      + 创建xml实例
//      + open 打开一个url地址 [ 发送请求前的配置]
//          + method 请求方式：GET(get/delete/head/options...)/POST(post/put/patch...)
    /*
            GET和POST
            -   GET请求传递给服务器的信息，除了请求头传递外，要求基于URL问号传参传递给服务器
                xhr.open('GET','./1.json?lx=1&name=xxx');
            -   POST则基于请求主体传递
                xhr.send('lx=1&name=xxx')
            1)GET传递的信息不如POST多，因为URL有长度限制，超出会被截掉，POST理论上无限制，但传递过多会传输超时...可以手动限制
            2)GET会产生缓存(浏览器默认产生，不可控):两次及以上，请求相同的API接口，并且传递参数一样，浏览器可能会把第一次请求的信息直接返回，而非从服务器获取最新信息。请求URL的末尾设置随机数，以此清除GET缓存
                xhr.open('GET','./1.json?lx=1&name=xxx'+Math.random());
            3)POST相对安全: GET传递的信息基于URL末尾拼接，劫持或修改.
    */
//          + url 请求的URL地址
//          + async 是否异步(默认true)
//          + username userpass ...
//      + onreadystatechange 监听请求过程，不同阶段不同处理
    /*    - AJAX状态 
            - xhr.readyState  
            1. UNSENT 未发送,默认  [0]
            2. OPENED 已打开 执行了OPEN [1]
            3. HEADERS_RECEIVED: 服务器已经返回响应头的信息 [2]
                - 响应头：Date 返回时的服务器时间 先返回后主体慢慢返回 
                - 响应主体：客户端需要的信息
            4. LOADING 主体正在加载返回 [3]
            5. DONE 主体信息返回 [4]
          - HTTP网络状态码 
            - xhr.status
            - 以2开始 200 服务正常返回数据（数据是否为想要的不一定 业务流程：code标识表示业务逻辑上的成功失败）
            - 以3开始 304 读取的是协商缓存的数据 301永久重定向(一般用于域名的转移) 302/307 临时转移/重定向（一般用于服务器的负载均衡）
            - 以4开始 400 请求参数有误 401 无权访问 404 地址错误 403 服务器拒绝执行 [一般都是客户端问题]
            - 以5开头 500 服务器发生未知错误 503 超负荷 [一般是服务器问题]
          - 获取响应主体信息 xhr.response/responseText/responseXML...
            JSON字符串 XML格式 文件流[binary]
          - 获取响应头信息 xhr.getResponseHeader/getAllResponseHeaders
    */
//      + send 发送请求，[设置请求的主体信息]
//          基于请求主体传递给服务器的数据格式有要求 [postman：接口测试工具]
//          1.form-data 文件上传
//          xhr.setRequestHeader('Content-Type','multipart/form-data');
//          --- 
//          fd.append('lx',0);
//          fd.append('name','xxx');
//          xhr.send(fd);
//          2.x-www-form-url-encoded格式字符串 
//          格式；"lx=1&name=xxx" [常用]
//          $ npm i qs
            /*
            xhr.setRequestHeader('Content-Type','application/x-www-form-url-encoded');
            ---
            xhr.send(Qs.stringify({
            lx:0,
            name:xxx
             }));
            */
//          3.raw 字符串
//          普通字符串 => text/plain
//          JSON => application/json    => JSON.stringify/parse [常用]
//          XML格式字符串 => application/xml
//          4.binary
//          buffer/二进制
//          xhr.setRequestHeader('Content-Type','image/jpeg');
//          5.GraphQL


let xhr = new XMLHttpRequest;
xhr.open('GET','./1.json');
xhr.setRequestHeader('Content-Type','multipart/form-data');
xhr.onreadystatechange = function () {
    if(xhr.readyState === 4 && xhr.status === 200){
        console.log(xhr.rensponseText);
    }
};
xhr.send(Qs.stringify({
    lx:0,
    name:xxx
}));
// xhr.abort() 中断，onabort
// xhr.timeout 设置超时时间 ontimeout
// xhr.withCredentials CORS跨域允许携带cookies等
// xhr.upload.onprogress 监听文件上传进度
// xhr 7-function
```