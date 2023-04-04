# 如何处理排序算法:合并排序

> 原文：<https://blog.devgenius.io/how-to-deal-with-sorting-algorithms-merge-sort-4308de4b84eb?source=collection_archive---------5----------------------->

![](img/60bfe4c46ddc50efcddf8a8e3191010e.png)

说到排序算法，人们应该永远记住:方法不止一种。上次我们谈到了[插入排序](https://medium.com/dev-genius/how-to-deal-with-sorting-algorithms-insertion-sort-1c14fd4fdb24)并使用了一个未排序的数组。这次我们将尝试一种不同的方法和更复杂的模式— **合并排序**。我们的 starter pack 是两个排序数组，我们的任务是将它们组合成一个排序数组。

让我们粉碎它！

实际上，我们的方法和上次插入排序没什么不同。我们的第一步是找到一个最小整数，并将其从数组中移除，但这次我们检查两个数组，而不是一个。

```
let firstHalf =  [2, 6, 7, 8, 11]
let secondHalf =  [3, 4, 5, 9, 10] //our two sorted arraysfunction findMinAndRemove(firstHalf, secondHalf) {
    let minfirstHalf = firstHalf[0]; 
    let minsecondHalf = secondHalf[0]; 
//here is first item of each array if(minfirstHalf < minsecondHalf) {
        return firstHalf.shift()
    } else {
        return secondHalf.shift()
    }
}
//The **shift()** method removes the first element from an array and returns that removed element. This method changes the length of the array. findMinAndRemove(firstHalf, secondHalf)
//2firstHalf
//[6, 7, 8, 11]
```

我们的第二步是——惊喜！—实际合并。在 merge 函数内部，我们将调用 findMinAndRemove 函数，直到其中一个数组为空，

```
function merge(firstHalf, secondHalf) {
    let sortedArr = [];
    let currentMin;while(firstHalf.length != 0 && secondHalf.length != 0) {
//we need BOTH condition to be true to make the loop run and when **one of the arrays is empty**, we know that the other one has only sorted elements, because well, it was a sorted array from the very beginning :)

    let currentMin = findMinAndRemove(firstHalf, secondHalf)
        sorted.push(currentMin)
//we add the minimum integer of both arrays
     }
    return sorted.concat(firstHalf).concat(secondHalf)
}//The **concat()** method is used to merge two or more arrays. This method does not change the existing arrays, but instead returns a new array.
```

这就是我们合并两个有序数组的方法。现在，让我们用同样的逻辑来处理**的一个无序数组**。

所以，我们应该采取的第一步是将一个无序的数组分割成子数组，直到每个元素都是一个独立的子数组。

我们的第二步是合并这些子阵列。重要提示:merge 只适用于排序后的数组，所以当一个数组只有一个元素时，它会被自动排序。然后我们可以合并，直到它成为一个有序的数组。

这是代码。

```
function mergeSort(array){
let midpoint = array.length/2
//first step - we split array every time we call a function
let firstHalf = array.slice(0, midpoint)
let secondHalf = array.slice(midpoint, array.length) if(array.length < 2){
       return array //when it's just one element, we return this one-element-array
    } else {
    merge(mergeSort(firstHalf), mergeSort(secondHalf))
      }
    }
```

这是排序和合并的基础。玩得开心！

![](img/7299835663d8f1592a26c17fbda8a7e9.png)