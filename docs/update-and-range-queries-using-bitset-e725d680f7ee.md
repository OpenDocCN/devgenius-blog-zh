# 使用位集更新和范围查询

> 原文：<https://blog.devgenius.io/update-and-range-queries-using-bitset-e725d680f7ee?source=collection_archive---------26----------------------->

![](img/a0a3512b01e0b8f87054ed184d8a61a7.png)

由 [Ales Krivec](https://unsplash.com/@aleskrivec?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在竞争性编程中，人们一定遇到过这样的问题:需要更新数组中的特定索引，还需要为多个查询找到给定范围内元素的总和。解决这类问题的一种方法是段树，但也有一种简单的技术可以优化解决这类问题，这就是所谓的位集。

首先，让我们看看什么是位集。BitSet 创建一个由布尔值表示的位数组。BitSet 只为每个布尔值占用 1 位空间，因此 BitSet 占用的空间小于布尔数组或向量。BitSet 在 C++和 Java 中都可用，但我将用 Java 来解释。BitSet 也有一些限制，但让我们先看一个例子。

给你一个大小为 N 的数组，有 Q 个查询。每个查询属于以下类型-
类型 1:计算给定范围内奇数的个数。
类型 2:统计给定范围内偶数的个数。
类型 3:用给定的数字替换索引 X 处的元素。

一种强力方法是对每个查询的范围内的偶数或奇数进行计数，但是对于 n 大小的 10⁵和 10⁵数量的查询，每个查询的最坏情况可以是 O(N*Q ),其等于 O(N*N ),这将导致 TLE。

如果你知道如何使用细分树，它显然是一个很好的解决方案。但是让我们在这里使用 BitSet。

```
import java.io.*;
import java.util.*;class Main {
 public static void main (String[] args) {
  Scanner in = new Scanner(System.in);
  int N = in.nextInt();
  int arr[] = new int[N];
  for(int i = 0; i < N; i++){
      arr[i] = in.nextInt();
  }
  // Initializing the BitSet of size N
  BitSet B = new BitSet(N);
  for(int i = 0; i < N; i++){
      if(arr[i] % 2 == 1)
      // Making the ith bit set where arr[i] is odd
      B.set(i);
  }
  int Q = in.nextInt();
  while(Q-- != 0){
      int type = in.nextInt();
      int X = in.nextInt();
      int Y = in.nextInt();
      if(type == 1){
          // Making new BitSet of given range
          BitSet B1 = B.get(X-1,Y);
          // Count no. of index whose bit is set
          System.out.println(B1.cardinality());
      }
      else if(type == 2){
          // Making new BitSet of given range
          BitSet B2 = B.get(X-1,Y);
          // Count no. of index whose bit is unset
          System.out.println((Y-X+1) - B2.cardinality());
      }
      else{
          // If previous element at index X is even and
          // new element is odd then set the bit at index X
          if(arr[X-1] % 2 == 0 && Y % 2 == 1) 
          B.set(X-1);
          // If previous element at index X is odd and
          // new element is even then unset the bit at index X
          else if(arr[X-1] % 2 == 1 && Y % 2 == 0) 
          B.flip(X-1);
      }
  }
 }
}
```

创建一个与数组大小相同的位集，如果第 I 个元素是奇数，则设置第 I 位。对于类型 1 查询，使用[**get**](https://docs.oracle.com/javase/7/docs/api/java/util/BitSet.html#get%28int,%20int%29)(int from index，int toIndex)方法从原始位集生成给定范围的新位集。使用 [**基数**](https://docs.oracle.com/javase/7/docs/api/java/util/BitSet.html#cardinality%28%29) ()方法打印该范围内奇数个元素的数量。Cardinality 方法给出了位集中集合位的计数。位集代替段树的原因是基数方法的时间复杂度是 O(1 ),并且它不给出 TLE。

类似地，对于类型 2 查询，为了找到给定范围内偶数元素的数量，从原始位集生成该范围的新位集，并从该范围内的元素数量中减去该位集的基数，以获得答案。

对于类型 3 的查询，检查要被替换的元素是否是偶数，新元素是否是奇数，然后在该索引处设置该位，否则，如果要被替换的元素是奇数，新元素是偶数，则在该索引处翻转该位。

因此，使用 BitSet，这个问题以一种简单而又优化的方式解决了。请注意，类似的位集函数在 C++中也是可用的。

对于任何特定问题，您也可以使用位集数组。假设，给你一串小写字母，你必须对每个字符进行一些更新和范围查询，然后你可以根据问题使用位集数组，然后你可以使用各种位集方法来解决问题。

```
BitSet B[] = new BitSet[26];
for(int i = 0; i < 26; i++){
    // BitSet of size of length of string for each character
    B[i] = new BitSet(N);
}
```

根据问题的不同，BitSet 可以用在许多其他地方，您可以使用它来优化解决方案。

编码快乐！