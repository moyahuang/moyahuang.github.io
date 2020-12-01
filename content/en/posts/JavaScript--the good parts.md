---
title: "JavaScript--the good parts"
date: 2020-10-29T06:40:51+09:00
description: javascript,the good parts
draft: false
hideToc: false
enableToc: true
enableTocContent: true
tocPosition: inner
tags:
- JavaScript
series:
- 前端
categories:
-
---
## Objects
- what is an object?
- simple types of JS
- why are numbers, strings and booleans object-like?
- how to add a create method to the Object function?

```javascript
if(typeof Object.create !== 'function'){
    Object.create=function(o){
        var F=function(){};
        F.prototype=o;
        return new F();
    }
}
```
-what is an object literal?
## Functions
- what is the difference between a function and an object
- what is a function literal
- what is anonymous function
- what is a closure
- what are the two additional parameters that a function has
- what determines the value of this parameter
- what are the four patterns of invocation
### Method invocation pattern
- Method invocation pattern is a method that is defined as a property of an object, for example

```javascript
var myObject={
    value: 0,
    increment: function(inc){
        this.value+=typeof inc==='number'?inc:1;
    }
};
```
And the methods is invoked when using dot expression or [subscript] expression, like this

```javascript
//invocation method one
myObject.increment();

//invacation method two
myObject['increment'](2);
The parameter this is only bound when the function is invoked , also referred as late binding. Methods can use this to access the object context so that it can retrieve the values from the object or modify the object.

### Function invocation pattern?
When a function is invoked in this way

```javascript
var sum=add(1, 2);
```
this is bound to the global object, which is a design error of the language.

But this is not expected when the function is used as a inner function. You suppose this is pointed to the outer function, but it is not.

```javascript
var value=2;
var myNum={
    value: 1
}
myNum.double=function(){
    var helper=function(){
         this.value=add(this.value, this.value);//this here indicates the global object
    }
    helper();
}
console.log(myNum.value); //4
```
So the situation above is not what we wanted, we can implement a simple workaround technique like this

```javascript
myNum.double=function(){
	var that=this; //use another variable to store the context
    var helper=function(){
         that.value=add(that.value, that.value);//this here indicates the global object
    }
    helper();
}
console.log(myNum.value);//2
```
### Constructor invocation pattern
what is a prototypal inheritance language
When a function is invoked with the new prefix, a new object is created with a hidden link to the value of the function’s prototype member. And this is bound to the new object.

And by convention, such function is capitalized.

```javascript
var Person=function(name){
    this._name=name;
}
Person.prototype.getName=function(){
    return this._name;
}
var mike=new Person("Michael")
console.log(mike.getName());
```
### Apply invocation pattern
An method from an object can be applied in another object when they have the same property names. It can be used in this way: 1. an object’s method is 2. applied to 3. another object

```javascript
var Animal={
    bark:function(){
        console.log(this.sound);
    }
}

var dog={
    sound: "wang"
}

Animal.bark.apply(dog);
```
In this situation, this is applied to the parameter of the apply function.

### Return
what does return do

if no return value is specified, what will be returned

when a function is invoked with new and the return value is not an object, what will be returned?

### Exceptions
how to throw an exception
how to catch an exception
### Augmenting Types
define a method that helps us make augmenting methods
the basic types of JS can be augmented. To make an example

```javascript
//this method can hide the ugliness of the using of 'prototype'
Function.prototype.method=function(name,func){
    if(!this.prototype[name]){
        this.prototype[name]=func';
        return this;
    }
}
```
Once the Function is augmented, we can invoke the method with other basic types. For example, we sometimes need to extract the Integer part of a number.

```javascript
Number.method("integer", function(){
    return Math[this<0'ceil':'floor'](this);//unload
});

console.log((-1.6).integer());//load
```
Or we need to get rid of the white spaces at the start/end of a word

```javascript
String.method("trim",function(){
    return this.replace(/^\s+|\s$/g, '');
})
```
### Memoization
please make a function that helps making memoized functions like fibonacci and factorial
## Array
what would happen if the length of an array object is set to be larger than the actual value?
what if the length is set to be smaller?
how to append new element to an array？
how to correctly delete an element of an array?
how to enumerate the elements of an array in order?
to avoid failures to identify arrays that were constructed in a different window or frame, how to define the function

```javascript
var is_array=function(value){
    return Object.prototype.toString.apply(value);
}
```
please define a function that creates a matrix and another function that creates an identity matrix
how to define a comparison function that sorts an array of numbers?
note: imagine the middle point of the two parameters as the origin, return negative if the left parameter should come first, return positive if otherwise.

please define a method that sort an array of objects by taking a member name
## String
what’s the difference between string.match(regexp) and regexp.match(string) p89
:question:

it is said that for in statement can loop over all of the property names in an object, but by my observation, only the properties owned by the object will be visited.

“if a particular order of properties is required, avoid the for in statement entirely” but it is listed in order

what is the difference between a function and a method?

what values can be produced by typeof