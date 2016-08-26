<!--
author: zhangxuefeng
date: 2016-08-18
title: js中的apply与call
tags: javascript
category:javascript
status: publish
summary: js中关于函数与对象的关键概念
-->
js 里函数调用有 4 种模式：方法调用、正常函数调用、构造器函数调用、apply/call 调用。

同时，无论哪种函数调用除了你声明时定义的形参外，还会自动添加 2 个形参，分别是 this 和 arguments。

### 方法调用
函数是一个对象的属性

```
var a = {    
    v : 0,    
    f : function(xx) {                
        this.v = xx;    
    }
}
a.f(5);
```
上面函数里的 this 就绑定的是这个对象 a。所以 this.v 可以取到对象 a 的属性 v。

### 正常函数调用

```
function f(xx) {        
    this.x = xx;
}
f(5);
```

 f 里的 this 绑定的是全局对象，如果是在浏览器运行的解释器中，一般来说是 window 对象。所以这里 this.x 访问的其实是 window.x ，当然，如果 window 没有 x 属性，相当于是给 window 对象添加了一个 x 属性，同时赋值。
 
 
###  构造器函数调用

如果你在一个函数前面带上 new 关键字来调用，那么 js 会创建一个 prototype 属性是此函数的一个新对象，同时在调用这个函数的时候，把 this 绑定到这个新对象上。

```
function a(xx) {        
    this.m = xx;
}
var b = new a(5);
```
这里this 绑定的就不再是前面讲到的全局对象了，而是这里说的创建的新对象。

## apply/call 调用

在 js 里，**函数其实也是一个对象**，那么函数自然也可以拥有它自己的方法。在 js 里，每个函数都有一个公共的 prototype —— Function，而这个原型自带有好几个属性和方法，其中就有这里困惑的 bind、call、apply 方法。

先说 apply 方法，它让我们构造一个参数数组传递给函数，同时可以自己来设置 this 的值，这就是它最强大的地方，上面的 3 种函数调用方式，你可以看到，this 都是自动绑定的，没办法由你来设，当你想设的时候，就可以用 apply() 了。apply 函数接收 2 个参数，第一个是传递给这个函数用来绑定 this 的值，第二个是一个参数数组。

```
function a(xx) {        
    this.b = xx;
}
var o = {};
a.apply(o, [5]);
alert(a.b);    // undefined
alert(o.b);    // 5
```
上面 apply() 接收两个参数，第一个是绑定 this 的值，第二个是一个参数数组，注意它是一个数组，你想传递给这个函数的所有参数都放在数组里，然后 apply() 函数会在调用函数时自动帮你把数组展开。而 call() 呢，它的第一个参数也是绑定给 this 的值，但是后面接受的是不定参数，而不再是一个数组，也就是说你可以像平时给函数传参那样把这些参数一个一个传递。所以如果一定要说有什么区别的话，看起来是这样的：
```
function a(xx, yy) {    
    alert(xx, yy);    
    alert(this);    
    alert(arguments);
}
a.apply(null, [5, 55]);
a.call(null, 5, 55);
```
最后再来说 bind() 函数，上面讲的无论是 call() 也好， apply() 也好，都是立马就调用了对应的函数，而 bind() 不会， bind() 会生成一个新的函数，bind() 函数的参数跟 call() 一致，第一个参数也是绑定 this 的值，后面接受传递给函数的不定参数。 bind() 生成的新函数返回后，你想什么时候调就什么时候调，看下代码就明白了
```
var m = {   
    "x" : 1
};
function foo(y) {
    alert(this.x + y);
}
foo.apply(m, [5]);
foo.call(m, 5);
var foo1 = foo.bind(m, 5);
foo1();
```

### 补充：
在 js 里想定义一个函数，于是你会这么写:
```
function jam() {};
```
其实这是 js 里的一种语法糖，它等价于：
```
var jam = function() {};
```
然后你想执行这个函数，脑洞大开的你会这么写：
```
function jam() {}();
```
但是这么写就报错了，其实这种写法也不算错，因为它确实是 js 支持的函数表达式，但是同时 js 又规定以 function 开头的语句被认为是函数语句，而函数语句后面是肯定不会带 () 的，所以才报错，于是聪明的人想出来，加上一对括号就可以了。于是就变成了这样：
```
(function jam() {}());
```
这样就定义了一个函数同时也执行它