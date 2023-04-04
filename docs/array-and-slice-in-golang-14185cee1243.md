# Golang 中的数组和切片。

> 原文：<https://blog.devgenius.io/array-and-slice-in-golang-14185cee1243?source=collection_archive---------5----------------------->

## 让我们从 Golang 的基础开始

![](img/ee8414102149334eb1ff06d5f9f83267.png)

塔曼娜·茹米在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 数组语法:

像大多数编程语言一样，Go 也有数组，但在 Go 中很少使用数组。

```
/* Declearing array here [5] is the limit of array, int is the type of elements and 
{elements}	 
 */
var arry = [5]int {1,2,3,4,5}/* Declearing array here [...] means array limit is not fixed, int is the 
type of elements and {elements}	 
 */
var arry = [...]int {1,2,3,4,5,6,... ...}
```

# 切片语法:

在 Go 语言中，我们更多地使用切片而不是数组，它保存值的序列。切片消除了数组的限制。

```
// One dimensional Slice.var slice = []int {1,2,3,4,5}// Multidimensional Slice.var multi_slice = [][]int
```

# 数组和切片的区别:

> *使用[]生成数组，使用[]生成切片*

```
// Array 
var arry = [5]int {1,2,3,4,5}//Slice 
var slice = []int {1,2,3,4,5}
```

# 切片:

```
package mainimport ( "fmt" )
 func main() { var x = []int {4, 3, 4, 5, 6, 7} fmt.Println(x[:4]) }
 //Output : 4 3 4 5
```

> *注意:当我们对切片进行切片时，我们必须使用正数，因为索引必须为正。*

# 附加功能:

内置的 append 函数用于在切片中追加数据。

```
package main import ( "fmt" ) func main() {    var x []int    x = append(x, 4, 3, 4, 5, 6, 7) fmt.Println(x) } // Output : 4 3 4 5 6 7
```

如果有任何遗漏信息或错误，请告诉我，我刚刚从 Python 迁移到 Go。

## 结论

数组是任何编程语言的基础，这也是为什么我试图弄清楚这个基本概念。

感谢您的阅读！