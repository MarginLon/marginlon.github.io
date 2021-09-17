---
title: "JavaScript Summary"
date: 2020-12-09T10:02:39+08:00
draft: false
---
- [1. Abstract](#1-abstract)
- [2. Basic Item](#2-basic-item)
  - [2.1. {数据类型及转换}](#21-数据类型及转换)
    - [2.1.1. {数据类型}](#211-数据类型)
      - [2.1.1.1. 原始值](#2111-原始值)
      - [2.1.1.2. 对象类型](#2112-对象类型)
    - [2.1.2. {类型检测&转换}](#212-类型检测转换)
      - [2.1.2.1. typeof/instanceof/.constructor/toString.call/Array.isArray([value])](#2121-typeofinstanceofconstructortostringcallarrayisarrayvalue)
      - [2.1.2.2. 其他=>Number](#2122-其他number)
      - [2.1.2.3. 其他=>String](#2123-其他string)
      - [2.1.2.4. 其他=>布尔](#2124-其他布尔)
      - [2.1.2.5. ==的规则](#2125-的规则)
      - [2.1.2.6. 对象=>字符串/数字](#2126-对象字符串数字)
      - [2.1.2.7. 其他](#2127-其他)
  - [2.2. {堆栈内存及执行代码的步骤}](#22-堆栈内存及执行代码的步骤)
  - [2.3. {函数的底层执行机制}](#23-函数的底层执行机制)
  - [2.4. {闭包机制}](#24-闭包机制)
    - [2.4.1. 闭包应用之循环事件绑定的N种解决办法](#241-闭包应用之循环事件绑定的n种解决办法)
  - [2.5. {浏览器垃圾回收处理}](#25-浏览器垃圾回收处理)
  - [2.6. {let&const&var}](#26-letconstvar)
  - [2.7. {this}](#27-this)
    - [2.7.1. 事件绑定](#271-事件绑定)
    - [2.7.2. 函数执行 [普通/成员访问/匿名函数/回调函数]](#272-函数执行-普通成员访问匿名函数回调函数)
    - [2.7.3. 构造函数](#273-构造函数)
    - [2.7.4. 箭头函数 [generator]](#274-箭头函数-generator)
    - [2.7.5. call/apply/bind强制修改this指向](#275-callapplybind强制修改this指向)
  - [2.8. {变量提升&块级作用域}](#28-变量提升块级作用域)
  - [2.9. {JS高阶技巧}](#29-js高阶技巧)
    - [2.9.1. 单例设计模式](#291-单例设计模式)
    - [2.9.2. 惰性函数](#292-惰性函数)
    - [2.9.3. 柯里化](#293-柯里化)
    - [2.9.4. compose](#294-compose)
  - [2.10. {防抖&节流}](#210-防抖节流)
  - [2.11. {面向对象编程}](#211-面向对象编程)
  - [2.12. {原型&原型链}](#212-原型原型链)
  - [2.13. {DOM}](#213-dom)
  - [2.14. {浏览器模型}](#214-浏览器模型)
- [3. Others](#3-others)
  - [3.1. {&& ||}](#31--)
---
# 1. Abstract

---

# 2. Basic Item

## 2.1. {数据类型及转换}
<br/>

### 2.1.1. {数据类型}
<br/>

#### 2.1.1.1. 原始值
<br/>

- Undefined
- Null
- Boolean
- Number
- String
- Symbol 
  - 1.对象的唯一属性
    - ```js
      let key = Symbol();
      let obj = {
        [key]:100
      };
      console.log(obj[key]);

      let arr = Object.getOwnPropertySymbols(obj);
      arr.forEach(item=>{
        console.log(obj[item]);
      });
      ```
  - 2.宏观管理标识：标志唯一性(vuex/redux)
  - 3.底层原理
    - Symbol.hasInstance
    - Symbol.iterator
    - Symbol.toPrimitive
    - Symbol.toStringTag
    - ...
- Bigint
<br/>

#### 2.1.1.2. 对象类型
<br/>

  - 标准普通对象 object
  - 标准特殊对象 Array/RegExp/Date/Error/Math/ArrayBuffer/DataView/Set/Map...
  - 非标准特殊对象 Number/String/Boolean/Symbol/BigInt/... 基于构造函数[Object]创造出来的原始值对象类型
    - ```js
      new Symbol() // TypeError
      new BigInt() // TypeError: Not a constructor
      Object(Symbol()) // pass 
      ```
  - 可调用对象 [实现了call方法] function
    - 普通
    - 构造[内置，自定义]
    - 生成器
    - 箭头
    - 基于ES6快速为对象属性赋值函数的模式（QF）

<br/>

### 2.1.2. {类型检测&转换}
<br/>

#### 2.1.2.1. typeof/instanceof/.constructor/toString.call/Array.isArray([value])
<br/>

- typeof 运算符
  - 返回字符串
  - null -> 'object'
  - 实现CALL的对象[函数、箭头函数、生成器函数、构造函数] -> 'function'
  - 未实现CALL的对象 -> 'object'
  - 未声明的变量 -> 'undefined'

- [example] instanceof [class]  实例是否属于类
- [example].constructor===[class] 获取构造函数
- Object.prototype.toString.call([val])

<br/>

#### 2.1.2.2. 其他=>Number
<br/> 

- Number([val])
  - 隐式转换的调用方法（如```isNaN()```）
  - 出现非有效数字字符即为```NaN```，空串为0
  - Symbol->报错
- parseInt/parseFloat([val],[radix])
  - [val] 必须是一个字符串，否则隐式转换
  - [radix] 不设置或为0，按照10处理，若字符串'0x'开头，按照16处理
  - 处理过程：在[val]中找到所有符合[radix]进制的内容，直到遇到不符合的字符，把找到的内容按照[radix]进制转换为10进制
  - [radix]范围：2~36,包括0,否则```NaN```
- 隐式转换
  - inNaN()
  - 数学运算(特殊："+"的字符串拼接)
  - "=="
<br/>

#### 2.1.2.3. 其他=>String
<br/>
- 规则：
  - 原始值引号包含
- toString() [排除Object.prototype.toString]
    - 普通对象{} => 'object Object'
- String([val])
- 隐式转换
  - "+" : 一边出现字符串，则拼接
  - 基于alert/confirm/prompt/document.write等方式输出
<br/>

#### 2.1.2.4. 其他=>布尔
<br/>

- 规则:
  - 只有"0, NaN, Null, Undefined, 空字符串"为False
- !
- !!
- Boolean([val])
- A||B  A&&B
- 隐式转换
  - 循环或条件判断的结果



<br/>

#### 2.1.2.5. ==的规则
<br/>

- 类型相同
  - ```{}=={}``` :false 比较堆内存地址
  - ```[]==[]``` :false 
  - ```NaN==NaN``` :false ```Object.is(NaN, NaN)``` :true
- 类型不同
  - ```null==undefined``` :true ```===``` :false null/undefined与其他类型不相等
  - 字符串==对象 对象=>字符串
  - 两边类型不一致，先转换为数字再比较
<br/>

#### 2.1.2.6. 对象=>字符串/数字
<br/>

1. 检测```Symbol.toPrimitive```，获得原始值
2. 第一步没有，```valueOf```获取原始值
3. 第二步没有，调用```toString```，字符串
4. 第三步后，若有需要，基于```Number()```，数字
<br/>

#### 2.1.2.7. 其他
<br/>

- "+"
  - {} + 0 左边{}视为代码块 // +0 => 0
  - ({} + 0)  // "[object Object]0"
  - 0 + {} // "0[object Object]"
- 模板字符串实现的是字符串拼接，对象转换为字符串，其余数学运算，对象=>数字
<br/>

----

## 2.2. {堆栈内存及执行代码的步骤}

- 浏览器运行JS代码，提供了栈内存(Stack)以运行代码
  - 栈内存是计算机内存分配的
  - 执行环境栈 EC stack (execution context stack)
    - EC(G) 全局执行上下文，全局代码执行
      - VO(G) 全局变量对象[存储全局上下文中声明的变量]
      - GO(global object) 全局对象 [window]
    - EC(X) 某函数的执行上下文 
  - 等号赋值
    1. 创建值  
    原始值类型的值，储存在栈内，对象数据类型单独开辟堆(Heap)内存
    2. 声明变量  
    把声明的变量存储到当前上下文的变量对象中，如EC(G)  
    3. 赋值  
    指针的指向
  - 堆内存
    - 赋予一个16进制地址，对象中的键值依次存储，内存地址放在栈中供变量引用。
----
## 2.3. {函数的底层执行机制}
![函数底层机制](https://github.com/MarginLon/MarginPostImage/blob/master/%E5%87%BD%E6%95%B0%E5%BA%95%E5%B1%82%E6%9C%BA%E5%88%B6.png?raw=true)

- 正常的函数创建 ```function fn(y){...}```
- 匿名函数之函数表达式 ```var fn = function(y){...};```
- 创建步骤：
    1. 创建一个函数(值/对象)
       1. 创建一个堆内存
       2. 声明函数作用域scope(即哪个上下文)
       3. 存储代码(字符串)
       4. 16进制地址存入栈中以供引用
    2. 声明一个变量fn (函数名也是变量名)
    3. 关联
- 执行步骤
    1. 形成一个全新的私有context =>  EC(FN1)
    2. AO(FN1)&emsp;进栈 （AO:ActiveObject 变量对象）
    3. 初始化作用域链[Scope-chain]
        - <当前自己的私有context，函数的作用域> => 链的右侧是当前context的“上级”context
    4. 初始化this
    5. 初始化arguments
    6. 形参赋值：当前上下文声明一个形参变量，并传递实参值，非严格模式建立映射机制：集合中的每一项和对应的形参变量绑定（只在代码执行之前）。
    7. 变量提升
    8. 代码执行
        - 变量：私有只操作自己，和外界无关；如果不是自己私有，则基于chain向context查找，是否为上级私有，如果不是，继续向上查找，直到EC(G)，即<span style="color:red">作用域链查找机制</span>
    9. 出栈 or 不出栈
---
## 2.4. {闭包机制}
![其他](https://github.com/MarginLon/MarginPostImage/blob/master/%E9%87%8A%E6%94%BE%E6%9C%BA%E5%88%B6.png?raw=true)
- EC(FN)：
    * 开辟的某个堆内存，被当前EC(FN)外的变量占用，此时当前context即EC(FN)不能被出栈释放
    * 闭包：(大函数执行返回小函数只是其中一种形式)
        - 函数执行产生一个私有context，保护内部私有变量不受污染。[保护]
        - 有可能形成不被释放的context，私有变量和一些值被保存，可供下级context中调取使用。[保存]
        - 闭包特点
          * 保护：保护私有context的私有变量和外界互不影响
          * 保存：context的私有变量和值保存起来
          * 弊端：栈内存太大，影响性能，需合理利用
### 2.4.1. 闭包应用之循环事件绑定的N种解决办法
```js
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) {
    // i=0 buttons[0] 第一个按钮
    // i=1 buttons[1] 第二个按钮
    // ....
    // 每一轮循环 i 变量的值 和 需要获取对应某个按钮的索引是一样的  buttons[i]
    buttons[i].onclick = function(){
       console.log('获取当前按钮索引:${i}');
       //不能实现
    };
  }

  //方案1.1：基于“闭包”
  // [每一轮循环都产生一个闭包，存储对应索引；
  // 点击事件触发，执行对应函数，让其上级context闭包]
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) {
      // 每一轮循环形成一个闭包，存储私有变量i的值
      //    + 自执行函数执行，产生EC(A) 私有形参i=0/1/2
      //    + EC（A）中有个小函数,让全局buttons中的某一项占用创建的函数
    (function(i) {
       buttons[i].onclick = function(){
        console.log('获取当前按钮索引:${i}');
       };
    })(i);
  }

  //方案1.2：
   var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) i{
       buttons[i].onclick = function(i){
        return function () {
            console.log('获取当前按钮索引:${i}');
            };
    })(i);
  }
  //方案1.2注解
  var obj = { //返回值赋值给fn
      fn: (function(){
          console.log('大函数');
          return function(){
              console.log('小函数');
          }
      })()
  };
  obj.fn();//执行返回的小函数

  //方案1.3 基于let也是闭包
  let buttons = document.querySelectorAll('button');
  for (let i = 0; i < buttons.length; i++) i{
       buttons[i].onclick = function(){
            console.log('获取当前按钮索引:${i}');
            };
  }

  //方案2 自定义属性
  var buttons = document.querySelectorAll('button');
  for (var i = 0; i < buttons.length; i++) i{
      // 每一轮循环给当前button设置自定义属性存索引
       buttons[i].myIndex = i;
       buttons[i].onclick = function(){
           // this -> 当前点击的按钮
            console.log('获取当前按钮索引:${this.myIndex}');
       };
  }

  //方案3 事件委托 html <button index='i'></button>;
  // 无论点击body中的谁，都会触发body的点击事件
  // ev.target事件源：具体点击的是谁
  document.body.onclick = function(ev) {
      var target = ev.target
          targetTag = target.tagName;
      if(targetTag==="BUTTON"){
          var index = target.getAttribute('index');
          console.log('获取当前按钮索引:${this.myIndex}');
      }
  };
```

---
## 2.5. {浏览器垃圾回收处理}
- GC：浏览器的垃圾回收机制（内存释放）
    * 栈内存：
        + 加载页面，形成全局context，只有页面关闭后，全局context释放
        + 函数执行私有context，进栈执行；函数中代码执行完成，大部分情况，出栈释放。
          + 特殊：如果当前context的某些内容（一般是一个堆），被context以外内容占用，则不能出栈释放。
    * 堆内存
        + （谷歌）：引用标记
            - 浏览器在空闲或指定时间，查看所有堆内存，把没有被任何东西占用的堆内存释放。
        + （IE低版本）：引用计数
            - 创建了堆内存，被占用一次，则浏览器计数+1，取消占用计数-1，计数=0释放内存； => 某些情况导致计数混乱，出现“内存泄漏”
    * 释放内存：
        + x=null => 手动取消占用
---
## 2.6. {let&const&var}

- 传统声明变量：var function
- ES6：let const import()
- let VS const
    - let 变量储值可改 
    - const 赋值不能再与其他赋值关联
- let VS var
    - var 存在变量提升 let 不存在变量提升
        + 变量提升：代码执行前“var/function”提前声明或定义
            - var 只声明
            - function 声明+定义
            - EC(G):if 无论条件是否成立，都变量提升，新浏览器function只声明不定义。
    - 全局context var相当于给GO增一个属性，一改随改；let和GO没有关系。
    - let不允许同context重复声明（不管先前何种方式声明，都不能let声明,词法解析阶段），var无所谓。
    - var
        + var：方法内局部，方法外全局
        + 不加var：全局
        + ![var](https://github.com/MarginLon/MarginPostImage/blob/master/var%E5%8F%98%E9%87%8F.png?raw=true)
    - 暂时性死区（基于typeof检测一个未被声明的变量，不会报错，结果是'undefined'）
        - 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
        + ```js
          console.log(typeof n);//undefined
          let n = 12;//使上一句报错 
          ```
    - let产生块级私有context
        + context & 作用域
            - 全局context
            - 函数执行的私有context
            - 块级作用域（私有context）
                - 对象 function的大括号 判断，循环和代码块的大括号
                ```js
                    debugger;//开启断点调试,控制台基于F10/F11 逐过程/逐语句 控制执行
                    //代码块不会对n产生限制，n使全局context
                    {
                        var n = 12;
                        console.log(n);

                        let m = 13;
                    }
                    console.log(n);
                    console.log(m);// m is not defined


                    let i = 0;//不产生块级context
                    for (; i< 5;i++)
                    {
                        console.log(i);
                    }
                    console.log(i);//5
                ```
---

## 2.7. {this}
- 全局context: this -> window
- 块级context: this -> 继承上级context(包括箭头函数)
### 2.7.1. 事件绑定
- DOM0: ```xxx.onxxx = function(){}```
- DOM2:   
    - ```xxx.addEventListener('xxx',function(){})```  
    - ```xxx.attachEvent('onxxx',function(){})```
- 给当前元素的某个事件行为绑定方法，当事件触发、方法执行，方法中的this是当前元素本身
- 特殊性：attachEvent -> window
```js
  document.body.onclick = function () {
    console.log(this);//body
};
```

### 2.7.2. 函数执行 [普通/成员访问/匿名函数/回调函数]
- 正常的普通函数执行：函数执行前是否有点，没有点，this即window（严格模式下undefined）；有点，谁点this谁
- 匿名函数（自执行函数/回调函数）如果没有特殊处理，则this一般都是window/undefined
  - 函数表达式：等同于普通函数或事件绑定
  - 自执行函数
  - 回调函数：一般是window/undefined，如果另外函数执行中特殊处理，特殊为主。
  - 括号表达式：小括号中包含"多项"，取最后一项，但this受到影响（一般是window/undefined）
```js
function fn(){
  console.log(this);
}
let obj = {
  name: 'Margin',
  fn
  // fn: fn
};

fn(); //this->window
obj.fn();// this -> obj
(obj.fn)(); //this -> obj
(10, obj.fn)(); // this -> window/undefined

// 函数表达式
var fn = function () {
  console.log(this);
};
fn();

// 自执行函数
(function(x){
  console.log(this); // this -> window/undefined
  })(10);

// 回调函数：把一个函数A作为实参，传递给另外一个执行的函数B [B执行中把A执行]
function fn(callback) {
  //callback -> 匿名函数
  //callback(); -> window
  callback.call(obj);  // this -> obj
}
fn(function(){
  console.log(this);
});

// 
let arr = [10,20,30];
arr.forEach(function(item,index){
    console.log(this); 
    // obj 触发回调函数执行的时候,
    // forEach内部把回调函数的this改为传递的第二个参数值obj
},obj);

//
setTimeout(function(){
    console.log(this); //window
},1000);
setTimeout(function(x){
    console.log(this, x); //window 10
},1000, 10);
```



### 2.7.3. 构造函数
- 初始this，把this指向当前的实例对象。
### 2.7.4. 箭头函数 [generator]
### 2.7.5. call/apply/bind强制修改this指向

---


## 2.8. {变量提升&块级作用域}
- 变量提升：在当前context，JS代码自上而下执行之前，浏览器提前处理（词法解析的一个环节）
  - var/function 提前声明/定义
  - 基于var/function 全局context 声明的变量映射到GO(window)一份，并随动。
  - if 无论条件是否成立，都变量提升（条件中带function在新浏览器只提前声明，不提前赋值）
  - ```js
    var func = function AAA() {
      // 函数表达式匿名函数“具名化”，不能外部访问
      // 函数内部私有context会把名字作为context的变量
      // AAA(); 递归调用 而非arguments.callee
    }
    ```
- 块级作用域
  - [原文章](https://juejin.im/post/6844903955814694919)
  - 块级作用域内默认变量
    - 不带var，let，const，只有执行过定义的变量的代码后才可以访问，给window赋值属性，之前会报错
    - 块内默认变量依旧是全局变量
    - 没执行之前不可以访问
  - 块级作用域函数声明
    - 块内函数声明会提升到块顶部，也会在全局作用域用var声明一个同名的undefined变量
    - 块外的全局同名变量的赋值时机是执行完块内函数声明语句
    - 块内的函数声明每次执行的时候都会给全局那个同名的变量赋值一次，并且，只有执行那个定义函数声明的代码才会触发赋值，你写的函数声明就相当于setter,每执行一次就给外部的那个同名的变量赋值一次
    - 如果块内同时有同名的函数声明和默认的变量声明，那给默认的变量赋值时其实相当于赋值给那个同名的函数，因为查找块内的作用域链时找到了,就不会往全局声明了。 
---

## 2.9. {JS高阶技巧}
- 模块化编程
  - 单例
  - AMD require.js
  - CMD sea.js [CommonJS]
  - CommonJS node.js
  - ES6Module
### 2.9.1. 单例设计模式
  - 对象：描述同一个事物的属性和方法，可防止全局污染
    - Object实例
    - “命名空间”
    - window.xxx = xxx
    - 闭包+单例 [早期模块化]
### 2.9.2. 惰性函数
   - 函数重构[闭包]
   - ```js
            function get_css(element,attr){
                if ('getComputedStyle' in window) {
                    get_css = function (element, attr) {
                        return window.getComputedStyle(element)[attr];
                    };
                }
                else {
                     get_css = function (element, attr) {
                        return element.currentStyle[attr];
                };
               }
               return get_css(element, attr);
            }
            ```
### 2.9.3. 柯里化
   - （预先处理）形成一个闭包，存储信息，供下级context调用
 ```js
let res = fn(1,2)(3);
console.log(res);// =>6 1+2+3

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

// ES6 ToDo

//数组 reduce
//     + arr.reduce([function])
let arr = [10, 20, 30, 40];
let result = arr.reduce(function (result, item, index){
        console.log(result,item,index);
        return result + item;
});
// 重构 reduce
function reduce(arr, callback, initValue) {
    let result = initValue,
        i = 0;
    if(typeof result=="undefined"){
        result = arr[0];
        i = 1;
    }

}
let arr = [10, 20, 30, 40];
let result = reduce(arr, function(result, item, index){
    return result + item;
});
console.log(result);

```
### 2.9.4. compose
   - f(g(h(x))) => compose(f,g,h)(x)
```js 
const compose = (...funcs) => {
     return x => {
        return funcs.reduceRight((result, item) =>{
            //result->x item->add1
            //result->add1(x) item->add1
            return item(result);
        }, x);
     };
 };

```
---
## 2.10. {防抖&节流}
- 自己规定频繁触发的条件
- 防抖：只识别一次  --点击事件
- 节流：降低触发频率，能识别‘多次’ --键盘 滚动条
```js
/*
debounce:函数防抖
  @params
    func [funcion,required] : 最后要执行的函数
    wait [number] 触发的频率时间
    immediate [boolean] 设置是否开始边界触发
  @return
    func执行的返回结果
*/
function debounce (func, wait, immediate) {
  if(typeof func!=="function") throw new TypeError('func must be an function');
  if(typeof wait ==="boolean") {
    immediate = wait;
    wait = 300;
  }
  if(typeof wait !=="number") wait =300;
  if(typeof immediate !=="boolean") immediate = false;

  var timer = null;
  return function proxy(...params){ 
    //基于逻辑处理 让fn只执行一次
    var runNow = !timer && immediate; 

    if(timer) {clearTimeout(timer);}
    timer = setTimeout(function(){
      if(timer) {
        clearTimeout(timer);
        timer = null;
      };
      !immediate ?func(...params): null;
    }, wait);
    runNow?func(...params): null;
  };
}
function fn(){
  console.log('OK');
}
box.onclick = debounce(fn,300,true);


/*
throttle:函数节流
  @params
    func [funcion,required] : 最后要执行的函数
    wait [number] 触发的频率时间
  @return
    func执行的返回结果
*/
function throttle(func, wait) {
  if(typeof func!=="function") throw new TypeError('func must be an function');
  if(typeof wait !=="number") wait =300;

  var timer =null,
      previous =0,
      result;
  
  return function proxy(){ 
    var now = +new Date(),
        remaining = wait - (now - previous),
        self = this,
        params = [].slice.call(arguments);
    if(remaining<=0){
      if(timer) {
          clearTimeout(timer);
          timer = null;
        }
      //立即执行
      result = func.apply(self, params);
      previous = +new Date();
    } else if(!timer) {
      // 没有达到间隔时间，之前没有设置过定时器，此时设置定时器，等待remaining后执行一次
      timer =setTimeout(function(){
        if(timer) {
          clearTimeout(timer);
          timer = null;
        }
        result = func.apply(self, params);
        previous = +new Date();
      },remaining)
    }
    return result;
  };
}
function fn(){
  console.log('OK');
}
box.onclick = throttle(fn,500);
```
---
## 2.11. {面向对象编程}

   + 对象，类，实例：
        - 对象：泛指
        - 类：细分“对象”，大类和小类
            - 封装，继承，多态
                - 封装：功能的代码封装，低耦合高内聚
                - 继承：子承父
                - 多态：函数重载（名字相同，传参的个数或类型不同，JS中不存在），重写（子写父的新方法）
        - 实例：某个类中具体事物
            + 独有特征
            + 共有特征
   + JS
        - 内置类
        - 自定义类（类都是“函数数据类型”）
            + 函数执行的时候基于new执行即可“构造函数执行”
                - 构造函数 VS 普通函数
                    1. 创建context后，浏览器<span style="color:red">默认创建一个对象实例</span>，把当前Fn函数当作类（”构造函数“）
                    2. 初始this，把this指向当前的实例对象。
                    3. 执行完，return时
                        - 没有return/return 基本数据类型值，浏览器默认把创建的实例对象返回
                        - return 引用类型，返回自己的返回值。
            + Fn VS Fn()：
                * Fn代表函数本身（堆内存），Fn()函数执行，获取返回值。
             + new Fn VS new Fn()：
                * 执行，第一个不传递实参，第二个传递实参。 
                * 优先级 19 VS 20
            + JS创建值（实例）两种方案:
              + 字面量
              + 构造函数new 
              + 对于对象和函数类型，无区别；<span style="color:red">对于原始值，字面量返回原始值类型，构造函数方式返回对象类型，但都是所属类的实例。</span> 
              + Symbol和BigInt 不允许new ```Object()```
            + 检测一个属性是否为当前对象的成员
                - 属性名 in 对象：无论公私有，有就是true
                - 对象.hasOwnProperty(属性名)：只有私有true
            + 检测公有属性
                ```js
                function hasPubProperty(obj,attr){
                  //弊端：既私有又公有
                  return (attr in obj) && !obj.hasOwnProperty(attr);
                }
                ```
            + 验证实例属于类 instanceof 
              + ```1 instanceof Number // => false``` 
            + 基于"for..in"循环遍历
                + 优先遍历数字属性
                + 不会遍历到Symbol
                + 会把自己扩展到“类原型”上的公共属性方法也遍历 [可枚举的] ```if(!obj.hasOwnProperty(key)) break;//遍历到公共属性，停止遍历```
                + Object.keys(obj):非Symbol私有属性 [数组]
                + Object.getOwnPropertyNames(obj): 同上
                + Object.getOwnPropertySymbols(obj):Symbol私有属性 [数组]
                ```js
                let keys = [...Object.keys(obj),...Object.getOwnPropertySymbols(obj)];
                keys.forEach(key=> {
                            console.log(`属性名:$(),属性值:$()`);.
                });
                ```
             
---
## 2.12. {原型&原型链}
- 函数数据类型内置prototype属性，属性值是一个对象(除Function.prototype是函数)，对象中存储属性和方法是供当前类所属实例调用的公共属性和方法
    * 箭头函数&QF函数没有prototype
    * 原型对象上有一个内置的属性 constructor（构造器），属性值是当前函数本身
- 对象数据类型都具备：\_\_proto\_\_，属性值是当前实例所属类的原型prototype
    * Object.protptype的\_\_proto\_\_值是null，Object是所有对象的“基类” 
- 原型链查找机制
    * f1.say
      * 先找私有属性
      * 没有，基于\_\_proto\_\_找所属类原型的公共属性方法
      * 没有，基于原型对象的\_\_proto\_\_查找，直到Object.prototype
    * f1.\_\_proto\_\_.say
      * 跳过私有的，找所属类原型的公有属性方法
      * Fn.prototype.say   

---
## 2.13. {DOM}
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
  + 类型 
    + Node
    + NodeList(ByTagName/ByClassName/querySelectorAll)   
  + nodeType:元素1/属性2/文本3/注释8/文档9
  + nodeName:
  + nodeValue:
  + childNodes:获得所有子节点
  + children:获得所有元素子节点
  + firstChild/lastChild:
  + firstElementChild/lastElementChild:
  + previousSibling/nextSibing:
  + previousElementSibling/nextElementSibling:
- 增删改
  - createElement
  - createTextNode
  - appendChild
  - insertBefore
  - cloneNode: true|false
  - removeChild
  - setAttribute
   
---
## 2.14. {浏览器模型}

---
# 3. Others

## 3.1. {&& ||}
    - A && B
        * 如果第一个操作数是对象，则返回第二个数
        * 如果第二个操作数是对象，则只有在第一个操作数的求值结果为true的情况下才会返回该对象。
        * 如果两个操作数都是对象，则返回第二个数操作数。 
        * 如果有一个操作数是null，则返回null。 
        * 如果有一个操作数是NaN，则返回NaN。
        * 如果第一个操作数是undefined，则返回undefined。 
        * 对于逻辑与，如果第一个操作数是false，无论第二个操作数是什么，结果都不可能再是true。
    - A || B
        * 如果第一个操作数是对象，则返第一个操作数
        * 如果第一个操作数的求值结果为false，则返回第二个操作数
        * 如果两个操作数都是对象，则返回第一个操作数
        * 如果两个操作数是null，则返回null
        * 如果两个操作数是NaN，则返回NaN
        * 如果两个操作数是undefined，则返回undefined 