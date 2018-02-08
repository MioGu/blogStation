---
title: JS高级(4)
date: 2017-06-26 20:03:19
tags: categories
categories:
- javascript
---

## 伪数组
拥有数组通过数字作为下标访问元素的特征，并且拥有length属性，但是没有数组方法的对象就称作伪数组

## arguments对象
arguments对象是函数中的一个伪数组，在函数被调用的时候，会把所有的实参存到这个伪数组当中

### 属性
* length: 传入的实参的个数
* callee: arguments对象所在的函数，一般被用来实现递归！
<!--more-->
### 如何通过arguments对象实现重载？
重载: 参数不同  实现不同的功能
 ```js
function test(){
    switch(arguments.length){
        case 1: 
            break;
        case 2: 
            break;
        case 3: 
            break;
        case 4: 
            break;
        case 5: 
            break;
    }
}
 ```

## 递归
### 概念
函数中直接或者间接的调用自己

### 两大要素
1. 自己调用自己
2. 结束条件

### 化归思想
化归思想，将一个问题由难化易，由繁化简，由复杂化简单的过程称为化归，它是转化和归结的简称。

### 递归解决数学问题
1. 前n项和
2. n!
3. 求幂
4. 求斐波那契数列的第n项

### 递归获取后代元素
1. 先获取元素的所有的子元素
2. 再去获取子元素的子元素，依次递归就可以获取到所有的后代元素
```js
var list = [];
function getChildren(ele){
	var children = ele.children;
	for(var i = 0; i < children.length; i++){
		var child = children[i];
		list.push(child);
		getChilren(child);
	}
}

function getChildren(ele){
	var list = [];
	var children = ele.children;
	for(var i = 0; i < children.length; i++){
		var child = children[i];
		list.push(child);
		var temp = getChilren(child);
		list = list.concat(temp);
	}
	return list;
}
```

## 作用域
变量起作用的范围

JS里面只有function（函数）可以创建作用域

JS中的作用域是词法作用域（静态作用域）
变量的作用域只和函数的声明位置有关，和函数的调用无关！代码在写出来之后，就可以根据代码的书写结构确定变量的作用域，而不需要关心具体的运行的时候的状况。
```js
var num = 123;
function f1(){
	console.log(num);
}
funciton f2(){
	var num = 456;
	f1();
}
f2();
```

块级作用域： js中没有块级作用域， 块级作用域就是代码块限定的作用域！

### 作用域链
函数可以创建作用域，函数中又可以声明函数，这样就形成了作用域嵌套作用域的链式访问结构，叫做作用域链！

### 变量搜索原则
当使用一个变量的时候
1. 现在当前使用该变量的作用域中进行查找，找该变量的声明，如果有，就直接使用
2. 如果没有就去上一级作用域中进行查找，找该变量的声明，如果有，就直接使用
3. 如果没有，就继续沿着作用域链向上查找，直到找到全局作用域为止

## 变量提升
js代码执行分两个阶段
1. 预解析阶段
2. 执行阶段

在预解析阶段，系统会将所有的变量声明，以及函数声明提升到其所在的作用域的最顶上，这个过程就叫做变量提升。

### 变量提升的特殊情况
1. 函数同名： 都提升，后面的会把前面的覆盖掉
2. 函数和变量同名： 只提升函数声明，忽略掉变量声明
3. 变量提升是分作用域的，变量和函数的声明，只会被提升到其所在的作用域的最顶上
4. 变量提升是分段(script标签)的， 在当前script标签中的声明，只会被提升到当前script标签的最顶上，不会跨标签提升
5. 条件式函数声明（在条件语句中声明的函数）：会被当做函数表达式来处理，只提升函数名，不提升函数体！   （条件式函数声明不推荐使用！）
6. 函数的形参： 在预解析之前，就已经完成了函数的形参的声明以及赋值，所以形参的声明和赋值不参与变量提升！
