# JS 系列#14:高级 JS 特性

> 原文：<https://blog.devgenius.io/js-series-14-advance-js-features-cd4eab15bbd?source=collection_archive---------14----------------------->

## 默认参数，休息和传播和解构

![](img/c09757e03c66199b60728d55b8b53117.png)

在我们之前的文章中，我们已经介绍了很多新的 JavaScript 特性。现代 Js 引入了许多新功能，如下所列…

1.  箭头功能
2.  字符串模板文字
3.  保持不变
4.  为了…在
5.  为了…的
6.  指数算子
7.  String.includes()
8.  Array.includes()
9.  Object.values()
10.  休息和伸展
11.  解构
12.  默认参数函数
13.  对象增强
14.  班级
15.  异步和等待

我们在之前的文章中已经讨论了很多。在本文中，我们主要关注四个特性。

1.  默认参数函数
2.  传播
3.  休息参数
4.  解构

# 默认参数函数

`Default parameter function`是一种在`function definition`中创建`optional`参数的方法，可以在方法调用时跳过这些参数。

## 示例— 1 |常规函数

```
function multiply(x , y) {
   return x * y;
}multiply(3,5); // 15
multiply(5); // NaN
```

正如你在上面的例子中看到的，这个函数需要 2 个参数。当用两个参数调用`multiply(3, 5)`时，它返回预期的输出。

但是当第二个参数被跳过时，第二个参数被当作`undefined`处理，表达式变成`x * undefined`导致`NaN`值。

因此，我们可以改进函数，以获得更好、更有意义的结果，如下所示…

```
**Example-1:**
function multiply(x , y) {
   if(y === undefined) {
       y = 1;
   } return x * y;
}**Example-2:** function multiply(x , y) {   
   y = y === undefined ? 1 : y;
   return x * y;
}multiply(3,5); // 15
multiply(5); // 5
```

在上面的例子中，如果变量`y`是`undefined`，我们有意将`1`赋给变量`y`。

JS 中介绍的更好的方法是`default argument parameter`。请参见下面的示例…

## 示例-默认参数函数

```
function multiply(x , y = 1) {
    return x * y;
}multiply(9,10) // 90
multiply(5) // 5
```

在上面的例子中，如果`second parameter`通过，那么它覆盖`default parameter`，否则使用`default parameter`值。

# 传播算子

根据 MDN， **Spread 语法** ( `...`)允许在需要零个或多个参数**(对于函数调用)**或元素**(对于数组文字)**的地方扩展一个可迭代的表达式，比如一个`array`表达式或字符串，或者在需要零个或多个键值对**(对于对象文字)**的地方扩展一个对象表达式。

如上面定义中所强调的，`Spread`用于 3 种情况…

1.  函数调用
2.  数组文字
3.  对象文字

让我们逐一讨论以上 3 个用例。

## 函数调用中的扩展运算符

```
function sum(a,b,c) {
    return a + b + c;
}console.log(sum(1,2,3)); // 6
console.log(sum([1,2,3])); // '1,2,3undefinedundefined'
```

正如你在上面的例子中所看到的，`sum()`函数接受 3 个数值，当它作为`sum(1,2,3)` 被调用时，它返回`6`，但是当它被像`sum([1,2,3])`一样的`array`调用时，整个`array`变成了`first argument`，第二个和第三个参数保持不变`undefined`导致所有 3 个值的连接。

但是使用`spread`操作符，来自数组的所有 3 个值可以分别扩展成 3 个变量。

```
sum(...[1,2,3]); // 6
```

让我们再看一个使用`Math.max()`函数找到最大值的例子。

```
console.log(Math.max(1,3,9,2,88,5)) // 88
console.log(Math.max([1,5,2])) // NaN
console.log(Math.max(...[1,5,2])) // 5
```

正如您已经知道的，`Math.max()`函数接受 2 个或更多参数来查找`max`值。

如上例所示，当您在`max()`函数中传递一个`array` 时，它会返回`NaN`。通过使用`spread`操作符，我们可以解决这个问题，因为`spread`操作符将扩展`array`值。

## 在数组文字中展开

`array`文字中的`Spread`运算符使用现有数组创建一个新的`array`。将一个`array`中的元素分散到一个新的`array`中。

```
const nums1 = [1,2,3];
const nums2 = [4,5,6];const nums = [...nums1,...nums2];
console.log(nums); // [1, 2, 3, 4, 5, 6]console.log([...nums1,...nums2,7,8,9]); // [1, 2, 3, 4, 5, 6, 7, 8, 9]console.log(['x','y',...nums1]); // ['x', 'y', 1, 2, 3]
```

## 在对象文字中传播

`object`中的`Spread`文字将属性从一个`object`复制到另一个`object`文字。

```
const person = {name:'Alex', age:44 , salary: 100000};
const student = {name:'Alia',age:22, marks: 85};const studentInfo = {...student,fees:50000}
console.log(studentInfo); // {name: 'Alia', age: 22, marks: 85, fees: 50000}const personInfo = {gender:'male',...person};
console.log(personInfo); // {gender: 'male', name: 'Alex', age: 44, salary: 100000}const obj = {...student,...person}
console.log(obj); // {name: 'Alex', age: 44, marks: 85, salary: 100000}
```

如果两个`objects`相同，那么顺序中后面出现的`object’s property`将覆盖前面的。

# 休息参数

`Rest parameter`是`ES6`中引入的另一个令人兴奋的特性。它所代表的`3 dots(…)`看起来与`spread`相似但不是。其实和`spread`运算符几乎相反。

与`Spread operator`，`Rest parameters`允许`function`接受`infinite`个参数作为`array`。在其他一些编程语言中，如`Java`，它也被称为`varargs`。看看下面的例子就明白了`Rest parameters`。

```
function sum(...args) {
    let total = 0;
    for(let i = 0; i < args.length; i++) {
        total += args[i];
    }
    return total;
}sum(1,2,3) // 6
sum(2,3,4,5) // 14
```

上述示例可以通过`reduce()`方法复制。

```
function sum(...args) {
    return args.reduce((total,item) => {
        return total + item;
    });
}
```

在上面的例子中，您可以看到`args`使用`rest parameters`接收多个参数作为一个`array`。

## Rest 参数语法

> `function f(a, b , …args)`

如果你查看上面的`rest parameter`的`syntax`，你可以很容易地观察到`Rest parameters`可以传递额外的参数，但是`rest parameters`必须在最后被写入。

```
function add(x,y,...args) {
    let total = x + y;
    for(let i = 0; i < args.length; i++) {
         total += args[i];
    }
    return total;
}add(5,6,1,2,3,4) // 21
```

在上例中，前两个参数由`a`和`b`接收，其余参数由`args`接收。

## 关于 Rest 参数的要点

1.  函数定义只能有一个 rest 参数，不允许有多个 rest 参数。

```
foo(...x,...y,...z) // Invalid
```

2.rest 参数必须是函数定义中的最后一个参数。

```
bar(...args, param1, param2) // Invalid
```

# arguments 对象—实现 Rest 参数功能的旧方法

正式的方式是，当`JS`中没有引入`Rest parameters(...args)`时，每个`function`中的`arguments`参数就是`built-in`参数，可以接收任意数量的`parameters`，你可以通过它`iterate`来检索参数。

```
function sum() {
    let total = 0;
    for(let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}sum(1,2,3) // 6
```

每个功能都有一个名为`arguments`的`default parameter`来接收`rest parameters`。其余参数是没有被任何定义的参数`variables`接收的参数。

# Rest 参数 V/S 参数对象

1.  这个`arguments`物体不是真正的`array`。您不能对`pop()`、`sort()`、`push()`、`forEach()`等参数执行任何`array`操作，而`rest parameter(…args)`会创建一个`array`实例，您可以使用`rest parameters`执行上述所有`array`操作。
2.  `arguments`对象不能使用`arrow`功能，而`rest parameters(…args)`可以使用`arrow`功能。

# 解构

`Destructuring`是一个`short`或`clean`语法，用于将`Values from an array`、`properties from an Object`解包到不同的变量中。

```
const cars = ['BMW','AUDI','VW','Maruti']
const first = cars[0];
const second = cars[1];console.log(first,second); // BMW AUDi
```

在上面的例子中，我们已经使用`index`从`array`中获取了`first`和`second`元素。但是使用`destructuring`可以有更干净的方法来做同样的事情，如下例所示…

```
const cars = ['BMW','AUDI','VW','Maruti']
const [first,second] = cars
console.log(first,second) // BMW AUDI
```

现在如果你在想，有没有办法将`skip`元素与`destructuring`元素结合起来？所以答案是`Yes`。请参见下面的示例…

```
const cars = ['BMW','AUDI','VW','Maruti']
const [first , , third] = cars
console.log(first,second) // BMW VW
```

如果您想在两个变量中收集`first`和`second`元素，并在一个`array`中收集`rest`元素，还有另一种方法，因此这可以通过组合`destructuring`和`rest parameters`来完成。

```
const cars = ['BMW','AUDI','VW','Maruti']
const [first , second , ...others] = cars
console.log(first,second, others) // BMW AUDI ['VW', 'Maruti']
```

## 析构对象属性

如上例所示，你可以看到`destructuring`通过`position`解包`array`元素，但是在`objects`的情况下`destructuring`使用`property`名称。

```
const person = {name:'Alex',age:'30',married: 'NO'}const {name,age} = person;
console.log(name,age) // Alex 30const {age,married} = person;
console.log(age,married) // 30 NO
```

你可以在上面的例子中看到使用属性名从一个对象中获取属性。但是有没有可能`property`名和`resultant variable`名可以不一样？所以答案是`Yes`。要实现这一点，您需要使用如下例所示的其他`syntax`…

```
const person = {name:'Alex',age:'30',married: 'NO'}
const {age,married:maritalStatus} = person;
console.log(age,maritalStatus)// 30 NO
```

如果你喜欢这篇文章，请关注我:

【中:】[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)