# String 和 StringBuilder 有什么区别？

> 原文：<https://blog.devgenius.io/what-is-the-difference-between-string-and-stringbuilder-bc533f8f16f2?source=collection_archive---------12----------------------->

![](img/3dcab04c5b3031561da3392ec6338701.png)

当应用程序必须处理大量字符串的编辑时，**字符串**和**字符串生成器**之间的区别是一个重要的概念。

# 线

字符串对象是由一个**系统表示的 **UTF-16** 代码单元的集合。Char** 属于系统名称空间的对象。由于这个对象的值**是只读的**，整个对象字符串被定义为**不可变的**。一个字符串对象在内存中的最大大小是 **2 GB** ，或者大约**10 亿个字符**。

# 不变的

不可变意味着每次系统的方法。字符串，则在内存中创建一个新的字符串对象，这将导致为新对象重新分配空间。

例如，通过使用字符串串联运算符+=显示名为 test 的字符串变量的值发生了变化。它生成一个新的 String 对象，该对象具有与原始对象不同的值和地址，并将它赋给测试变量。

```
string test;
test += “red”; // a new object string is created
test += “coding”; // a new object string is created
test += “planet”; // a new object string is created
```

# StringBuilder

StringBuilder 是一个属于**系统的动态对象。Text** 命名空间，并允许修改它封装的字符串中的字符数；这种特性叫做**可变性**。

# 易变性

StringBuilder 维护一个缓冲区来容纳字符串的扩展，以便能够追加、移除、替换或插入字符。如果缓冲区中有可用空间，则添加新数据。否则，分配一个新的、更大的缓冲区，将原始缓冲区中的数据复制到新缓冲区，然后将新数据追加到新缓冲区中。

```
StringBuilder sb = new StringBuilder(“”);
sb.Append(“red”);
sb.Append(“blue”);
sb.Append(“green “);
string colors = sb.ToString();
```

# 长度、容量和最大容量

*   StringBuilder。Length: 这个属性定义了 StringBuilder 实例当前包含的字符数。
*   **StringBuilder。Capacity:** 该属性指定当前实例可以包含的字符数。每当添加的字符数使长度大于容量时，capacity 属性的值就会加倍。
*   **StringBuilder。MaxCapacity:** 这个属性定义了可以分配给 StringBuilder 的最大大小。当达到最大容量时，无法为 StringBuilder 对象分配更多的内存。试图添加字符或将其扩展到超出其最大容量会引发**ArgumentOutOfRangeException**或 **OutOfMemoryException** 异常。

```
StringBuilder sb = new StringBuilder();
Console.WriteLine(“Capacity: {0} | Length: {1}”, sb.Capacity, sb.Length);
sb.Append(“This is a sentence.”);
Console.WriteLine(“Capacity: {0} | Length: {1}”, sb.Capacity, sb.Length);
for (int ctr = 0; ctr < 3; ctr++) 
{
    sb.Append(“This is an additional sentence.”);
    Console.WriteLine(“Capacity: {0} | Length: {1}”, sb.Capacity, sb.Length);
}// Output
// Capacity: 16 | Length: 0
// Capacity: 32 | Length: 19
// Capacity: 64 | Length: 50
// Capacity: 128 | Length: 81
// Capacity: 128 | Length: 112
```

# 演出

为了帮助您更好地理解 String 和 StringBuilder 之间的性能差异，我创建了以下示例:

```
Stopwatch timer = new Stopwatch();
string str = string.Empty;
timer.Start();
for (int i = 0; i < 10000; i++) 
{
    str += i.ToString();
}
timer.Stop();
Console.WriteLine(“String : {0}”, timer.Elapsed);
timer.Restart();
StringBuilder sbr = new StringBuilder(string.Empty);
for (int i = 0; i < 10000; i++) 
{
    sbr.Append(i.ToString());
}
timer.Stop();
Console.WriteLine(“StringBuilder : {0}”, timer.Elapsed);
// Output
// String : 00:00:00.0706661
// StringBuilder : 00:00:00.0012373
```