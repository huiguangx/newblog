---
title: JS基础
permalink: /pages/c68879/
categories:
  - JS基础
summary: js基础
tags:
  - js基础
abbrlink: 31880
date: 2020-10-01 11:35:12
---

# JS数组的十二种遍历方法

## 一 、for

普通版

```js
for (let i = 0; i < arr.length; i++) {
    // ...
}
```

优化版

```js
// 使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。
let len = arr.length;
for (let i = 0; i < len; i++) {
    // ...
}
```

**`for in`** 

作用：循环对象或者数组

```js
var obj = {
    name : 'abc',
    age : 18,
    sex : '男'
};
var arr = ['abc', 18, '男'];
for(prop in obj){
    console.log(prop);  //name age sex
    console.log(typeof(prop));  //string
}      
for(prop in arr){
    console.log(prop);  //0 1 2
    console.log(typeof(prop));  //string
}//for in循环的是对象，那么它的变量名就是对象的属性名，如果循环的是数组，那么它的变量名就是数组的索引。
```

`for` 循环和 `for...in` 能正确响应 `break`、`continue` 和 `return` 语句，但 `forEach` 不行。

## 二、forEach

注意：`forEach` 不会对空数组进行检测

```js
arr.forEach((item, index, arr) => {
    // ...
})
```

> `item` : 每个元素 `index` : 元素数组下标 `arr` : 数组本身

## 三、 map

- `map` 有返回值
- `map` 和 `forEach` 都不会对空数组进行检测
- `map` 不会改变原始数组

```js
const result = arr.map((item, index, arr) => {
	return item * 2;
})

console.log(result); // [2, 4, 6]
```

## 四、 for...of

只有可迭代对象（`iterator`）才能使用（`Array`, `Map`, `Set`, `String,` `TypedArray`, `arguments` 对象等等）

```js
for (let item of arr) {
    // ...
}
```

五、 `filter`

接收一个回调函数作为参数，返回值是一个新数组，**把传入的函数依次作用于每个元素**，然后根**据返回值是true还是false决定保留还是丢弃**该元素。

注意：

- filter() 不会对空数组进行检测
- filter() 不会改变原始数组

```js
const arr = [
    {name: 'tony', age: '20'},
    {name: 'jack', age: '18'}
]

const result1 = arr.filter(item => {
    return item.age >= 20;
})

const result2 = arr.filter(item => {
    return item.age >= 20;
})

console.log(result1); // [{name: 'tony', age: 20}]
console.log(result2); // [] --> 没有满足条件的就返回一个空数组
```

## 六、 every

如果每一项返回 `true`, 则返回 `true`（全部符合条件），否则返回 `false`

- `every` 不会对空数组进行检测
- `every` 不会改变原始数组

```js
const arr = [1, 2, 3, 4, 5];
const result = arr.every((item, index, arr) => {
    return item > 3; // 每一项是否大于3
})

console.log(result); // false
----------------------------------------------------------
const arr = [1, 2, 3, 4, 5];
const result = arr.every((item, index, arr) => {
    return item > 0; // 每一项是否大于0
})

console.log(result); // true
```

## 七、 some

`some` 如果每一项都返回 `true`，则返回 `true`（只要有一个符合），否则返回 `false`。

- `some` 不会对空数组进行检测
- `some` 不会改变原数组

```js
const arr = [1, 2, 3, 4, 5];
const result = arr.some((item, index, arr) => {
    return item > 3; // 数组有没有大于3的
})

console.log(result); // true --> 只要有一个大于3结果都为 true
```

## 八、 reduce

`reduce` 顺着挨个累加，对于空数组是不会执行回调函数的。

```js
const arr = [1, 2, 3];
// 没指定初始值的情况下
arr.reduce((initialValue, currentValue, index, arr) => {
    console.log(initialValue); // 第一次循环 initialValue = 1
    return initialVaule + currentValue; // 6 --> 从左往右累加起来
})

// 给了初始值之后
arr.reduce((initialValue, currentValue, index, arr) => {
    console.log(initialValue); // 第一次循环 initialValue = 10
    return initialVaule + currentValue; // 16 --> 从左往右累加起来
}, 10)
```

## 九、 reduceRight

`reduceRight` 倒着挨个累加，[遍历](https://so.csdn.net/so/search?q=遍历&spm=1001.2101.3001.7020)的是空数组的话，不会执行

```js
const arr = [1, 2, 3];
arr.reduceRight((initialValue, currentValue, index, arr) => {
    console.log(initialValue); // 第一次循环 initialValue = 3
    return initialVaule + currentValue; // 6
})

arr.reduceRight((initialValue, currentValue, index, arr) => {
    console.log(initialValue); // 第一次循环 initialValue = 10
    return initialVaule + currentValue; // 16
}, 10)
```

## 十、 find

找到符合条件的第一项，没找到返回 `undefined`，对于空数组，是不会执行的，并且不改变原数组

```js
const arr = [
    {name: 'tony', age: '20'},
    {name: 'jack', age: '18'}
]

const result1 = arr.find(item => {
    return item.name === 'jack';
})
console.log(result1); // {name: 'jack', age: 18}

const result2 = arr.filter(item => {
    return item.namee === 'mary';
})
console.log(result2); // undefined

```

## 十一、 fiendIndex

返回符合条件的第一项的下标，没有则返回 -1，对于空数组，是不会执行的，并且不改变原数组

```js
const arr = [
    {name:'tony1',age:'20'},
    {name:'tony2',age:'20'},
    {name:'tony3',age:'20'},
    {name:'jack',age:"30"}, // 只管返回第一个满足条件的
    {name:'jack',age:"100"}
];

const result = arr.findIndex(item => {
    return item.name === 'jack';
})

console.log(result); // 3
```

## 十二、 keys、values、entries

它们都返回一个遍历器对象，可以用 `for...of` 循环进行遍历

- `keys` – 返回元素索引
- `values` – 返回元素本身
- `entries` – 返回元素和下标

```
const arr = ['a', 'b', 'c'];

for (let index of arr.keys()) {
    console.log(index);
   	// 0
    // 1
    // 2
}

for (let ele of arr.values()) {
    console.log(ele);
    // a
    // b
    // c
}

for (let [index, ele] of arr.entries()) {
    console.log(idnex, ele);
    // 0 "a"
    // 1 "b"
    // 2 "c"
}
```

