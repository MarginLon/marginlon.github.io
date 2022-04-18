---
title: "Node.js"
description:
toc: true
authors:
tags:
- Node.js
categories:
- Node.js
series:
- Node.js
date: '2020-10-01T22:02:39+08:00'
lastmode: "2020-10-01T22:02:39+08:00"
featuredImage:
featuredVideo:
draft: false
---
- [1. 模块化规范](#1-模块化规范)
- [2. Node 内置包](#2-node-内置包)
- [3. 登录流程](#3-登录流程)

## 1. 模块化规范

- CommonJS
  - require exports module __dirname__filename
  - 导入：= require('')
  - 导出：module.exports =
- ES6Module
  - ```<script type='module'></script>```
  - 导入： import {} from '' /import xxx from ''
  - 导出： export {let var const function class}/export default

---

## 2. Node 内置包

- 全局变量 : global process
- commander.js
  - npm i commander
- path
- url
- fs
  - 文件系统:
- http
- Buffer
  - 缓冲区，暂时存放输入输出数据的一段内存
- 流
- egg

## 3. 登录流程

- Web Storage
  - cookie
    1. 大小：4kb
    2. 带宽：访问消耗
    3. 安全风险
  - Web Storage
    1. 存储大小：5~10MB
    2. 不发送至服务器
    3. 更丰富的接口
    4. localStorage,sessionStorage
  - localStorage
    - 持久化的本地存储，不手动删除不会过期
    - 存储：localStorage.setItem(key,value);
    - 读取：localStorage.getItem(key);
    - 删除：localStorage.removeItem(key); / localStorage.clear();
- cookie
- session
  - 客户端 => 用户名 密码 => 服务器
  - 服务器 => session_id => Cookie => 客户端
  - 客户端 => Cookie.session_id => 服务器
  - 扩展性差，多台关联服务器需要session共享 : 1.session持久化，写入数据库或持久层，持久层不能挂. 2.服务器不存，保存在客户端
- token
  - json web token : 跨域身份验证，cookie里自动发送不能跨域，放在HTTP请求头的Authorization或跨域的时候，JWT 放在 POST 请求的数据体里面。
    - '.'分割 三部分：Header.Payload.Signnature
    - Header：{"alg": "HS256","typ": "JWT"}
    - Payload:

    ```js
    {
    iss (issuer)：签发人,
    exp (expiration time)：过期时间
    sub (subject)：主题
    aud (audience)：受众
    nbf (Not Before)：生效时间
    iat (Issued At)：签发时间
    jti (JWT ID)：编号
    私有字段...
    }
    ```

    - Signature:HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret)
    - （1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

    - （2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

    - （3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

    - （4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

    - （5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

    - （6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。
