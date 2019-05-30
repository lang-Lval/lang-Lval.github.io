---
title: js常见数组的方法及高阶函数
tags: js
categories: js
date: 2019-03-18 16:40:18
---

### forEach的用法
 一般数据遍历方法
 ```
   var array = [1,2,3,4,5,6,7];  
    for (var i = 0; i < array.length; i) {  
        console.log(i,array[i]);  
    }  
```
for in 方法遍历数组
<!--more-->
```
    for(let index in array) {  
        console.log(index,array[index]);  
    };  
```
forEach 方法遍历数组
```
array.forEach（function(v,i,arr){  
    console.log(v);  
});
 
```
语法：array.forEach(function(v, i, arr), thisValue)
上述代码中第一个参数v代表当前元素，第二个参数i代表当前元素的索引（可选），第三个参数arr代表当前元素所属的数组对象（可选）


thisValue 可选。传递给函数的值一般用 "this" 值。
如果这个参数为空， "undefined" 会传递给 "this" 值

特点：
 forEach() 方法对数组的每个元素执行一次提供的函数。总是返回undefined；
 forEach() 方法会改变原数组

 ```
 var array1 = [1,2,3,4,5];
 
 varp = array1.forEach(function(value,index){
 
    console.log(value);   //可遍历到所有数组元素
 
    return value + 10
});
console.log(p);   //undefined    无论怎样，总返回undefined
```

### map 方法
语法：
和forEach一样
array.map(function(currentValue,index,arr), thisValue)
第一个参数v代表当前元素，第二个参数i代表当前元素的索引（可选），第三个参数arr代表当前元素所属的数组对象（可选）
```
var p1 = array1.map(function(value,index){
 
    console.log(value);   //可遍历到所有数组元素
 
    return value + 10
});
console.log(p1);   //[11, 12, 13, 14, 15]   返回一个新的数组
```

不同于forEach的是：
map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

map() 方法按照原始数组元素顺序依次处理元素。

注意： map() 不会对空数组进行检测。

注意： map() 不会改变原始数组。


### filter
filter也是一个常用的操作，它用于把Array的某些元素过滤掉，然后返回剩下的元素。和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。
```
//例如，在一个Array中，删掉偶数，只保留奇数，可以这么写：
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15],返回满足过滤条件元素的数组
```


filter()接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示Array的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：
```
var arr = ['A', 'B', 'C'];
var r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});


//利用filter，可以巧妙地去除Array的重复元素：

var p,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
    p = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
alert(p.toString());
//去除重复元素依靠的是indexOf总是返回第一个元素的位置，后续的重复元素位置与indexOf返回的位置不相等，因此被filter滤掉了。

```


### sort排序算法

数组的sort()方法默认把所有元素先转换为String再排序，比如'10'排在了'2'的前面，因为字符'1'比字符'2'的ASCII码小。如果使用sort()方法的默认排序规则，直接对数字排序，结果肯定是错误的，
但sort()方法也是一个高阶函数，它可以接收一个比较函数来实现自定义的排序
```
//升序排列：
var arr = [10, 20, 30, 1, 2, 3];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
}); // [1, 2, 3, 10, 20, 30]


//降序排列：
var arr = [10, 20, 30, 1, 2, 3];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [30, 20, 10, 3, 2, 1]



//sort()方法会直接对Array进行修改，它返回的结果仍是当前Array：
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象 
```

### every()
定义和用法
every() 方法用于检测数组所有元素是否都符合指定条件（通过函数提供）。

every() 方法使用指定函数检测数组中的所有元素：

如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
如果所有元素都满足条件，则返回 true

注意： every() 不会对空数组进行检测。

注意： every() 不会改变原始数组。

语法：array.every(function(currentValue,index,arr), thisValue) 
currentValue	（必须。当前元素的值）
index	（可选。当前元素的索引值）
arr	  （可选。当前元素属于的数组对象）
thisValue （可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。
如果省略了 thisValue ，"this" 的值为 "undefined"）
返回值：布尔值。如果所有元素都通过检测返回 true，否则返回 false。
```
//检测数组内所有元素是否大于10
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// passed is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true
```


### some()
定义和用法
some() 方法用于检测数组中的元素是否满足指定条件（函数提供）。

some() 方法会依次执行数组的每个元素：

如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测。
如果没有满足条件的元素，则返回false。
注意： some() 不会对空数组进行检测。

注意： some() 不会改变原始数组。

语法：array.some(function(currentValue,index,arr),thisValue) 形式与every相同

返回值：布尔值。如果数组中有元素满足条件返回 true，否则返回 false。
```
//检测数组中是否有大于10的元素
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [2, 5, 8, 1, 4].some(isBigEnough);
// passed is false
passed = [12, 5, 8, 1, 4].some(isBigEnough);
// passed is true
```