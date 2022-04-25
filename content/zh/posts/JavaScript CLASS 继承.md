---
title: "JavaScript CLASS 继承"
description:
toc: true
authors:
tags:
- JavaScript
- 面向对象
categories:
- 面向对象
series:
- JavaScript
date: "2022-04-16T16:28:39+08:00"
lastmode: "2022-04-06T16:28:39+08:00"
featuredImage:
featuredVideo:
draft: false
---
- [1.CLASS](#1class)
- [2.类的继承](#2类的继承)

## 1.CLASS

```js
//ES6
class Modal {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        // this.n = 100;
    } 
    // 给实例的私有属性和方法
    n = 100;

    // 原型上公共方法（公共属性无法直接设置，方法不能使用箭头函数）
    // 增加的方法没有prototype，不可枚举
    getX() { console.log(this.x);}
    getY() { console.log(this.y);}

    //当作普通对象设置“静态”属性和方法
    static n = 200;
    static setNumber(n) {this.n=n;}
}
//原型上的公共属性提取到外面单独写
Modal.prototype.z = 10;
let m = new Modal(10, 20);

//ES5
function Modal(x,y){
    this.x=x;
    this.y=y;
}

Modal.prototype.z=10;
Modal.prototype.getX=function(){
    console.log(this.x);
}
Modal.prototype.getY=function(){
    console.log(this.y);
}
// Model->普通对象: 私有属性和方法与实例无关
Modal.n=200;
Modal.setNumber=function(n){
    this.n=n;
};
let m = new Model(10,20);

```

---

## 2.类的继承

- 封装：低耦合、高内聚
- 多态: 重载[方法名相同，参数类型或个数不同 => 多个方法] & 重写[子类重写父类中的方法]
- 继承: 子类继承父类 [JS:子类的实例既有子类赋予的私有/共有属性和方法，也想拥有父类的私有/公有属性方法]
  1. 原型继承

  ```js
  //基于原型链__proto__
  // + 父要赋予其实例的私有属性，子实例会变为公有属性
  // + 子实例可以修改父的方法，影响父实例。
  Child.prototype = new Parent;
  Child.prototype.get = function get(){};
  ```

  2. call继承

  ```js
  // 父类普通方法执行{原型失效}，让方法中的this是子类的实例，让父类的私有赋予子类实例的私有
  function Child(){
    Parent.call(this);
  }
  Child.prototype = new Parent;
  ```

  3. 寄生组合式继承

   ```js
  function Child(){
    Parent.call(this);
  }

  Child.prototype = Object.create(Parent.prototype);
  ```

  4. ES6 extends继承

  ```js
  class Parent {
    constructor(){
      this.x =100;
    }
    getX(){}
  }
  class Child extends Parent {
    constructor(){
      super();
      this.y = 200;
    }
    getY(){}
  }
  ```
