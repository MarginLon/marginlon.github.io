---
title: "JavaScript HTTP网络层 跨域方案"
date: 2021-09-23T20:00:00+08:00
draft: false
---

- [1. HTTP网络层](#1-http网络层)
- [2. 跨域方案](#2-跨域方案)
 
# 1. HTTP网络层
- http vs https
  - 超文本传输协议(s: ssl加密) 端口号:80 443
  - ftp:文件上传下载协议 端口号: 21
- 页面 [第一次访问]
  1. URL解析
     - 地址解析 
       - 协议://登录信息@域名:端口号/请求资源的文件路径?查询字符串#片段标识符(HASH)
       - 问号参数
         - 客户端 -> 服务器 GET请求
         - A页面 -> B页面
       - HASH
         - 锚点定位
         - HASH路由
     - 编码  
     ```js
     encodeURI
     decodeURI
     encodeURIComponent
     decodeURLComponent
     escape unescape
     ```  
  2. URL解析
  3. 缓存检查 [第二次访问]
     - Memory Cache; Disk Cache
        + 打开网页：查找 disk cache 中是否有匹配，如有则使用，如没有则发送网络请求
        + 普通刷新 (F5)：因TAB没关闭，因此memory cache是可用的，会被优先使用，其次才是disk cache
        + 强制刷新 (Ctrl + F5)：浏览器不使用缓存，因此发送的请求头部均带有 Cache-control: no-cache，服务器直接返回 200 和最新内容 
     - 资源文件 
       * 强缓存 ：先看强缓存，强制缓存，有缓存则用缓存[html页面不能强缓存 资源文件设置时间戳/webpack生成新文件]
         * Expires: 缓存过期时间
         * Cache-Control:（优先）max-age = 2592000
       * 协商缓存: 304 
         * Last-Modified / ETag
         * 先向服务器发请求,传递Etag等值，一样：没更新，返回304，从缓存拿文件；不一样：更新，返回200，从服务器拿文件并加入缓存
     - 数据信息 api
       * 自己处理[cookie,localStorage,sessinoStorage,vue/redux...]  
  5. DNS解析 找到ip [20-120ms] 
     * 递归查询
       *  客户端 -> 浏览器缓存 -> 本地hosts -> 本地DNS解析器缓存 -> 本地DNS服务器 -> 客户端(逐级探测)
     * 迭代查询
       *  客户端 -> 本地DNS服务器 -> 根域名 -> 本地DNS服务器 -> 顶级域名 -> 本地DNS服务器 -> ... -> 客户端
     * 减少DNS请求次数/ dns-prefetch  
  6. TCP三次握手 找到服务器并建立通信通道
     - TCP作为一种可靠传输控制协议，其核心思想：既要保证数据可靠传输，又要提高传输的效率！ 
  7. 通信
     - HTTP报文：请求报文/响应报文
     - 状态码: 
           - 200：OK
           - 202 Accepted: 服务器接受请求，未处理（异步） 
           - 204 No Content： 服务器已经处理了请求，不需要返回实体内容
           - 206 Partial Content:成功处理了GET请求
           - 301 Moved Permanently 永久转移 [域名迁移]
           - 302 Move Temporarily 临时转移 [负载均衡]
           - 304 Not Modified 读取的是协商缓存的数据
           - 305 Use Proxy
           - 400 Bad Request : 请求参数有误
           - 401 Unauthorized：权限（Authorization）
           - 403 Forbidden
           - 404 Not Found
           - 405 Method Not Allowed
           - 408 Request Timeout
           - 500 Internal Server Error
           - 503 Service Unavailable
           - 505 HTTP Version Not Supported
  8. TCP四次挥手
     - 服务器端收到客户端的SYN连接请求报文后，可以直接发送SYN+ACK报文
     - 但关闭连接时，当服务器端收到FIN报文时，很可能并不会立即关闭链接，所以只能先回复一个ACK报文，告诉客户端：”你发的FIN报文我收到了”，只有等到服务器端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送，故需要四步握手。
     - Connection: keep-alive
  9.  页面渲染
- HTTP1.0 1.1 2.0
  - 1.0 vs 1.1
    - 缓存标识
    - 连接带宽优化：1.1断点续传 ： 206
    - 错误通知： 1.1新增24个错误状态码 409 410...
    - Host头处理;
    - 1.1 长连接 Connection: keep-alive
  - 2.0 vs 1.X
    - 新的二进制格式：
    - header压缩：编码压缩
    - 服务端推送：服务器会多推送文件
  - 多路复用
    - HTTP/1.0  每次请求响应，建立一个TCP连接，用完关闭
    - HTTP/1.1 「长连接」 若干个请求排队串行化单线程处理，后面的请求等待前面请求的返回才能获得执行机会，一旦有某请求超时等，后续请求只能被阻塞，毫无办法，也就是人们常说的线头阻塞；
    - HTTP/2.0 「多路复用」多个请求可同时在一个连接上并行执行，某个请求任务耗时严重，不会影响到其它连接的正常执行；
```js
/*
 *     1.利用缓存
 *       + 对于静态资源文件实现强缓存和协商缓存（扩展：文件有更新，如何保证及时刷新？）  
 *       + 对于不经常更新的接口数据采用本地存储做数据缓存（扩展：cookie / localStorage / vuex|redux 区别？）
 *     2.DNS优化
 *       + 分服务器部署，增加HTTP并发性（导致DNS解析变慢）
 *       + DNS Prefetch
 *     3.TCP的三次握手和四次挥手
 *       + Connection:keep-alive
 *     4.数据传输
 *       + 减少数据传输的大小
 *         + 内容或者数据压缩（webpack等）
 *         + 服务器端一定要开启GZIP压缩（一般能压缩60%左右）
 *         + 大批量数据分批次请求（例如：下拉刷新或者分页，保证首次加载请求数据少）
 *       + 减少HTTP请求的次数
 *         + 资源文件合并处理
 *         + 字体图标
 *         + 雪碧图 CSS-Sprit
 *         + 图片的BASE64
 *       + ......
 *     5.CDN服务器“地域分布式”
 *     6.采用HTTP2.0
 * ==============
 * 网络优化是前端性能优化的中的重点内容，因为大部分的消耗都发生在网络层，尤其是第一次页面加载，如何减少等待时间很重要“减少白屏的效果和时间”
 *     + LOADDING 人性化体验
 *     + 骨架屏：客户端骨屏 + 服务器骨架屏
 *     + 图片延迟加载
 *     + ....
 */
```

# 2. 跨域方案

- Web页面地址 和 请求接口地址 协议 域名 端口号三者有一个不一样就是跨域，默认不能请求
- 解决方案
  - 修改本地hosts:开发跨域，部署同源
  - JSONP
    - script标签的src请求资源文件[基于GET]，不存在跨域机制
    - 需要服务器端支持
    - axios、fetch...默认基于promise
  - CORS
    - 服务器端设置允许源，允许当前客户端发请求，避开浏览器的安全策略
    aixos: withCredentails: 
  - Proxy
    - 利用后端和后端通信默认是没有安全策略的
    - 中间代理服务器：预览页面 从其他服务器获取数据
      - 开发环境
        - node.js 自己写
        - vue/react => webpack-dev-server
      - 生产环境
        - nginx 反向代理
        - ...
  - 扩展：其他跨域方案「配合iframe」
    - postMessage
    - window.name
    - document.domin
    - location.hash