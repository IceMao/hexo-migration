---
title: Javascript事件流
date: 2017-09-18 16:47:53
tags: 
    - javascript
    - html
categories: Javascript
---
## 事件流

>事件流描述的是从页面中接收事件的顺序。

### DOM事件流

>“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段，处于目标阶段和事件冒泡阶段。

{% fi http://hexoblog-1251943703.cossh.myqcloud.com/postImg/DOMeventflow.png, alt, "DOM事件流" %}

<!--more-->

1. 事件捕获阶段：为截获事件提供机会。实际的目标在捕获阶段(1,2,3)不会接收到事件。
2. 处于目标阶段：事件发生(4)，并在事件处理中被看成冒泡阶段的一部分。
3. 事件冒泡阶段：事件传播回文档(5,6,7)。

### 事件冒泡
  
* 事件开始时，具体元素接收，逐级向上传播到不具体的节点（文档）。
* 从下到上
* `<div>` -> `<body>` -> `<html>` -> `document`

### 事件捕获

在事件到达预定目标之前捕获它

* 不具体节点最早接收事件，具体节点最后接收。
* 从上往下
* `document` -> `<html>` -> `<body>` -> `<div>`

#### 注

1. IE9，Firefox，Chrome和Safari浏览器中会冒泡或捕获到 window对象