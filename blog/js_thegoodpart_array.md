<!--
author: zhangxuefeng
date: 2016-09-06
title: javascript语言精粹-数组
tags: javascript
category:javascript
status: publish
summary: javascript语言精粹读书笔记
-->
### 数组
> javascript 提供一种拥有一些类数组(array-like)特性的对象。它把数组下标转变成字符串，用其作为索引。


```
var numbers = ['zero','one','two','three','four','five','six'];
var numbers_obj = {0:'zero',1:'one',2:'two',3:'three',4:'four',5:'five',6:'six'};
```
两者产生的结果相似。numbsers继承自Array.prototype，而numbers_obj继承自Object.prototype，所以numbers继承了大量有用的方法，同时还有一个length属性。

- 长度  

每个数组都有一个length属性，它的值是这个数组的最大整数属性名加上1。它不一定等于这个数组里的属性的个数。


设置更大的length不会给数组分配更多的空间，而把length设小将导致所有下标大于等于新length的属性被删除。

```
// 新增一个元素到数组的尾部
numbsers[numbsers.length] = 'seven';
or
numbsers.push('eight');
```


- 删除

由于 javascript的数组其实就是对象，所以delete运算符可以用来从数组中移除元素，但是这样会在数组中留下一个空洞。

slice方法可以删除一些元素并将他们替换为其他元素


- 数组与对象

>当属性名是小而连续的整数时用数组，否则使用对象。

```

var is_array = function(value){
    return value && typeof value === 'object' && value.constructor = Array;
}

var is_array = function(value){
    return Object.prototype.toString.apply(value) === '[object Array]';
}
```
- 方法

javascript提供一套数组可用的方法。这些方法是存储在Array.prototype中的函数，Array.prototype同样也是可以扩充的
```
Array.method('reduce',function(f,value){
    var i;
    for(i = 0;i<this.length;i += 1){
        value = f(this[i],value);
    }
    return value;
});

var data = [1,2,3,4];

var add = function(a,b){
    return a+b;
}

var mult = function(a,b){
    return a*b;
}

var sum = data.reduce(add,0);

var product = data.reduce(mult,1);


//因为数组其实就是对象，可以给一个单数的数组添加方法

data.total = function(){
    return this.reduce(add,0);
}

var total = data.total();
```

因为字符串'total'不是整数，所以给数组增加一个total属性【并不会】改变它的length属性。当属性名是整数时，数组才是最有用的，但它依旧是对象，并且可以接受任何字符串作为属性名。

Object.create方法在数组中是没有意义的，因为它产生一个对象，而不是一个数组。产生的对象将继承这个数组的值和方法，但是没有那个特殊的length属性。


- 预设值

javascript数组通常不会设置预设值。如果用[]定义一个新的数组，它将会是空的。如果访问一个不存在的元素，将会是undefined。
```
Array.dim = function(dimension,initial){
    var a = [],i;
    for(i = 0;i < dimension; i+=1){
        a[i] = initial;
    }
    return a;
};

var myArray = Array.dim(10,0);
```