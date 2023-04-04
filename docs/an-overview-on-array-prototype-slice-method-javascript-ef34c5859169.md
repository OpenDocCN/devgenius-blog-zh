# 数组概述。Prototype.slice 方法— JavaScript

> 原文：<https://blog.devgenius.io/an-overview-on-array-prototype-slice-method-javascript-ef34c5859169?source=collection_archive---------13----------------------->

![](img/0e71152397c750a80ce0b40b87d7a327.png)

JavaScript 的 Array slice()方法将数组的副本作为新数组返回，为了返回新数组，它也接受可选的参数，一个 start(包含)和一个 end(不包含),这意味着如果将 1 作为 start(索引)和 3 作为 end(索引)传递，那么 slice 将返回给定数组的新副本，其中包含两个值(索引)。slice()方法不改变原始数组。在数组的 slice()方法中，end 在内部被视为 end-1。

# 例子

```
const cars = ['Toyota', 'Volkswagen','Daimler','Ford Motor','Honda','BMW','Hyundai','Nisaan','Renault','Kia'];

// no value
console.log(cars.slice());
// expected output: [ 'Toyota', 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// single index
console.log(cars.slice(4));
// expected output: [ 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// from middle index
console.log(cars.slice(4,7));
// expected output: [ 'Honda', 'BMW', 'Hyundai' ]

// from beginning of the index
console.log(cars.slice(0,4));
// expected output: [ 'Toyota', 'Volkswagen', 'Daimler', 'Ford Motor' ]

// negative index
console.log(cars.slice(-4));
// expected output: [ 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// from index 6 to -3(excluded)
console.log(cars.slice(4,-1));
// expected output: [ 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault' ]
```

# JavaScript 数组。Prototype.slice()语法

```
slice()
slice(start)
slice(start, end)
```

正如你所看到的，你可以以多种方式使用 slice()方法，这取决于你想如何使用，因为正如你在上面的代码中看到的，你也可以使用负索引来访问数组的值，以根据需要创建自己的新数组。

# JavaScript 数组。Prototype.slice()参数

## 开始(可选)

开始提取的从零开始的索引。

可以使用负索引，它从数组的末尾开始选择。slice(-3)提取序列中的最后三个元素。

如果 start 被省略或未定义，slice 从索引 0 开始。

如果 start 大于序列(数组)的长度，将返回一个空数组。

## 结束(可选)

要从返回的数组中排除的第一个元素的索引。切片提取到但不包括 end，即 end-1。

可以使用负索引，表示从序列末尾的偏移量。

如果 end 被省略或未定义，则 slice 从序列 array.length 的末尾开始提取。

如果 end 大于序列的长度，则 slice 提取到序列 array.length 的末尾

# slice()函数的返回值

Array 方法 slice()基于提供的参数返回一个包含提取元素的新数组。

# 例子

## JavaScript slice()方法

```
const cars = ['Toyota', 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia'];

// slicing the hole array from starting to end
let ncars = cars.slice();
console.log(ncars);
// [ 'Toyota', 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// slicing from the middle of the array
let middleIndex = Math.floor(cars.length / 2);
let middleArr = cars.slice(middleIndex);
console.log(middleArr);
// [ 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// slicing from the second element to second last element
let second = 1;
let secondLast = cars.length - 1;
let almostFull = cars.slice(second, secondLast);
console.log(almostFull);
// [ 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault' ]
```

***输出:***

```
[ ‘Toyota’, ‘Volkswagen’, ‘Daimler’, ‘Ford Motor’, ‘Honda’, ‘BMW’, ‘Hyundai’, ‘Nisaan’, ‘Renault’, ‘Kia’ ]
[ ‘BMW’, ‘Hyundai’, ‘Nisaan’, ‘Renault’, ‘Kia’ ]
[ ‘Volkswagen’, ‘Daimler’, ‘Ford Motor’, ‘Honda’, ‘BMW’, ‘Hyundai’, ‘Nisaan’, ‘Renault’ ]
```

## JavaScript slice()负索引

在 JavaScript slice()方法中，可以对开始和结束参数使用负索引。最后一个元素的索引是-1，倒数第二个元素的索引是-2，依此类推。

```
const cars = ['Toyota', 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault', 'Kia'];

// slicing the array from second-to-last
let narr = cars.slice(1,-1);
console.log(narr);
// [ 'Volkswagen', 'Daimler', 'Ford Motor', 'Honda', 'BMW', 'Hyundai', 'Nisaan', 'Renault' ]

// slicing array from from last four
let lastfour = cars.slice(-4);
console.log(lastfour);
// [ 'Hyundai', 'Nisaan', 'Renault', 'Kia' ]

// both negative indexes
let bothNegative = cars.slice(-5,-2);
console.log(bothNegative);
// [ 'BMW', 'Hyundai', 'Nisaan' ]
```

***输出:***

```
[ ‘Volkswagen’, ‘Daimler’, ‘Ford Motor’, ‘Honda’, ‘BMW’, ‘Hyundai’, ‘Nisaan’, ‘Renault’ ]
[ ‘Hyundai’, ‘Nisaan’, ‘Renault’, ‘Kia’ ]
[ ‘BMW’, ‘Hyundai’, ‘Nisaan’ ]
```

*本文原载于*[*programmingeeksclub.com*](https://programmingeeksclub.com/javascript-array-prototype-slice-method-explained/)

*我的个人博客网址:* [*编程极客俱乐部*](https://programmingeeksclub.com/)
*我的脸书页面:* [*编程极客俱乐部*](https://www.facebook.com/profile.php?id=100086258693659)
*我的电报频道:* [*编程极客俱乐部*](https://t.me/dpgcl)
*我的推特账号:*[*kul deep Singh*](https://twitter.com/kusinghofficial)