<!--
author: zhangxuefeng
date: 2016-09-02
title: javascript语言精粹-对象与函数
tags: javascript
category:javascript
status: publish
summary: javascript语言精粹读书笔记
-->
### 对象
- javascript中，对象是可变的键控集合（keyed collection）。对象是属性的容器，其中每个属性都拥有名字和值。属性的名字可以是包含字符串在内的任意字符。属性值可以是除undefined值外的任何值。
- javascript 包含一个原型链的特性，允许对象继承另外一个对象的属性。
- 一个对象的字面量就是包围在一对花括号中的零或多个【键值对】。
```
var person = {'name':'zxf','age':24};

/*|| 会取第一个为true的,&&会尝试取到最后一个为true的（遇到false停止）*/
//可以用 || 来填充默认值 
var status = person.status || "unknown";
//尝试从undefined的成员属性中取值会导致 TypeError异常，可以通过&&运算符来避免错误
var gril_name = person.girl && person.girl.name;
```
- 对象通过引用传递，**永远不会被复制**  
```
var a = b = c = {};
```
- 反射机制
```
typeof person.name 
//检测对象是独有该属性，不检查原型链
person.hasOwnProperty('name');
```

- delete 可以删除对象属性，它不会触及原型链中的对象（可能使原型链中的同名属性暴露出来）


### 函数

- **函数**包含一组语句，他们是javascript的基础模块单元，用于代码复用，信息隐藏和组合调用。函数用于指定对象的行为，所谓编程，就是将一组需求分解成一组函数与数据结构的技能。
- javascript中函数就是对象。对象是【名/值】对的集合并拥有一个连接到原型对象的隐藏连接。对象的字面常量连接到Object.protoype（也是一个object）。

因为函数时对象，所以他们可以像任何其他值一样被使用。
函数可以在保存变量、对象和数组中。函数可以被当做参数传递给其他函数，函数也可以再返回函数。而且，因为函数时对象，所以函数可以拥有方法。

> 函数的与众不同在于他们可以被调用。

- 调用一个函数会暂停当前函数的执行，传递控制权和参数给新函数。除了申明是定义的形参，每个函数还接受两个附加的参数：this和arguments。

**调用运算符** 是跟在任何一个函数表达式之后的一对圆括号
```
var that = this;
```

- 基本类型扩充 
```
Function.prototype.method = function(name,func){
    if(!this.prototype[name]){
        this.prototype[name] = func;
    } 
    return this
}

Number.method('integer',function(){
    return Math[this < 0 ? 'ceil':'floor'](this); 
});

(-10/3).integer();
```

- 【作用域】 在编程语言中，作用域控制着变量与参数的可见性与生命周期，它减少了命名冲突，提供了自动的内存管理。

javascript 并不支持块级作用域，只有函数作用域。意味着定义在函数内的参数和变量在函数外是不可见的；而在一个函数内部任何位置定义的变量，在该函数内部任何位置都可见。

最好的做法是在函数体的顶部声明函数中可能用到的所有变量。

- 【闭包】 避免在循环中创建函数，可以在循环之外创建一个辅助函数，让这个辅助函数绑定参数，避免引起混淆。
- 【模块】 我们可以使用函数和闭包来构造模块。模块是一个提供接口却隐藏状态与实现的函数或对象。

### 柯里化
 允许我们把参数与传递给它的参数结合，产生一个新的函数

```

function add(){var sum = 0; for(i=0;i<arguments.length;i++){sum += arguments[i];} return sum;}

Function.method('curry',function(){
    var slice = Array.prototype.slice,
        args = slice.apply(arguments),
        that = this;
        console.log(args);//[1]
        console.log(arguments);//[1]
        console.log(this);//function add(){}
        console.log(that);//function add(){}
        return function(){
        console.log('666');
        console.log(arguments);//[6]
        console.log(args);//[1]
        console.log(this);//window
        console.log(that);//function add(){}
            return that.apply(null,args.concat(slice.apply(arguments)));
        }
});


//调用
var add1 = add.curry(1);
document.writeln(add1(6));

```
### 记忆

函数可以将先前操作的结果记录在某个对象里，从而避免重复的运算。

> 定义一个函数，然后返回它。

```
var memoizer = function(memo,formula){
    var recur = function(n){
        var result = memo[n];
        if(typeof result !== 'number'){
            result = formula(recur,n);
            memo[n] = result;
        }
        return result;
    }
    return recur;
}

var fibonacci = memoizer([0,1],function(recur,n){
    return recur(n-1) + recur(n-2);
});


```

### 继承

> 继承提供了两个有用的服务。首先是代码重用的一种形式。另外一个好处是引入了一套类型系统的规范，程序员无需编写显示类型转换的代码。

javascript是一门弱类型语言，从不需要类型转换。对象的集成关系变得无关紧要。对于一个对象来说重要的是**它能做什么**，而不是它从哪里来。


```
Function.method('inherits',function(Parent){
    this.prototype = new Parent();
    return this;
});


//定义构造器
var Mammal = function(name){
    this.name = name;
}
Mammal.prototype.get_name = function(){
    return this.name;
}
Mammal.prototype.says = function(){
    return this.saying || '';
}



var Cat = function(name){
    this.name = name;
    this.saying = 'meow';
}

Cat.prototype = new Mammal('From the Mammal');
Cat.prototype.purr = function(n){
    var i,s = '';
    for(i=0;i<n;i++){
        if(s){
            s += '-';
        }
        s += 'r';
    }
    return s;
};
Cat.prototype.get_name = function(){
    return this.says() + ' '+ this.name +' ' + this.says();
};

var myCat = new Cat('Tom');
myCat.says();
myCat.purr(5);
myCat.get_name();

//会从原型链上找方法
```
### 原型

> 基于原型的继承：一个对象可以继承一个旧对象的属性

```
var myMammal = {
    name:'the Mammal',
    get_name : function(){
        return this.name;
    },
    says:function(){
        return this.saying || '';
    }
}

//差异化继承

var myCat = Object.create(myMammal);
myCat.name = 'Tom';
myCat.saying = 'meow';
myCat.purr = function(n){
    var i,s = '';
    for(i=0;i<n;i++){
        if(s){
            s += '-';
        }
        s += 'r';
    }
    return s;
};
myCat.get_name = function(){
    return this.says() + ' '+ this.name +' ' + this.says();
};

```

### 函数化

继承模式的一个弱点就是没法保护隐私，对象的属性都是可以见的。

应用模块模式

```
//spec对象包含构造器需要构造一个新实例的所有信息
//my对象是一个为继承链中的构造器提供秘密共享的容器。my对象可以选择新的使用，如果没有传入一个my对象，那么会创建一个my对象。
var constructor = function(spec,my){
    var that,其他私有实例变量
    my = my || {};
    //把共享变量和函数添加到my中
    that = 一个新对象
    添加that的特权方法
    return that;
};

```
```
var mammal = function(spec){
    var that = {};
    
    that.get_name = function(){
        return spec.name;  
    };
    
    that.says = function(){
        return spec.saying || '';
    };
    return that;
}

var myMammal = mammal({name:'mammal'});


var cat = function (spec){
    spec.saying = spec.saying || 'meow';
    var that= mammal(spec);
    that.purr = function(n){
        var i,s = '';
        for(i=0;i<n;i++){
            if(s){
                s += '-';
            }
            s += 'r';
        }
        return s;
    };
    that.get_name = function(){
        return that.says() + ' '+ spec.name +' ' + that.says();
    };
    return that;
};

var myCat = cat({name:'Tom'});

```

```
//先给function添加method方法
Function.prototype.method = function(name,func){
    if(!this.prototype[name]){
        this.prototype[name] = func;
    } 
    return this
}

//处理父类方法的方法
Object.method('superior',function(name){
    var that = this,method = that[name];
    return function(){
        return method.apply(that,arguments);  
    };
});


var coolcat = function(spec){
    var that = cat(spec),
    super_get_name = that.superior('get_name');
    //super_get_name = that.get_name;
    that.get_name = function(n){
        return 'like ' + super_get_name() + ' baby'; 
    }
    return that;
}

var coolCat = coolcat({name:'Bix'});
var name = coolCat.get_name();

```

> 如果对象的所有状态都是私有的，那么该对象就成为一个“防伪(temper-proof)对象”。该对象的属性可以被替换或删除，但该对象的完整性不会受到损害。如果我们用韩淑华的样式创建一个个对象，并且该对象的所有方法都不使用this或that，那么该对象就是持久性(durable)的。一个持久性对象就是一个简单功能函数的集合。


### 部件

我们可以从一套部件中把对象组装出来。例如，可以构造一个给任何对象添加事件与处理特性的函数。它会给对象添加一个on方法、一个fire方法和一个私有的事件注册表对象:

```
var eventuality = function(that){
	var registry = {};

	that.on = function(type, method, parameters){
		var handler = {
			method : method,
			parameters : parameters
		};

		if(registry.hasOwnProperty(type)){
			registry[type].push(handler);
		}else{
			registry[type] = [handler];
		}
		return this;
	};

	that.fire = function(event){
		var array,
		    func,
		    handler,
		    i,
		    type = typeof event === "string" ? event : event.type;

		if(registry.hasOwnProperty(type)){
			array = registry[type];
			for(i = 0; i < array.length; i++){
				handler = array[i];
				func = handler.method;
				if(typeof func === "string"){
					func = this[func];
				}
				func.apply(this, handler.parameters || [event]);
			}
		}
		return this;
	};
	
//	that.get_registry = function(){
//		return registry;
//	};s

	return that;
};
```