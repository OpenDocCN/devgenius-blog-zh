# 如何处理排序算法:插入排序

> 原文：<https://blog.devgenius.io/how-to-deal-with-sorting-algorithms-insertion-sort-1c14fd4fdb24?source=collection_archive---------6----------------------->

![](img/7d794b5a3ec55aca228f730a51c9398c.png)

从我不太长但不知何故富有成效的学习算法的经历中，我学到了最重要的一课:你显然可以忘记一些高级概念，但你应该永远记住，如何解决一些基本问题。这就是为什么你总是知道从哪里开始你的算法评估过程。

比如排序。实际上，在现实世界中，我们要求计算机做的很多事情都涉及到将物品按正确的顺序排列。所有的过滤/搜索/推荐/价格范围操作都需要一些排序，或多或少有些高级。

说到算法，当我们的数组排序后，很多问题都变得简单了。例如，我们可以更容易地实现二分搜索法方法:当一个数组排序时，找到它的中值是很简单的，我们不只是猜测——我们确切地知道我们需要看下面还是上面。

让我们来看看这个简单的例子。

```
let sortedArray = [-1, 1, 3, 5, 6]
array[2]
// 3
// so know need to look lower
// only need to look at indices 0 and 1
let unsortedArray = [5, 6 -1, 1, 3]
array[2]
// -1
// so guess again
```

因此，使用排序数组的好处是非常明显的。让我们解决一个简单的算法，使用**插入排序。**这种类型的排序与您对手中扑克牌的排序非常相似。你从一堆未分类的卡片中挑选卡片，并按正确的顺序将它放入一个已分类的卡片中。

我们的任务是这样的:我们需要将数组中的整数从最小到最大排序。

```
//FIRST STEP: find, remove and return the smallest integer one by one. We should call this function until there are no other elements left in this array.function findMinAndRemove(arr){
    let min = arr[0];
    let minIndex = 0;    
    for(let i = 0; i < arr.length; i++) {
        if(arr[i] < min) {
            min = arr[i];
            minIndex = i;
        } //we find a minimum value and its index 
    }
    arr.splice(minIndex, 1); //.splice() return an updated array
    return min;
}//SECOND STEP: create a sorted array by pushing the smalles integers to the end of this arrayfunction selectionSort(arr){
    let newMin;
    let sortedArr = [];  //our new sorted array while(arr.length != 0) {
        newMin = findMinAndRemove(arr); //the return value from FIRST STEP
        sortedArr.push(newMin) //adding 
    }
    return sortedArr
}
```

这可能看起来令人困惑，但通过练习，你会明白的！:)

![](img/af09b05e88724f9f845167eb088012ef.png)