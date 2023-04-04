# 算法:什么是递归？

> 原文：<https://blog.devgenius.io/algorithms-what-is-recursion-ddaac64e2e03?source=collection_archive---------18----------------------->

什么是递归？这不是一个真正的算法，而是一些算法中使用的方法！在计算机科学和所有编码领域，递归是一种为了解决问题而重复执行步骤的方法。你可能会想，这和迭代有什么不同。可以把迭代看作是建立一个解决方案的过程，而递归把一个问题分解成更小的实例来得到解决方案。换句话说，把递归看作是一种把问题简化成更小的子问题的方法。这里有一个使用俄罗斯娃娃的视觉例子。

![](img/1edbffa36a56e48873ed7587dddfbce4.png)

图片来自维基百科

你打开一个，里面有足够多的娃娃，直到另一个装不下为止。

那么这一切和算法有什么关系呢？递归是算法中常用的方法。我实际上已经在一篇博客文章中介绍了一个使用递归的算法，但是我们将在这篇文章的后面讨论它！我们先来看一些例子。

```
function addUpTo(array, index) {
 if (index > 0) {
  return addUpTo(array, (index-1)) + array[index]
 } else {
   return array[index]
 }
}
```

这里，我们有一个函数，它将数组中的数字从它的开始处相加，或者将一个索引 0 加到您决定在数组的“终点”处使用的任何索引。例如，如果你有一个[1，2，3，4，5]的数组，并放入一个索引 2，它会加上 1，2，3，得出总数为 6。如果你把指数设为 1，它会把 1 和 2 相加，得出最终答案 3。

如此处所示，该函数在索引大于 0 的情况下调用自身进行重复。索引减 1，以便遍历每个索引。

让我们来分解一下这看起来是什么样子，以便更好地理解递归是如何工作的。让我们使用我在下面提供的数组和索引。

```
let a = [1,2,3,4,5]addUpTo(a, 4)
```

如果我们更深入地研究递归，这就是这个函数的样子。这就是为什么我用一套俄罗斯娃娃作为视觉例子。这个特定的函数继续下去，直到指数为 0，这可以比作一套俄罗斯娃娃，直到最后一个娃娃不能包含另一个娃娃在里面。

```
addUpTo( [1, 2, 3, 4, 5], 4)
 addUpTo( [1, 2, 3, 4, 5], 3)
  addUpTo( [1, 2, 3, 4, 5], 2)
   addUpTo( [1, 2, 3, 4, 5], 1)
    addUpTo( [1, 2, 3, 4, 5], 0)
```

我们用另一个算法来举例吧！

```
function isPalindrome(myString) {
 if (myString[0] === myString[(myString.length - 1)] && myString.length > 1) {
  isPalindrome(myString.slice(1, (myString.length - 1)))
   return true
 } else {
   return false
 }
}
```

这个函数检查你输入的字符串是否是回文。(回文是一个单词，如果你把它反过来，它的拼写完全相同)。如果你看第二行，类似于前面提到的算法，该条件将一直存在，直到每个元素(在这个上下文中是字母)都被遍历，并且直到该条件被满足，该函数将继续调用自己(因此是递归)。

让我们更深入地看看第一个条件的前半部分。该函数检查第一个和最后一个字母是否相同。如果没有，很简单，这不是一个回文。有趣的是第一个条件的后半部分。第一个条件的后半部分只是确保字符串仍然存在。

现在让我们看看第三行。对于这一部分，函数继续调用自己，检查字符串的第一个字母，并重用长度减一的索引值。

还记得我说过，递归实际上被用在我之前写过的一个算法中吗？那是[合并排序算法](https://medium.com/@jonathan_wong/algorithms-merge-sort-638679180288)！

```
function mergeSort(array) {
 let midPoint = array.length/2
 let firstHalf = array.slice(0, midPoint)
 let secondHalf = array.slice(midPoint, array.length)

 if (array.length < 2) {
  return array
 } else {
   return (merge(mergeSort(firstHalf), mergeSort(secondHalf)))
 }
};function merge(array1, array2) {
 let sorted = []
 while (array1.length > 0 && array2.length > 0) {
  if (array1[0] > array2[0]) {
   sorted.push(findMinAndRemoveSorted(array2))
  } else {
    sorted.push(findMinAndRemoveSorted(array1))
  }
 }
 return sorted.concat(array1).concat(array2)
}function findMinAndRemoveSorted(array) {
 return array.shift()
}
```

如果你仔细看，递归实际上在第一个函数的 else 条件中使用了两次**！类似于前面解释的两个例子，函数被调用，直到满足一个条件。如果你想了解这个算法的更多信息，请查看我的[上一篇博文](https://medium.com/@jonathan_wong/algorithms-merge-sort-638679180288)！**

**总之，把递归和算法想象成一个算法调用它自己并减少一个数组/字符串/等等，直到满足一个条件并返回解决方案。如果你注意到在我提到的所有例子中，算法中使用的数组和字符串在某种程度上被“简化”了，从而解决了问题。**