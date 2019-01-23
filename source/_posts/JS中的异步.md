---
title: JS中的异步
date: 2019-01-22 15:47:02
categories: 异步
tags:
  - JS中的异步
  - Promise
  - async
---

为什么js会有异步呢？

<!-- more -->
![d363e94eeb511d8f031213518fb8be15-c858de0f-192e-4d9e-8ba7-5d0be3a58761](https://img-1256907623.cos.ap-chengdu.myqcloud.com/d363e94eeb511d8f031213518fb8be15-c858de0f-192e-4d9e-8ba7-5d0be3a58761.JPG)

#### 1.JS为何有异步
*__JavaScript是单线程语言__*。所谓的单线程就是程序在执行时，是有顺序的，前面的必须处理好，后面的才会执行。
```javascript
	var i, t = Date.now()
	for (i = 0; i < 100000000; i++) {
	}
	console.log(Date.now() - t)  // 240
```
运行上面代码发现时间，这段程序话费了240ms时间来运行。执行过程中js并不会去执行其他代码。但是如果有一种情况，可能会有大量的网络请求，而一个网络资源啥时候返回，这个时间是不可预估的。那也要花费时间去等待嘛，很显然不合理。

#### 2.异步代码的实现
最常见的网络请求：
```javascript
  axios.get('/user?ID=12345').then(res => {
  	console.log(res)
  })
  .catch(err => {
  	console.log(err)
  });
```
里面的`console.log(res)`、`console.log(err)`并不会马上执行。会在请求成功或者失败后执行。对于这种传递过去不执行，等出来结果之后再执行的函数，叫做callback，即回调函数。

#### 3.开发中常见的异步操作
- 网络请求，如ajax http.get
- 定时函数，如setTimeout setInterval
- ......
#### 4.ES6中的Promise
使用Promise可以完成异步操作
```javascript
    let promise = new Promise((res, rej) => {
        console.log('Promise')
        res()
        rej()
    }).then(() => {
        console.log('resolved!!!')
    }).catch(() => {
        console.log('reject!!!')
    });
    console.log('Hi!');
    
    // log: Promise -> Hi! -> resolved!!!
```
Promise对象有三种状态:`pending（进行中）`、`fulfilled（已成功）`、`rejected（已失败）`。所以Promise只有两种情况从`pending`变为`fulfilled`和从`pending`变为`rejected`。
#### 5.async await
在项目中，需要等待网络请求然后执行某个函数，所以可以在外面使用async await。
```javascript
	async function getHost () {
	  const url = '......'
	  const result = await axios.get(url)
	  return Promise.resolve(result)
	}
	返回Promise对象。可以用`.then`调用
```

