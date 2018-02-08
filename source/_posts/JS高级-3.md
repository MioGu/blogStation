---
title: JS高级(3)
date: 2017-06-24 20:34:36
tags:
categories:
- javascript
---

## 原型链
对象都有原型，原型也是对象，原型也有原型，这样就形成了原型组成链条结构，称为原型链。

### Object.prototype
Object.prototype是js中所有对象的原型链的最终一环，null标志着原型链的结束！
Object.prototype中的所有的成员可以被js中所有的对象共享！

<!--more-->
### 属性搜索原则

当使用对象访问一个属性的时候
1. 现在对象本身进行查找，如果有，就直接使用
2. 如果没有，就去对象的原型中进行查找，如果有，就直接使用
3. 如果没有，就继续沿着原型链向上查找，直到找到为止，或者找到null为止

## Object.prototype的成员

|名称|说明|
|--|--|
|hasOwnProperty| 判断对象本身是否拥有某个属性, `对象.hasOwnProperty(属性名)`|
|isPrototypeOf| 判断当前对象是不是另外一个对象的原型 `对象.isPrototypeOf(对象)`|
|propertyIsEnumerable| 判断属性是否属于对象本身并且可以被for-in遍历 `对象.propertyIsEnumerable(属性名)`|
|toString toLocaleString| 将对象转换成字符串的，toLocaleString会将对象转成本地格式的字符串|
|valueOf| 获取对象的值，一般用在值类型和引用类型运算的时候| 

## 函数的创建方式
### 函数声明
```js
function 函数名(){}
```
### 函数表达式
```js
var 函数名 =  function(){};
```
### Function
```js
var 函数名 = new Function();
```

## Function
### 不传参
创建出来的是一个空函数，没有任何的功能
```js
var func = new Function();
```
### 传一个参数
```js
var func = new Function(methodBody);
//methodBody是一个字符串，里面是符合js语法规范的js代码！会被作为函数体创建新的函数！
```

### 传多个参数
```js
var func = new Function(arg1, arg2...argN, methodBody);
//arg1...argN 都是形参的名称， 是字符串！
//methodBody是一个字符串，里面是符合js语法规范的js代码！会被作为函数体创建新的函数！
```

## eval()
eval可以将字符串转换成代码并直接执行， Funciton创建出来的是函数，需要调用才能执行！

### 解析JSON格式的字符串
```js
//如果直接将json格式的字符串传递给eval使用，eval会把{}当做代码块处理，报错！

var jsonStr = '{"name": "吴彦祖", age: 18}';

//解决办法：
//1. 把变量的声明也作为字符串传递给eval
eval("var obj = " + jsonStr);
//2. 在json格式的字符串前后拼接()
var o = eval("(" + jsonStr + ")");
```
Function和eval都有安全性问题！一般情况下不会使用这两个东西！

## 函数的原型链
函数也是一个对象，函数的构造函数是Function，所以可以分析出函数的原型链

函数 ----> Function.prototype ---> Object.prototype ---> null

## 完整的原型链
将对象的原型链和函数的原型链结合在一起就绘制成完整的原型链

### 绘制步骤
1. 绘制对象的原型链
	1.1. 绘制对象的构造函数
	1.2. 绘制对象的构造函数的原型
	1.3. 绘制对象本身
	1.4. 绘制3者的三角关系
	1.5. 继续沿着构造函数的原型链向上绘制，直到null为止

2. 将构造函数当做对象，绘制函数的原型链
	2.1. 绘制函数的构造函数Function
	2.2. 绘制Function.prototype
	2.3. 绘制3角关系
	2.4. 继续沿着Function.prototype的原型向上绘制，直到null为止

3. 绘制Object构造函数
	3.1 找到Object构造函数和Object.prototype之间的关系绘制
	3.2 将Object当做对象，找到和其构造函数Function的关系，以及原型的关系

4. 将Function当做对象，绘制其原型链

## instanceof 关键字
```js
对象A instanceof 构造函数B
//判断 构造函数B.prototype是不是在 对象A 的原型链上

//Object ---> Function.prototype ---> Object.prototype ---> null
console.log(Object instanceof Function);
console.log(Object instanceof Object);

//Function ---> Function.prototype ---> Object.prototype ---> null
console.log(Function instanceof Object);
console.log(Function instanceof Function);

```

## 函数的成员
1. name : 函数的名称
2. length: 函数的形参的个数
3. caller： 函数是在哪个函数中被调用的，caller就指向那个外部函数（这个caller必须要在函数中去使用，才能访问到值！）
4. arguments： 函数在被调用的时候，会把所有的实参存到这个对象里面（这个对象必须要在函数中去使用，才能访问到值！ 这个arguments是函数的属性，和之前学过的arguments不一样！）

## 静态成员和实例成员
* 静态成员： 通过构造函数访问的成员就是静态成员
* 实例成员： 通过实例对象访问的成员就是实例成员