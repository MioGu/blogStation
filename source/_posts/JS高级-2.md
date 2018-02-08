---
title: JS高级(2)
date: 2017-06-24 20:23:02
tags:
categories:
- javascript
---

## 面向对象编程举例
1. 初步实现（面向过程的方式）
2. 函数封装 
3. 对象封装
## 创建对象的方式
1. 字面量
```js
var obj = {
	key: value,
	key1: value1
};

//$().css({})
//$.ajax({})
//复用性差
```

<!--more-->
2. 内置构造函数
```js
var obj = new Object();
obj.key = value;
obj.key1 = value1;
//复用性差
```
3. 自定义构造函数
```js
function Person(){
	this.key = value;
	this.key1 = value1;
}

var p = new Person();
```

## 构造函数
### 什么是构造函数
	构造函数就是一个函数，他和普通函数的区别在于，他一般被用来初始化对象！！

### 构造函数的特点
1. 首字母大写
2. 一般和new关键字一起使用
3. 不需要写return语句，默认返回new创建出来的对象

### 构造函数的执行过程
1. 使用new关键字创建对象
2. 调用构造函数，将构造函数中的this指向new创建出来的对象
3. 执行构造函数中的代码，通过this为创建的对象新增属性和方法
4. 默认的返回了new创建出来的对象

### 构造函数的注意事项
1. 如果手动给构造函数写了return语句
	* 如果return的是值类型的数据，对默认返回值不会有任何影响
	* 如果return的是引用类型的数据，则返回的是该引用类型的数据

2. 如果把构造函数当做普通函数来调用
	* this会指向window
	* 返回值会和普通函数一样

### 构造函数写法存在的问题
```js
function Person(){
	this.name = "";
	//每次创建对象都会重新执行一次函数声明，也就是创建一个新的函数
	//每个对象中都会有这么一个方法，但是每个方法的功能都是一样的，所以造成资源浪费
	this.sayHello = function(){

	};
}
//解决方案：
// 将方法的声明提出来放到构造函数外面，每次给对象的方法赋值的时候，直接将外面声明好的函数直接赋值给对象的方法，这样，所有的对象就都共享同一个方法了

function sayHello(){}

funciton Person(){
	this.name = "";
	this.sayHello = sayHello;
}

//这么解决会造成全局变量污染以及代码结构混乱的问题
```

## 原型
### 原型的概念
构造函数在创建出来的时候，系统会默认的帮构造函数创建并且关联一个空对象，这个对象就是原型

### 原型的作用
在原型中的所有的属性和方法，可以被和其关联的构造函数创建出来的所有的对象共享！

### 原型的访问方式
1. 构造函数.prototype
2. 对象.__proto__ (有兼容性问题)

### 原型的使用方式
1. 利用对象的动态特性，给系统创建好的默认的原型中新增属性和方法
```js
Person.prototype.name = "";
```
2. 直接给构造函数.prototype属性赋值一个新的对象！

### 原型的使用注意事项
1. 一般将需要共享的内容放在原型当中，对象特有的东西放在对象本身中
2. 使用对象.属性获取对象的属性的时候，会先在对象本身进行查找，如果有就使用，如果没有，就会去原型中进行查找
3. 使用对象.属性 = 值，给对象的属性赋值的时候，会直接在直接在对象本身进行查找，如果有，就修改，如果没有，就新增这个属性
4. 在使用构造函数.prototype=新的对象的时候， 赋值之前创建的对象和赋值之后创建的对象的原型不一样

## 面向对象的三大特性
* 封装：将功能的具体实现封装在对象内部，只对外界暴露指定的接口，外界在使用的时候，只需要关心接口如何使用，而不需要关心对象内部功能的具体实现，这就是封装 （ATM，电脑）
* 继承：自己没有的东西，别人有，拿过来使用，就是继承（js中的继承是基于对象的！）
* 多态：js中没有多态！

## 继承的实现方式
1. 混入式继承（mix-in）
```js
var obj = {};
var obj1 = {
	name: "",
	age: 18
}

for(var k in obj1){
	obj[k] = obj1[k];
}
```
2. 原型继承
	* 直接将要继承的对象，赋值给构造函数的prototype属性，这样创建出来的所有的对象能够访问的原型就是这个要继承的对象，也就是说实现了继承！
```js
function Person(){
	
}
var obj = {
	name: "",
	age: 18
}

Person.prototype = obj;
```
	* 利用混入的方式，将要继承的对象中的所有的方法和属性，添加到构造函数默认的原型中去
```js
function Person(){
	
}
var obj = {
	name: "",
	age: 18
}
for(var k in obj){
	Person.prototype[k] = obj[k];
}
```
3. 经典继承
```js
//Object.create
var obj1 = {
	name: "",
	age: 18
}
var obj = Object.create(obj1);
//创建出来一个新的对象obj，obj的原型就是obj1


function myCreate(obj){
	if(Object.create){
		return Object.create(obj);
	}else{
		function F(){}
		F.prototype = obj;
		return new F();
	}
}
```