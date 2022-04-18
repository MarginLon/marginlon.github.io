---
title: "Webpack"
description:
toc: true
authors:
tags:
- Webpack
categories:
- Webpack
series:
- Webpack
date: '2020-10-01T22:02:39+08:00'
lastmode: "2020-10-01T22:02:39+08:00"
featuredImage:
featuredVideo:
draft: false
---
- [1. Webpack](#1-webpack)

## 1. Webpack

```js
 // npx webpack => npm run build   
    // package.json
 "scripts": {
     "build": "webpack"
 } 

// 
 module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```

- 面试
  1. Loader
     - css-loader: 加载CSS，支持模块化，压缩，文件导入等
     - style-loader: 把CSS注入JS中，通过DOM操作加载CSS
     - postcss-loader:扩展CSS语法
     - babel-loader: ES6=>ES5
     - file-loader: 文件输出，处理图片和字体
     - url-loader:base64url编码 -> 去掉尾部的"=" , "+" 替换成 "-"
     - vue-loader: 加载Vue.js单文件组件
  2. Plugin
     - html-webpack-plugin:设置渲染页面的模板
     - clean-webpack-plugin:目录清理
     - define-plugin:定义环境变量
     - DllPlugin:减少构建时间，进行分离打包
  3. Loader和Plugin
     - Loader本质是function，返回转换的结果，Webpack只认识JavaScript，Loader负责其他类型的资源的转译
       - Loader在module.rules里配置，类型为数组，每一项都是Object，内部包含test（文件类型），loader，options等属性
     - Plugin是插件，基于事件流框架Tapable，扩展Webpack的功能，在Webpack运行的生命周期会广播许多事件，Plugin可以监听这些事件，合适的时机通过Webpack提供的Api改变结果。
       - plugins中单独配置，类型为数组，每一项都是Plugin的实例，参数通过构造函数传入。
  4. Webpack构建流程
       1. 初始化参数：从配置文件和Shell读取合并参数
       2. 开始编译：初始化Compiler对象，加载所有配置的插件，执行对象的run方法编译
       3. 确定入口：从entry找到所有入口文件
       4. 编译模块：从入口文件出发，调用所有配置的Loader对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件完成本步
       5. 完成模块编译：得到每个模块的最终内容和依赖关系
       6. 输出资源：组装成包含多个模块的Chunk，把每个Chunk转为单独的文件=>输出列表
       7. 输出完成：确定内容后，根据出口配置输出的路径和文件名，把文件写入文件系统
  5. Webpack热更新
     - Webpack的模块热更新(HMR)，功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。主要是通过以下几种方式，来显著加快开发速度：(1)保留在完全重新加载页面时丢失的应用程序状态。(2)只更新变更内容，以节省宝贵的开发时间。(3)调整样式更加快速 - 几乎相当于在浏览器调试器中更改样式。
     - 维护一个Websocket，本地资源变化，客户端会进行资源对比，发送Ajax请求获取更新内容，再借助这些信息继续发送jsonp请求获取增量更新，后续部分交给HotModulePlugin更新
  6. 优化构建速度
       1. 使用最新的webpack版本
       2. Loaders应用于最少的模块
       3. 使用 DllPlugin 将更改不频繁的代码进行单独编译。这将改善引用程序的编译速度，即使它增加了构建过程的复杂性。
       4. 使用 cache-loader 启用持久化缓存。使用 package.json 中的 "postinstall" 清除缓存目录。
       5. 使用 更少/更小 的库。在多页面应用程序中使用 CommonsChunksPlugin。在多页面应用程序中以 async 模式使用 CommonsChunksPlugin 。移除不使用的代码。只编译你当前正在开发部分的代码。
       6. 多进程/多实例：thread-loader 可以将非常消耗资源的 loaders 转存到 worker pool 中。
       7. 基础包分离：使用html-webpack-externals-plugin将基础包通过CDN引入，不打入bundle中使用SplitChunksPlugin进行分离，替代CommonsChunkPlugin。
  7. 手写loader/plugins
