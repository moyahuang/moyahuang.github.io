---
title: "JS的继承是个啥玩意儿"
date: 2019-11-14T14:00:50+08:00
author: "Moya"
hideToc: false
enableToc: true
enableTocContent: false
author: Moya
authorEmoji: 🙀
tags:
- 前端
- JavaScript
draft: false
---
继承可以帮我们实现1. 代码重用 2. 以及进行规约？

JS的对象采用原型继承(prototype-based inheritance)机制。当属性和方法添加到对象的原型上时，该对象及其后代便都会具备这些属性和方法。

在其他诸如C语言等程序语言中，通常都有一个函数叫做构造函数(constructor)。构造函数就是为对象实例赋初值的。JS中没有类（ES6以前），普通函数(function)即可以用作构造函数，不过为了跟普通函数进行区分，一般会把用作构造函数的普通函数名首字母大写。

当定义了这样一个函数（类）时
当定义了这样一个函数（类）时


```javascript
function Crane(a, b){}
```  

JS会为Crane.prototype增加一个属性`constructor`，其值指向刚才定义的方法（对象）或构造函数（以下统称为构造函数）本身，并且该构造函数还包括下面几个属性

```javascript
Crane.prototype={constructor: this} 
```

用一张图可以表示为

[![constructor](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS%5Cconstructor.png)](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS%5Cconstructor.png)

同时，构造函数本身也有`constructor`属性，这个属性指向Function构造函数

[![function_constructor](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS%5Cfunction_constructor.png)](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS%5Cfunction_constructor.png)

注：ES6引入了关键词class，JS也可以像其他语言一样在class内部作用域定义构造函数。

## 构造函数模式如何继承（下面的分类待斟酌，实质性区别是什么？）

### 使用Parent.call

上面提到的构造函数使用关键词`new`即可创建对象，这种模式怎样实现继承呢？我们看到下面的例子
```javascript
function Bird(type, color){  
 this.type=type;  
 this.color=color;  
 this.fly=function(){  
 console.log(this.color+' '+this.type+" is flying!");  
 }  
}  
//Parrot也是鸟 它有所有Bird拥有的属性  
function Parrot(type, color){  
 Bird.call(this, type, color); //继承鸟的所有属性和方法  
 this.talk=function(){  
 console.log(this.color+' '+this.type+" is talking!")  
 }  
}  
  
var prr=new Parrot("鹦鹉"， "彩色de");  
prr.talk();  
prr.fly();  
```
### [](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/#%E4%BD%BF%E7%94%A8ES6%E7%9A%84%E7%B1%BB%E5%85%B3%E9%94%AE%E8%AF%8D "使用ES6的类关键词")使用ES6的类关键词

ES6引入了类机制，使用关键词`extends`即可实现继承。上面的代码可以改成这样
```javascript
class Bird{  
 constructor(type, color){  
 this.type=type;  
 this.color=color;  
 }  
 fly(){  
 console.log(this.color+' '+this.type+" is flying!");  
 }  
}  
  
class Parrot extends Bird{  
 constructor(type, color){  
 super(type, color);  
 }  
 talk(){ ... }  
}  
```
### 伪类模式（不推荐）

我认为伪类模式与上面的继承方法的不同点在于，子类的构造函数会包含所有的属性，而无法不会进行属性的传递。因其关键点在于将子类的原型设置为父类对象。下面看一个例子
```javascript
var Mammal=function(name){  
 this.name=name;  
}  
//注意这里要采用加强prototype的方式添加方法  
Mammal.prototype.get_name=function(){  
 return this.name;  
}  
Mammal.prototype.says=function(){  
 return this.saying||'';  
}  
var Cat=function(name){  
 this.name=name;  
 this.saying='meow';  
}  
Cat.prototype=new Mammal();  
Cat.prototype.get_name=function(){  
 return this.says()+' '+this.name;//调用父类的方法  
}  
var myCat=new Cat("Katy");  
console.log(myCat.get_name()); // "meow Katy"  
```
当然，《JS语法精粹》里对上述的一些步骤进行了方法的封装，使程序表达性更高，隐藏了重复写`prototype`的一些”ugliness”。
```javascript
var Cat=function(name){  
	this.name=name;  
	this.saying="meow";  
	}.inherits(Mammal)  
	.method("get_name",function(){...});  
```
其中`inherits`是这么定义的
```javascript
Function.method("inherits",function(Parent){  
	this.prototype=new Parent();  
	return this;  
})  
```
Function.method也是《JS语法精粹》定义的一个方法，常常用到，这里我写一遍算是复习了
```javascript
Function.prototype.method=function(name, func){  
	if(this.prototype[name]!=="function"){  
	this.prototype[name]=func;  
		return this;  
	}  
}  
```
#### 伪类模式的缺点：

### [](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/#%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F "原型模式")原型模式

原型语言的好处是我们可以从一个原有的对象的基础上创建更多其他类似但又有所不同的对象，从而免除了将一个系统抽象类的过程。

首先我们要做的就是创建一个对象
```javascript
var myMammal={  
	name: "mammal",  
	get_name: function(){  
		return this.name;  
	}  
	says: function(){  
		return this.saying || '';  
	}  
}  
var myCat=Object.create(myMammal);  
myCat.name="Siamese";  
myCat.saying="meow";  
myCat.get_name=function(){  
	return this.says+" "+this.name;  
}  
```
其中Object.create是《JS语言精粹》定义的一个方法，其作用是返回一个以参数对象为原型的新对象，下面来复习一遍
```javascript
if(typeof Object.create !== 'function'){  
	Object.create=function(o){  
		var F=function(){};  
	    F.prototype=o;  
	    return new F();  
	}  
}  
```
上面的这种方式叫做**差异化继承(differential inheritance)**

### 函数模式

上面所述的继承模式的缺点是，对象的所有属性和方法都暴露在外。采用函数模式可以克服上述缺点。

首先我们要创建一个函数，用于生产新的对象。但是因为我们不需要用`new`来调用这个函数，所以该方法首字母小写。创建这个函数可以分为四个步骤：

1.  创建一个新对象(任何方式都可以)
2.  定义一些（私有的）变量和方法（在函数内部定义的变量和方法都是私有的，所以给私有加了括号）
3.  为创建的新对象添加特权方法
4.  返回新对象

这是书上的伪代码模板
```javascript
var constructor=function(spec, my){  
 var that, other private instance variables;  
 my = my || {};  
 that = a new object;  
 Add privileged methods to that  
 return that;  
}  
```
其中参数`spec`包含构造新实例的所有信息，而这个参数最好为本章节前提的`object specifier`，因为这样不用每次传入一个包含所有参数的完整对象。

参数`my`是继承链上所有函数构造器共享的私有资源。（？是不是类似于保护类）

下面继续用这种模式实现我们的示例，下面创建了一个父类对象
```javascript
var mammal=function(spec){  
 var that={};//创建新对象  
 that.get_name=function(){//为新对象添加特权方法  
 return spec.name;  
 }  
 that.says=function(){  
 return spec.saying || '';  
 }  
 return that;//返回新对象  
};  
  
var myMamma=mammal({name: 'mammal'});
```

其子类对象
```javascript
var cat=function(spec){  
 spec.saying=spec.saying||'';  
 var that=mammal(spec);//创建新对象  
 that.get_name=function(){//为新对象添加特权方法  
 return spec.says()+' '+spec.name;  
 }  
 return that;  
}  
```
:question: 书中定义的superior方法我不知道用来干嘛的

#### [](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/#%E7%BB%84%E4%BB%B6%EF%BC%9F "组件？")组件？

## [](https://moyahuang.github.io/passages/JS%E7%9A%84%E7%BB%A7%E6%89%BF%E6%98%AF%E4%B8%AA%E5%95%A5%E7%8E%A9%E6%84%8F%E5%84%BF-Understanding-inheritance-in-JSS/#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99 "参考资料")参考资料

1.  [https://www.freecodecamp.org/news/a-guide-to-prototype-based-class-inheritance-in-javascript-84953db26df0/](https://www.freecodecamp.org/news/a-guide-to-prototype-based-class-inheritance-in-javascript-84953db26df0/)
    
2.  《JS语法精粹》

