---
title: JS高级(5)
date: 2017-06-27 21:20:41
tags:
categories:
- javascript
---


## 闭包
### 概念
闭包指的就是那些能够访问独立的变量的函数！

### 闭包要解决的问题
函数中声明的变量不能被函数外部直接使用

### 闭包的基本模型
```js
function outer(){
	var data = "";
	function inner(){
		//操作data
	}
	return inner;
}
```
<!--more-->
### 使用闭包设置和获取变量的值
```js
function outer(){
	var data = "";
	var obj = {
		setData: function(value){
			data = value;
		},
		getData: function(){
			return data;
		}
	}
	return obj;
}
```
### 闭包的原理
就是作用域的访问规则！

### 闭包的作用
1. 给函数提供一个私有的变量
2. 保护变量，给变量的设置提供专有的渠道，在这个渠道中可以添加一些校验的逻辑

## 缓存
1. 浏览器缓存
2. CDN
3. 硬件缓存

### 缓存封装
```js
function createCache(){
	var cache = {};
	var keys = [];
	return function(key, value){
		if(value){
			cache[key] = value;
			if(keys.push(key) > 20){
				delete[keys.shift()];
			}
		}else{
			return cache[key];
		}
	}
}

```

### jQuery缓存源码
```js
function createCache(){
	var keys = [];
	function cache(key, value){
		if(keys.push(key + " ") > 20){
			delete cache[keys.shift()];
		}
		return (cache[key + " "] = value);
	}
}
```

## 递归实现的斐波那契数列存在的性能问题
由于存在大量的重复的计算，所以导致在40多之后，就算不出来了
```js
function fib(n){
    if(n==1 || n==2){
        return 1;
    }
    return fib(n-1) + fib(n-2);  //每个分支下都会执行到 n==1或者n==2的时候 即使之前已经被计算出结果
}
```

### 解决方案
1. 提供一个缓存，用这个缓存来存储计算出来的数据
2. 每次在计算的时候，首先先从缓存中去获取,如果有，就直接返回
3. 如果没有，就通过递归的方式去计算
4. 计算出来之后，一定要记着保存到缓存中去，以便下次使用

```js
function createCache(){
	var cache = {};
	var keys = [];
	return function(key, value){
		if(value){
			cache[key] = value;
			if(keys.push(key) > 20){
				delete[keys.shift()];
			}
		}else{
			return cache[key];
		}
	}
}

var cache = createCache();

function fib(n){
	var num = cache(n);
	if(!num){
		if(n == 1 || n == 2){
			num = 1;
		}else{
			num = fib(n - 1) + fib(n - 2);
		}
		cache(n, num);
	}
	return num;
}

```
