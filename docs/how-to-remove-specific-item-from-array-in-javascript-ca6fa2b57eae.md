# 如何在 Javascript 中从数组中移除特定项

> 原文：<https://blog.devgenius.io/how-to-remove-specific-item-from-array-in-javascript-ca6fa2b57eae?source=collection_archive---------14----------------------->

在本文中，我们将了解如何用 javascript 从数组中移除特定的项。

我们将使用 indexOf()方法和 splice()方法从 javascript 中特定索引处的数组中移除项目。

这里，我们将使用 index of()方法找到要移除的数组的索引，然后使用 splice()方法移除索引。

此外，我们将使用 filter()方法从数组中删除特定的值。

**indexOf()** 方法返回字符串中值的第一个索引。如果字符串中没有值，则 [**indexOf()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 返回-1。

**splice()** 方法用于通过移除或替换现有元素和/或添加新元素来更改数组的内容。 [**splice()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 方法覆盖原来的数组元素。

方法创建给定数组的副本。 [**filter()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 方法不改变原始数组，也不执行空数组元素的函数。

因此，让我们看看如何在 jquery 中从数组中移除特定的项。

# Javascript 从数组中移除特定的项

**示例:**

```
const array = [2, 3, 5];

console.log(array);
const index = array.indexOf(3);
if (index > -1) { // only splice array when item is found
  array.splice(index, 1); // 2nd parameter means remove one item only
}
// array = [2, 5]
console.log(array);
```

**输出:**

```
[ 2, 3, 5 ]
[ 2, 5 ]
```

**阅读另:** [**如何使用 JavaScript 将 HTML 转换成 PDF**](https://websolutionstuff.com/post/how-to-convert-html-to-pdf-using-javascript)

**例如:**

第一个函数删除单个匹配项，就像删除给定数组值[2，3，5，7，3，9，3]中第一个匹配项 3 一样。第二个功能是删除给定数组值中的所有匹配项。

```
function removeItemOnce(arr, value) {
    var index = arr.indexOf(value);
    if (index > -1) {
      arr.splice(index, 1);
    }
    return arr;
  }

  function removeAllItem(arr, value) {
    var i = 0;
    while (i < arr.length) {
      if (arr[i] === value) {
        arr.splice(i, 1);
      } else {
        ++i;
      }
    }
    return arr;
  }
  // Usage
  console.log(removeItemOnce([2,3,5,7,3,9,3], 3))
  console.log(removeAllItem([2,3,5,7,3,9,3], 3))
```

**输出:**

```
[ 2, 5, 7, 3, 9, 3 ]
[ 2, 5, 7, 9 ]
```

**例如:**

```
var value = 3

var arr = [2,3,5,7,3,9,3]
arr = arr.filter(function(item) {
    return item !== value
})
console.log(arr)
```

**输出:**

```
[2, 5, 7, 9]
```

**你可能也会喜欢:**

*   **另请阅读:** [**如何在 JavaScript**](https://websolutionstuff.com/post/how-to-search-object-by-id-and-remove-it-from-json-array-in-javascript) 中通过 ID 搜索对象并将其从 JSON 数组中移除
*   **读取还:** [**在导出数据表**中的数据时移除/隐藏列](https://websolutionstuff.com/post/remove-hide-columns-while-export-data-in-datatable)
*   **阅读另:** [**如何从 JavaScript 数组中移除元素**](https://websolutionstuff.com/post/how-to-remove-elements-from-javascript-array)
*   **另请参阅:** [**如何使用 JQuery**](https://websolutionstuff.com/post/how-to-remove-spaces-using-jquery) 删除空格

如果这篇文章有帮助，请点击拍手👏下面的按钮。