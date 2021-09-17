---
title: "JavaScript 模块化编程&单例设计模式 柯里化 compose 惰性函数"
date: 2021-09-12T20:00:00+08:00
draft: false
---
- [1. 模块化编程&单例设计模式](#1-模块化编程单例设计模式)
  - [1.1. 单例设计模式](#11-单例设计模式)
- [2. 进阶技巧](#2-进阶技巧)
- [2.1 JQ环境区分](#21-jq环境区分)
- [2.2 柯里化函数](#22-柯里化函数)
- [2.3 compose函数](#23-compose函数)
- [2.4 惰性函数](#24-惰性函数)

# 1. 模块化编程&单例设计模式  
- 模块化编程
  - 高级单例
  - AMD require.js
    - require & define
    - 依赖前置
  - CommonJS node.js 
    - 导入： require('')
    - 导出： module.exports.
    - 随用随导
  - CMD sea.js [CommonJS]
  - ES6Module
    - 导入：import
    - 导出: export & export default  
---
## 1.1. 单例设计模式
```js
/*
- 对象：每一个对象都是Object的单独实例，避免全局变量污染
    - Object实例  “命名空间”
    - 闭包：自执行函数  暴露api: window.xxx = xxx / 
        let Amodule = (fn(){
            return{ };
        })();
*/
```

---
# 2. 进阶技巧
# 2.1 JQ环境区分
  ```js
    ( function( global, factory ) {
    // Node : global->this
    // Webpack : global -> window
    // 浏览器 : global -> window

    "use strict";
	if ( typeof module === "object" && typeof module.exports === "object" ) {
        //支持commonJS:Node & Webpack
        module.exports = global.document ?
        //global -> window 在webpack端、运行，把factory执行的返回结果做模块导出 =>
            // module.exports = jQuery
        factory( global, true ) :
        // node端运行 直接导出一个函数=> JQ不支持Node端
			function( w ) {
				if ( !w.document ) {
					throw new Error( "jQuery requires a window with a document" );
				}
				return factory( w );
			};
    } else {
        //浏览器端运行JQ : 执行factory，传递实参window
        factory(global);
    }
    })
    (typeof window !== "undefined" ? window : this, function factory( window, noGlobal ){
        // webpack : window-->window   noGlobal-->true
        // 浏览器(webview): window-->window   noGlobal-->undefined
        "use strict";

        var jQuery = function( selector, context ) {
		    return new jQuery.fn.init( selector, context );
	    };

        /* 导出模块 */
        if ( typeof define === "function" && define.amd ) {
            define( "jquery", [], function() {
                return jQuery;
            } );
        }
        if ( typeof noGlobal === "undefined" ) {
            //浏览器端运行：向全局对象挂载两个属性 jQuery/$, 值是私有的jQuery函数
            window.jQuery = window.$ = jQuery;
        }
        return jQuery;
    });

    /*支持各环境*/
    (function(){
        let utils = {};

        if ( typeof module === "object" && typeof module.exports === "object" ) {
            module.exports = utils;
        }
        if ( typeof window !== 'undefined') window.utils = utils;
    })
  ``` 
# 2.2 柯里化函数
  - （预先处理）形成一个闭包，存储信息，供下级context调用
  ```js
  let res = fn(1,2)(3);
  console.log(res);// =>6 1+2+3

  //ES6 toDo
  const fn = function fn(...params){
    // params:[1,2]
    return function proxy(...args){
      // args:[3]
      params = params.concat(args);
      return params.reduce(function(result,item){
        return result + item;
      });
    };
  };
  //ToDo
  function fn() {
      let outerArgs = Array.from(arguments); 
      
      return function annoymous(){
          let innerArgs = Array.from(arguments);

          let params = outerArgs.concat(innerArgs);
          return params.reduce(function(result, item){
              return result + item;
          });
      };
  }  

  // eg.
  const curring = function curring(){
      let args = [];
      const add = (...args) => {
        params = params.concat(args);
        //每次执行都返回add，无限次执行
        return add;
      };
      //每次输出add函数或者进行运算，都需要先转化为字符串/数字
      add[Symbol.toPrimitive] = () => {
        //每次add传递的值求和
        return params.reduce((result, item) => result + item);
      };
      return add;
  };

  let add = curring();
  let res = add(1)(2)(3);
  console.log(+res); //->6

  add = curring();
  res = add(1, 2, 3)(4);
  console.log(+res); //->10

  add = curring();
  res = add(1)(2)(3)(4)(5);
  console.log(+res); //->15

  ```  
---

# 2.3 compose函数
```js
// f(g(h(x))) => compose(f,g,h)(x)
const add1 = x => x + 1;
const mul3 = x => x * 3;
const div2 = x => x / 2;

const compose = function compose(...funcs){
    let len = funcs.length;
    if(len===0) return x => x;
    if(len===1) {
      //console.log(funcs[0]);  x => x+1;
      return funcs[0];}
    return function operate(x) {
      return funcs.reduceRight((result,item)=>{
          if(typeof item==="function"){
            return item(result);
          } else{
            return result;
          }
        },x);
    };
};

console.log(compose(div2,mul3,add1,add1)(0));
[div2,mul3,add1,add1].reduceRight((result,item)=>{
  return item(result);
},0);

// redux源码的compose
const compose = function compose(...funcs){
  if(funcs.length===0){
    return (arg) => arg
  }

  if(funcs.length===1){
    return funcs[0]
  }
  // return funcs.reduce((a,b) =>{
  //   return x =>{
  //     return a(b(x));
  //   };
  // });

  // funcs [div2,mul3,add1,add1]
  // a:div2 b:mul3 -> x => div2(mul3(x))
  // a: x => div2(mul3(x)) b:add1 -> x => div2(mul3(add1(x)))
  return funcs.reduce((a,b) => (...args) =>a(b(...args)))
};
```
---
# 2.4 惰性函数
```js
const css = function css(elem, attr){
  if('getComputedStyle' in window){
    return window.getComputedStyle(elem)[attr];
  }
  return elem.currentStyle[attr];
};

//函数重构实现惰性思想
let css = function (elem, attr){
  if('getComputedStyle' in window){
    css = function(elem, attr){
      return window.getComputedStyle(elem)[attr];
    }   
  } else {
    css = function(elem,attr){
      return elem.currentStyle[attr];
    };
  }
  return css(elem, attr);
};

```