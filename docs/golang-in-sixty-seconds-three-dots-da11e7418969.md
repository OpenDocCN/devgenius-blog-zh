# 六十秒后的 Golang 三个点…

> 原文：<https://blog.devgenius.io/golang-in-sixty-seconds-three-dots-da11e7418969?source=collection_archive---------9----------------------->

![](img/ffe7a8baf1de2e0cfb0eaeb122bc8ab2.png)

照片由 [Bekky Bekks](https://unsplash.com/@bekkybekks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

三点符号`...`可以用在一些地方:

## 创建数组文本时指定长度

```
arr := [...]string{"One", "Two", "Three", "Four", "Five"}
fmt.Println(len(arr)) // 5
```

注意，这创建了一个固定长度的**数组**，而不是一个片。所以像`append`这样的方法是行不通的。

## 可变函数

这是一个可以接受任意数量尾随参数的函数。通过在类型前加上`...`来创建一个变量函数，如下所示:

```
func sum(nums ...int) int {
  total := 0
  for _, num := range nums {
    total += num
  }
  return total
}
```

在本例中，nums 将是`ints`的`slice`。你可以这样称呼`sum`:

```
total := sum(1, 2, 15, 24)
fmt.Println(total) // 42
```

您还可以使用`...` a 符号将一个**片**(不是一个数组)传递给一个变量函数，如下所示:

```
slice := []int{2, 5, 9, 8}
total = sum(slice...)
fmt.Println(total) // 24
```

[试试这里](https://go.dev/play/p/e7m8J3pRRLi)

## 程序包列表的通配符

您很可能已经看到或使用过这个命令:

```
go test ./...
```

它仅仅意味着“测试这个目录(和子目录)中的所有**包**

更[六十秒后 Golang](https://richard-t-bell90.medium.com/list/golang-in-sixty-seconds-7a26c5131734)

[*获得无限制的介质*](https://richard-t-bell90.medium.com/membership)

[*给我买杯咖啡*](https://ko-fi.com/richardtbell) *如果你喜欢这篇文章的话:)*