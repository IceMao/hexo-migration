---
title: 关于javascript数字精度丢失
date: 2017-09-25 18:19:53
tags: 
    - javascript
categories: Javascript
---
### 典型问题

0.1 + 0.3 不等于0.3
````
0.1 + 0.2 == 0.3 //false
````

### 原因

计算机计算是将其它进制（十进制）转换为二进制。浮点数0.1和0.2转换为二进制是无限循环小数（双精度浮点数的小数部分最多支持52位）

<!--more-->

0.1 -> 0.0001 1001 1001 1001…（无限循环）

0.2 -> 0.0011 0011 0011 0011…（无限循环）

0.1 + 0.2 = 0.30000000000000004

### 解决方法

先将浮点数转换为整数，再除相应的倍数转换

````
var floatobj = function() {
    /**
     * 获取数值的小数位数
     * @param {number} n 传入的数值 
     */
    function getDigits(n) {
        if (Object.prototype.toString.call(n) !== '[object Number]') {
            return;
        } else if (String(n).indexOf('.') == -1) {
            return 0;
        } else {
            let len = n.toString().split('.')[1].length;
            return len;
        }
    }

    function operation(a, b, o) {

        aDigits = getDigits(a);
        bDigits = getDigits(b);

        let len = aDigits - bDigits;
        let digits, result;
        if (len >= 0) {
            b = b * Math.pow(10, aDigits);
            a = a * Math.pow(10, aDigits);
            digits = aDigits;
        } else {
            a = a * Math.pow(10, bDigits);
            b = b * Math.pow(10, bDigits);
            digits = bDigits;
        }
        switch (o) {
            case 'add':
                result = (a + b) / Math.pow(10, digits);
                break;
            case 'subtraction':
                result = (a - b) / Math.pow(10, digits);
                break;
            case 'multiply':
                result = (a * b) / Math.pow(10, digits * 2);
                break;
            case 'division':
                result = a / b;
                break;
            default:
                result = '有误';
                break;
        }
        return result;
    }

    //加法
    function add(a, b) {
        return operation(a, b, 'add')
    }

    //减法
    function subtraction(a, b) {
        return operation(a, b, 'subtraction')
    }

    //乘法
    function multiply(a, b) {
        return operation(a, b, 'multiply')
    }

    //除法
    function division(a, b) {
        return operation(a, b, 'division')
    }
    return {
        add: add,
        subtraction: subtraction,
        multiply: multiply,
        division: division
    }
}()
console.info('add: ', floatobj.add(0.1, 0.2))
console.info('subtraction: ', floatobj.subtraction(212.77, 10.00))

console.info('multiply: ', floatobj.multiply(0.1, 0.2))
console.info('division: ', floatobj.division(Number.MAX_VALUE, 4))
console.info('division: ', floatobj.division(-20.9, 4))
````