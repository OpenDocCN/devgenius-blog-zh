# 用于测试自动化的 Javascript)

> 原文：<https://blog.devgenius.io/javascript-for-test-automation-e8-eaef5fc16579?source=collection_archive---------13----------------------->

# 数组

数组是一个项目序列，每个项目都有一个任意类型的值。可以通过将一系列值放在方括号([])中来定义数组:

```
> let myArray = [2, 'Pete', 2.99, 'another string']
```

这个例子突出了阵列的异构性；myArray 包含数值和文本值。数组可以包含任何东西，包括对象和其他数组。数组中的每个元素都有一个唯一的索引号，指示它在数组中的位置。因此，数组既是索引列表又是排序列表。数组元素可以通过它的索引号来引用。索引号放在数组名称或值后面的方括号([])中。例如，我们可以获得 myArray 的第一个成员，如下所示:

```
> myArray[0]
= 2
/* An array's initial entry is always at index 0\. As a result, the second element is at index 1, the third element is at index 2, and so on. To get to the third member of an array, type: */
> myArray[2]
= 2.99
```

知道 myArray 有四项，就很容易理解 myArray[3]返回第四项。假设你不知道数组中有多少个元素。你如何到达最后一个元素？不难，但是有点难。数组的长度属性实现了这个技巧:

```
> myArray[myArray.length - 1]
= 'another string'
```

我们必须使用数组的名称两次，因为我们必须考虑索引为 0 的元素，所以我们必须从长度中减去 1 以获得最后一个元素的索引。起初，零基索引看起来不寻常，使数学更加困难，但一旦你习惯了它——这需要练习——它很快就会成为第二天性。

## 修改数组

如果我们不能改变数组，它们将毫无用处。需要添加、删除和替换项目的能力。[]操作符和几个数组函数有助于这些活动。

## 用`[]`替换和添加元素

```
/* To replace an element of an array, use brackets ([]) with the assignment operator:*/> let array = [1, 2, 3]
> array[1] = 4
= 4> array
= [ 1, 4, 3 ]/* You can also use [] to add new elements to an array: */> array[array.length] = 10
= 10> array
= [ 1, 4, 3, 10 ]
```

因为前一个结束元素位于数组[array . length-1]，所以 array[array.length] = value 向数组的末尾添加一个新值。值得注意的是，用 const 声明并初始化为数组的变量行为很奇怪；虽然不能改变变量引用的数组，但可以改变数组的内容:

```
> const MyArray = [1, 2, 3]
> MyArray[1] = 5
> MyArray
= [1, 5, 3]> MyArray = [4, 5, 6] // Uncaught TypeError: Assignment to constant variable.
```

这种行为可能令人困惑。它起源于“变量作为指针”的概念，我们稍后会讲到。本质上，const 声明禁止修改 const 指向的项，但它不禁止改变该项的内容。因此，我们可以改变常量数组中的一个元素，但不能改变常量指向的数组。如果希望数组的元素也是常量，可以使用 Object.freeze 方法:

```
> const MyArray = Object.freeze([1, 2, 3])
> MyArray[1] = 5
> MyArray
= [1, 2, 3]/* Take note of how the assignment on line 2 had no effect on the array's content. It is critical to understand that Object.freeze only operates one level deep in the array. If your array includes nested arrays or other objects, their values can still be modified unless they are likewise frozen:*/> const MyArray = Object.freeze([1, 2, 3, [4, 5, 6]])
> MyArray[3][1] = 0
> MyArray
= [1, 2, 3, [4, 0, 6]]/* You must freeze the sub-array if you want it to be constant as well: */> const MyArray = Object.freeze([1, 2, 3, Object.freeze([4, 5, 6])])
> MyArray[3][1] = 0
> MyArray
= [1, 2, 3, [4, 5, 6]]
```

## 用`push`添加元素

push 函数将一个或多个数组成员附加到末尾:

```
> array.push('a')
= 5               // the new length of the array> array
= [ 1, 4, 3, 10, 'a' ]> array.push(null, 'xyz')
= 7> array
= [ 1, 4, 3, 10, 'a', null, 'xyz' ]
```

push 函数将其参数附加到调用者(数组)，导致调用者发生变化。然后返回数组的更新长度。记住方法和函数都执行动作和返回值。你必须小心辨别这两者。Push 将元素追加到调用方数组的末尾，但它返回数组的更新长度。需要注意的是，它不返回改变后的数组！新的 JavaScript 程序员经常对这种区别感到困惑，并浪费时间去弄清楚为什么一个函数没有返回预期的值。如果您有任何疑问，请参阅文档。

## 用`concat`添加元素

Concat 类似于 push 但是，它不会改变调用方。它连接两个数组并生成一个新数组，该数组包含原始数组的所有项，后跟提供给它的所有参数:

```
> array.concat(42, 'abc')
= [ 1, 4, 3, 10, 'a', null, 'xyz', 42, 'abc' ]> array
= [ 1, 4, 3, 10, 'a', null, 'xyz' ]
```

## 用`pop`移除元素

Pop 是 push 的反义词。当 push 向数组末尾添加一个元素时，pop 移除并返回数组的最后一个成员:

```
> array.pop()
= 'xyz'            // removed element value> array
= [ 1, 4, 3, 10, 'a', null ]// pop mutates the caller.
```

## 用`splice`移除元素

拼接功能允许您从数组中删除一个或多个成员，即使它们不在末尾:

```
> array.splice(3, 2)
[ 10, 'a' ]> array
= [ 1, 4, 3, null ]
```

在本例中，我们从索引位置 3 开始删除两个组件。splice 修改原始数组并返回一个新数组，包括被删除的元素。

## 迭代方法

## 用`forEach`迭代

JavaScript 包括几个内置方法，用于遍历数组的元素。forEach 函数是在循环和迭代一章中介绍的。这是跨数组迭代的直接方法；与 for 循环相比，样式标准通常更喜欢使用它。forEach 需要一个回调函数，该函数作为输入传递给 ForEach。回调函数是作为参数传递给另一个函数的函数。当被调用的函数被执行时，它调用回调函数。forEach 方法为每个元素调用一次回调，将其值作为参数传递。forEach 每次都返回 undefined。

```
let array = [1, 2, 3];
array.forEach(function(num) {
  console.log(num); // on first iteration  => 1
                    // on second iteration => 2
                    // on third iteration  => 3
}); // returns `undefined`
```

这段代码为每个数组条目调用一次回调方法。forEach 在每次迭代中使用元素的值作为参数来调用回调。之后，回调将它报告给控制台。最后，forEach 产生 undefined。我们也可以使用一个箭头函数来代替函数表达式，这使得我们的代码更加简洁，在你熟悉语法之后更容易理解。

```
let array = [1, 2, 3];
array.forEach(num => console.log(num));// We can also perform more complex operations:let array = [1, 2, 3];
array.forEach(num => console.log(num + 2)); // on first iteration  => 3
                                            // on second iteration => 4
                                            // on third iteration  => 5
```

## 用`map`变换数组

当您希望使用数组元素的值时，forEach 是一个不错的选择。但是假设您希望构建一个新数组，它的值依赖于旧数组的内容。假设您希望构建一个包含调用数组中所有整数平方的新数组。对于 forEach，您可能会得到类似这样的结果:

```
> let numbers = [1, 2, 3, 4]
> let squares = [];
> numbers.forEach(num => squares.push(num * num));
> squares
= [ 1, 4, 9, 16 ]> numbers
= [ 1, 2, 3, 4 ]
```

这很好，但是回调现在有了一个意想不到的副作用:它更新了 squares 变量，这不是回调方法的一部分。有时副作用会引起问题。考虑一下，如果在运行上述代码后重复 forEach 调用，会发生什么情况:

```
> numbers.forEach(num => squares.push(num * num));
> squares
= [ 1, 4, 9, 16, 1, 4, 9, 16 ]/* Because we failed to reset squares to an empty array, we now have two copies of the squares. This issue is handled more cleanly using the map method: */> let numbers = [1, 2, 3, 4]
> let squares = numbers.map(num => num * num);
> squares
= [ 1, 4, 9, 16 ]> squares = numbers.map(num => num * num);
= [ 1, 4, 9, 16 ]
```

此代码的前四行提供了与前面的 forEach 示例相同的结果。但是，map 生成一个新数组，其中每个 numbers 元素有一个成员，每个元素都设置为回调的返回值:在本例中，是数字的平方。这段代码比 forEach 代码短，并且没有副作用；回调不改变方块(map 的返回值改变方块)，如果我们再次运行相同的代码，我们不必重置变量。forEach 和 map 是有用的方法，但是它们可能会让新手感到困惑。主要区别在于，forEach 进行基本迭代并返回 undefined，而 map 改变数组的项并返回一个包含修改值的新数组。

## 用`filter`过滤数组

另一种数组迭代方法是 filter 方法。它产生一个新数组，包含回调函数为其返回 true 值的调用数组中的所有元素。那是相当多的。以下代码可能有助于您理解过滤器的作用:

```
> let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2]
> numbers.filter(num => num > 4)
= [ 5, 6, 7, 8, 9, 10 ]> numbers
= [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2 ]
```

filter 循环遍历数组的元素。它在每次迭代时调用回调函数，将当前元素的值作为参数传递。如果回调返回真值，元素的值将被追加到一个新数组中。否则，它会忽略元素的值，什么也不做。当过滤器完成迭代后，它将传递所选元素的数组:回调返回 true 结果的元素。在这种情况下，过滤器选择值大于 4 的所有组件。过滤器不会改变调用者。

## 用`reduce`从数组构建新值

reduction 方法有效地将数组的内容降低到单个值。它是最难掌握的数组迭代算法之一，但也是最基本的算法之一。Reduce 可用于创建 forEach、map 和 filter 函数，以及为数组指定的各种其他迭代技术。reduce 接受两个参数:一个回调函数和一个将累加器设置为零的值。在其最基本的形式中，回调函数接受两个参数:累加器的当前值和一个数组条目。它返回一个值，该值将在下面的回调调用中用作累加器。这看起来比实际要难，所以让我们看看 reduce 的两种简单用法:

```
> let arr = [2, 3, 5, 7]
> arr.reduce((accumulator, element) => accumulator + element, 0)
= 17> arr.reduce((accumulator, element) => accumulator * element, 1)
= 210
```

第一次调用计算所有数组值的总和，例如 2 + 3 + 5 + 7。首先，我们将累加器设置为 0。因此，在第一次调用回调方法时，累加器为 0，元素为 2。当我们再次调用回调时，这一次是使用元素 3，它返回 2，这成为新的累加器值。反过来，该调用产生 5。重复此过程，直到最终返回值为 17。第二次调用 reduce 计算数组整数的乘积(2 * 3 * 5 * 7)，1 作为累加器。(如果我们从 0 开始，最终返回值将是 0，因为 0 乘以任何值都是 0。)归约函数不仅用于计算整数；它还可以计算文本、对象和数组。这里有一个字符串的例子:

```
> let strings = ['a', 'b', 'c', 'd']
> strings.reduce((accumulator, element) => {
...   return accumulator + element.toUpperCase()
... }, '');
= 'ABCD'
```

调用者没有被 reduce 变异。(回调可能会改变调用者，尽管不建议这样做，这也不是 reduce 的错。)

## 数组可以是奇数

JavaScript 数组包含几个奇怪的行为，可能会让您放松警惕。如果您使用过其他语言的数组，这些异常可能会特别令人吃惊。

1.  您已经看到了一个奇怪之处:索引从 0 开始。这并不罕见，因为大多数计算机语言都使用从零开始的索引。然而，学生在使用数组时经常出错。
2.  length 属性总是返回一个比数组中利用率最高的索引点大 1 的值。例如，如果索引位置 124 处存在一个元素，并且没有其他项具有更高的索引值，则数组的长度为 125。
3.  数组是对象。因此，当应用于数组时，typeof 运算符不会生成“array”:

```
> let arr = [1, 2, 3]
> typeof arr
= 'object'/* If you really need to detect whether a variable references an array, you need to use Array.isArray instead: */> let arr = [1, 2, 3]
> Array.isArray(arr)
= true
```

4.当您将数组的 length 属性更改为一个新的更小的值时，数组将被截断。JavaScript 删除新的最后一个元素之后的所有元素。

5.当您将数组的 length 属性修改为更大的新值时，数组将扩展到新的大小。新项目未初始化，导致意外行为:

```
> let arr = []
> arr.length = 3
> arr
= [ <3 empty items> ]> arr[0]
= undefined> arr.filter(element => element === undefined)
= []> arr.forEach(element => console.log(element)) // no output
= undefined> arr[1] = 3
> arr
= [ <1 empty item>, 3, <1 empty item> ]> arr.length
= 3> arr.forEach(element => console.log(element))
= 3
= undefined> Object.keys(arr)
= ['1']/* Unset array members are seen as missing by JavaScript in general, although the length property includes them. */
```

6.您可以创建索引为负数、非整数甚至非数字的数组“元素”:

```
> arr = [1, 2, 3]
= [ 1, 2, 3 ]> arr[-3] = 4
= 4> arr
= [ 1, 2, 3, '-3': 4 ]> arr[3.1415] = 'pi'
= 'pi'> arr["cat"] = 'Fluffy'
= 'Fluffy'> arr
= [ 1, 2, 3, '-3': 4, '3.1415': 'pi', cat: 'Fluffy' ]/* These "elements" aren't genuine elements; they're array object characteristics that we'll go over later. Only index values (0, 1, 2, 3, and so on) contribute to the array's length. */> arr.length
= 3
```

7.因为数组是对象，所以 Object.keys 函数可用于将数组的键(其索引值)作为字符串数组返回。还包括负索引、非整数或非数字索引。

```
> arr = [1, 2, 3]
> arr[-3] = 4
> arr[3.1415] = 'pi'
> arr["cat"] = 'Fluffy'
> arr
= [ 1, 2, 3, '-3': 4, '3.1415': 'pi', cat: 'Fluffy' ]> Object.keys(arr)
= [ '0', '1', '2', '-3', '3.1415', 'cat' ]/* One quirk of this method is that it treats unset values in arrays differently from those that merely have a value of undefined. Unset values are created when there are "gaps" in the array; they show up as empty items until you try to use their value: */> let a = new Array(3);
> a
= [ <3 empty items> ]> a[0] === undefined;
= true> let b = [];
> b.length = 3;
> b
= [ <3 empty items> ]> b[0] === undefined;
= true> let c = [undefined, undefined, undefined]
> c
= [ undefined, undefined, undefined ]> c[0] === undefined;
= true/* Unlike the length property of Array, which counts unset values, Object.keys only counts items that have been set to a value: */> let aKeys = Object.keys(a)
> a.length
= 3
> aKeys.length;
= 0> let bKeys = Object.keys(b)
> b.length
= 3
> bKeys.length;
= 0> let cKeys = Object.keys(c)
> c.length
= 3
> cKeys.length;
= 3/* It is worthwhile to devote some time and effort to memorizing these habits. */
```

## 嵌套数组

数组的元素可以包含任何东西，甚至是其他数组。您可以创建内部包含数组的数组，也可以创建内部包含数组的数组。假设您希望跟踪参加混合双打网球比赛的所有队伍。你可以做一个这样的数组。

```
> let teams = [['Joe', 'Jennifer'], ['Frank', 'Molly'], ['Dan', 'Sarah']]// You can now find the teams by index.> teams[2]
= [ 'Dan', 'Sarah' ]/* If you want to retrieve the second element of teams[2], you can append [1] to the expression: */> teams[2][1]
= 'Sarah'
```

## 数组相等

你相信下面的短语会返回什么？

```
> [1, 2, 3] === [1, 2, 3]/* It returns true, which is a sensible answer. After all, the arrays appear to be similar. However, JavaScript is not appropriate in this case: the expression returns false. */> [1, 2, 3] === [1, 2, 3]
= falseHow about the following comparison?> let a = [1, 2, 3]
> let b = a
> a === b
```

令人惊讶的是，这个对比率是正确的。这是怎么回事？只有当两个数组是同一个数组时，JavaScript 才认为它们相等:它们必须占用相同的内存位置。这条规则适用于所有 JavaScript 对象；对象必须相同。因此，第二个示例返回 yes，但第一个示例返回 false。当 an 赋给 b 时，b 和 a 指的是同一个数组；它不会生成新的数组。

乍一看，第一个例子中的数组似乎是“同一个数组”，但它们不是。它们是恰好包含相同值的两个不同的数组。但是，因为它们在内存中占据不同的位置，所以它们不是同一个数组，因此不相等。考虑到这种行为，如何判断两个数组是否有相同的元素呢？一种可能是编写一个函数，将一个数组中的元素与另一个数组中的元素进行比较:

```
function arraysEqual(arr1, arr2) {
  if (arr1.length !== arr2.length) return false;for (let i = 0; i < arr1.length; i += 1) {
    if (arr1[i] !== arr2[i]) {
      return false;
    }
  }return true;
}console.log(arraysEqual([1, 2, 3], [1, 2, 3]));    // => true
console.log(arraysEqual([1, 2, 3], [4, 5, 6]));    // => false
console.log(arraysEqual([1, 2, 3], [1, 2, 3, 4])); // => false
```

arraysEqual 比较两个数组，如果一个数组中的元素与另一个数组中的元素不匹配，则返回 false。如果不是，则返回 true。如果数组长度不同，它立即返回 false 首先处理这个场景简化了剩余的过程。当两个数组的项都是基元值时，arraysEqual 效果最好。如果任何一对元素具有非基元值(数组或对象)，arraysEqual 可能不会产生预期的结果:

```
> arraysEqual([1, 2, [3, 4], 5], [1, 2, [3, 4], 5])
= false
```

## 其他数组方法

## 包含

```
/* The includes method checks whether an array contains a given element: */> let a = [1, 2, 3, 4, 5]
> a.includes(2)
= true> a.includes(10)
= false
```

## 分类

```
/* The sort method is a convenient technique to reorder the elements of an array in order. It yields a sorted array. */> let a = ["e", "c", "h", "b", "d", "a"]
> a.sort()
= [ 'a', 'b', 'c', 'd', 'e', 'h' ]
```

## 薄片

```
/* The slice method, unlike the splice method, extracts and returns a subset of the array. There are two optional arguments. The first is the index at which extraction begins, and the second is the index at which extraction concludes: */> let fruits = ['mango', 'orange', 'banana', 'pear', 'apple']
> fruits.slice(1, 3)
= [ 'orange', 'banana' ]> fruits.slice(2) // second argument defaults to rest of array
= [ 'banana', 'pear', 'apple' ]> fruits.slice() // no arguments duplicates the array
= [ 'mango', 'orange', 'banana', 'pear', 'apple' ]/* If the second parameter is omitted, slice returns the remainder of the array, beginning at the position specified by the first argument. It returns the entries up to but omitting that index with the second parameter. (Compare this to how splice handles its second parameter.) If no parameters are provided, slice produces a duplicate of the entire array: that is, a new array with the same items as the original. This comes in handy when you need to apply a destructive technique on an array that you don't want to change. */
```

## 反面的

```
// An array's order is reversed using the reverse method.> let numbers = [1, 2, 3, 4]
> numbers.reverse()
= [ 4, 3, 2, 1 ]> numbers
= [ 4, 3, 2, 1 ]/* The opposite is destructive: it changes the array. If you don't want to change the original array, as described in the preceding section, you can use slice with no arguments: */> let numbers = [1, 2, 3, 4]
> let copyOfNumbers = numbers.slice();
> let reversedNumbers = copyOfNumbers.reverse()
> reversedNumbers
= [ 4, 3, 2, 1 ]> numbers
= [ 1, 2, 3, 4 ]
```