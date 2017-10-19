---
title: this的作用域
date: 2017-09-18 17:01:01
tags: 
    - javascript
---
## ES5

### 非严格模式

### 严格模式 
```` 
'use strict'

function thisFn() {
  console.info('this',this);
};

thisFn();// 
````
<!--more-->

## ES6

 调用者的作用域