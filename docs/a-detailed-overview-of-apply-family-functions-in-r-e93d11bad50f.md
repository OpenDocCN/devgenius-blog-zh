# R 中应用系列函数的详细概述

> 原文：<https://blog.devgenius.io/a-detailed-overview-of-apply-family-functions-in-r-e93d11bad50f?source=collection_archive---------5----------------------->

## 选择正确的应用功能不再是一场噩梦！

![](img/e9a99626164659752664734055e4a8dd.png)

Apply-family 函数是 R 生态系统的一些最典型的特性。它们给 R 一种函数式编程范式的感觉。它们允许用户在不同的数据类型上运行函数。它们基本上都是循环。速度稍快，但看起来更整洁。这八个兄弟中的一些非常容易使用，而一些非常适合某些类型的操作。不过，以我的拙见，两个主要的好处是，它们使您的代码更加整洁和可读，并且与标准循环相比，它们与并行计算小工具更加兼容。

即使是最有经验的 R 用户有时也会在何时使用哪一个或者如何使用这个漂亮的函数家族时失败。这就是为什么这篇博文的目的是给 R 程序员一个关于 apply-family 函数的详细指南。我们开始吧！

以下是八个应用函数:

*   应用
*   拉普利
*   伤感的
*   vapply
*   地图
*   塔普利
*   rapply
*   eapply

# 1.应用()

apply()函数仅用于类似数组的数据类型，如 data.frame 和 matrix，并对这些数组按列或按行运行函数。而不是数组，函数接受一个 MARGIN 参数，这意味着如果我们传递 1，则按行操作；如果传递 2，则按列操作。

如果你的数组包含一个字符串变量，apply()可能不是最有用的工具，但是它非常适合科学计算，在科学计算中，我们只需要在列或行的基础上对矩阵运行函数。

```
example_matrix <- matrix(data = round(rnorm(n = 100 , mean = 100 , sd = 20)) , nrow = 10 , ncol = 10)apply(X = example_matrix , MARGIN = 1 , FUN =  mean)##  [1] 105.0 101.6  90.4 101.4 104.6 105.7 113.5  98.3 108.0  97.1apply(X = example_matrix, MARGIN = 2 , FUN = IQR)##  [1] 13.50 25.50 16.00 24.25 35.25 29.50 35.50 22.25 23.25 10.25apply(X = example_matrix , MARGIN = 1 , FUN = sd)##  [1] 21.83779 17.06328 19.92876 27.10965 22.19209 21.91423 17.16100 18.12334
##  [9] 14.09492 24.84373apply(X = example_matrix , MARGIN = 2 , FUN = median)##  [1] 118.5  92.5  93.0 116.5 106.0 101.5  97.5  86.5 103.0 113.5apply(X = example_matrix , MARGIN = 1  , max)##  [1] 135 127 118 159 133 139 140 117 126 132apply(X = example_matrix , MARGIN = 2  , min)##  [1] 90 68 61 96 76 59 60 73 74 77
```

# 2.拉普利()

lapply()获取一个列表，对其循环一个函数并返回一个列表。

```
example_list_numeric <- list(
  v1 = round(rnorm(10 , 100 , 20)) ,
  v2 = round(rnorm(10 , 100 , 20)) ,
  v3 = round(rnorm(10 , 100 , 20)) ,
  v4 = round(rnorm(10 , 100 , 20)) ,
  v5 = round(rnorm(10 , 100 , 20)) 
)example_list_string <- list(
  v1 = sample(x = c(letters , LETTERS) , size = 10 , replace = T) , 
  v2 = sample(x = c(letters , LETTERS) , size = 10 , replace = T) , 
  v3 = sample(x = c(letters , LETTERS) , size = 10 , replace = T) , 
  v4 = sample(x = c(letters , LETTERS) , size = 10 , replace = T) , 
  v5 = sample(x = c(letters , LETTERS) , size = 10 , replace = T) 
)lapply(X = example_list_numeric , FUN = mean)## $v1
## [1] 91.5
## 
## $v2
## [1] 90.6
## 
## $v3
## [1] 98.2
## 
## $v4
## [1] 101.4
## 
## $v5
## [1] 84lapply(X = example_list_string , FUN = tolower)## $v1
##  [1] "f" "n" "r" "o" "u" "j" "r" "a" "f" "u"
## 
## $v2
##  [1] "j" "q" "t" "s" "x" "j" "x" "h" "c" "c"
## 
## $v3
##  [1] "w" "i" "o" "i" "m" "l" "i" "j" "v" "c"
## 
## $v4
##  [1] "o" "y" "h" "y" "m" "a" "v" "l" "d" "q"
## 
## $v5
##  [1] "w" "n" "b" "w" "z" "u" "b" "d" "i" "d"lapply(X = example_list_string , FUN = toupper)## $v1
##  [1] "F" "N" "R" "O" "U" "J" "R" "A" "F" "U"
## 
## $v2
##  [1] "J" "Q" "T" "S" "X" "J" "X" "H" "C" "C"
## 
## $v3
##  [1] "W" "I" "O" "I" "M" "L" "I" "J" "V" "C"
## 
## $v4
##  [1] "O" "Y" "H" "Y" "M" "A" "V" "L" "D" "Q"
## 
## $v5
##  [1] "W" "N" "B" "W" "Z" "U" "B" "D" "I" "D"
```

如果您希望一次使用多个函数，可以将它们包含在一个匿名函数中，并将该匿名函数作为参数传递给 FUN 参数。

```
lapply(X = example_list_numeric , FUN = **function**(x) c(mean(x) , sd(x)))## $v1
## [1] 91.50000 22.16228
## 
## $v2
## [1] 90.60000 20.59234
## 
## $v3
## [1] 98.20000 24.93012
## 
## $v4
## [1] 101.4000  26.8295
## 
## $v5
## [1] 84.00000 22.03028lapply(X = example_list_string , FUN = **function**(x) c(tolower(x) , toupper(x)))## $v1
##  [1] "f" "n" "r" "o" "u" "j" "r" "a" "f" "u" "F" "N" "R" "O" "U" "J" "R" "A" "F"
## [20] "U"
## 
## $v2
##  [1] "j" "q" "t" "s" "x" "j" "x" "h" "c" "c" "J" "Q" "T" "S" "X" "J" "X" "H" "C"
## [20] "C"
## 
## $v3
##  [1] "w" "i" "o" "i" "m" "l" "i" "j" "v" "c" "W" "I" "O" "I" "M" "L" "I" "J" "V"
## [20] "C"
## 
## $v4
##  [1] "o" "y" "h" "y" "m" "a" "v" "l" "d" "q" "O" "Y" "H" "Y" "M" "A" "V" "L" "D"
## [20] "Q"
## 
## $v5
##  [1] "w" "n" "b" "w" "z" "u" "b" "d" "i" "d" "W" "N" "B" "W" "Z" "U" "B" "D" "I"
## [20] "D"
```

# 3.萨普利()

sappy()代表 simplify apply，是 lappy()的一个特例，它尽可能返回最简单的列表形式。更准确地说，sapply()是 lappy()的包装函数，如果 simplify 参数设置为 FALSE，它的行为与 lappy()完全一样。

当一个列表被传递给 sapply()时，它返回一个 vector 对象如果列表中的每个元素都是一个且只有一个元素的向量，它返回一个 matrix 或 data.frame 对象如果列表中的每个元素都是一个长度相同的向量，最后它返回一个列表。

```
list_1 <- list(
  v1 = round(rnorm(10 , 100 , 20)) ,
  v2 = round(rnorm(10 , 100 , 20)) ,
  v3 = round(rnorm(10 , 100 , 20)) ,
  v4 = round(rnorm(10 , 100 , 20)) ,
  v5 = round(rnorm(10 , 100 , 20)) 
) list_2 <- list(
  v1 = round(rnorm(15 , 100 , 20)) ,
  v2 = round(rnorm(8 , 100 , 20)) ,
  v3 = round(rnorm(17 , 100 , 20)) ,
  v4 = round(rnorm(3 , 100 , 20)) ,
  v5 = round(rnorm(10 , 100 , 20)) 
) sapply(X = list_1 , FUN = mean)##    v1    v2    v3    v4    v5 
## 111.8  98.5  94.5  92.2 101.1sapply(X = list_1 , FUN = **function**(x) c(mean(x) , length(x) , sd(x)))##             v1       v2       v3       v4        v5
## [1,] 111.80000 98.50000 94.50000 92.20000 101.10000
## [2,]  10.00000 10.00000 10.00000 10.00000  10.00000
## [3,]  23.85046 18.35605 18.38629 18.32303  15.75119sapply(X = list_2 , FUN = abs)## $v1
##  [1] 116  98 142 122 122 124  78 114 113  95 111 108  77  81 119
## 
## $v2
## [1]  93 107  96 119  83  96  85 114
## 
## $v3
##  [1] 100  87 134 110 116  99 105  66  94  77 106 110 132 114  94 135  74
## 
## $v4
## [1]  89 120  85
## 
## $v5
##  [1] 119  60  91 117  67 138 125 125  93 112
```

# 4.vapply()

应用系列函数的下一个成员是 vapply()。vapply()也可以看作是 lapply()和 sapply()的另一个表亲。使用 vapply()，我们可以通过。不同于 lapply()和 sapply()，我们有一个参数叫做 FUN。值，我们向该值传递一个向量，该向量表示我们希望得到的回报。我们作为参数传递给 FUN 的向量的内容。值不重要。它只是告诉 R 我们想要类似的东西。对 sapply()来说是比较安全的选择。以下是例子。

```
vapply(X = example_list_numeric , FUN = mean , FUN.VALUE = 7)##    v1    v2    v3    v4    v5 
##  91.5  90.6  98.2 101.4  84.0vapply(X = example_list_numeric , FUN = mean , FUN.VALUE = 681681614817)##    v1    v2    v3    v4    v5 
##  91.5  90.6  98.2 101.4  84.0vapply(X = example_list_numeric , FUN = mean , FUN.VALUE = 0.25)##    v1    v2    v3    v4    v5 
##  91.5  90.6  98.2 101.4  84.0vapply(X = example_list_numeric , FUN = **function**(x) c(mean(x) , sd(x)) , FUN.VALUE = c(58 ,3))##            v1       v2       v3       v4       v5
## [1,] 91.50000 90.60000 98.20000 101.4000 84.00000
## [2,] 22.16228 20.59234 24.93012  26.8295 22.03028vapply(
  X = example_list_numeric , 
  FUN = **function**(x) c(mean(x) , sd(x) , length(x)) ,
  FUN.VALUE = c(0.84 , 643 , 9))##            v1       v2       v3       v4       v5
## [1,] 91.50000 90.60000 98.20000 101.4000 84.00000
## [2,] 22.16228 20.59234 24.93012  26.8295 22.03028
## [3,] 10.00000 10.00000 10.00000  10.0000 10.00000vapply(X = example_list_string , FUN = toupper , 
       c('50' , 'sdfg' , 'dfg' , 'sd' , 'TRUE' , 's' , 'dfg' , 'd' , 'd' , 'd') )##       v1  v2  v3  v4  v5 
##  [1,] "F" "J" "W" "O" "W"
##  [2,] "N" "Q" "I" "Y" "N"
##  [3,] "R" "T" "O" "H" "B"
##  [4,] "O" "S" "I" "Y" "W"
##  [5,] "U" "X" "M" "M" "Z"
##  [6,] "J" "J" "L" "A" "U"
##  [7,] "R" "X" "I" "V" "B"
##  [8,] "A" "H" "J" "L" "D"
##  [9,] "F" "C" "V" "D" "I"
## [10,] "U" "C" "C" "Q" "D"vapply(X = example_list_string , FUN = tolower , 
       c('Obama' , 'Izmir' , 'dfg' , 'sd' , 'TRUE' , 's' , '0.86' , 'mean' , 'R is excellent' , 'd') )##       v1  v2  v3  v4  v5 
##  [1,] "f" "j" "w" "o" "w"
##  [2,] "n" "q" "i" "y" "n"
##  [3,] "r" "t" "o" "h" "b"
##  [4,] "o" "s" "i" "y" "w"
##  [5,] "u" "x" "m" "m" "z"
##  [6,] "j" "j" "l" "a" "u"
##  [7,] "r" "x" "i" "v" "b"
##  [8,] "a" "h" "j" "l" "d"
##  [9,] "f" "c" "v" "d" "i"
## [10,] "u" "c" "c" "q" "d"
```

注意我传递的向量的内容是多么的无意义和随机。它们只是象征我们想要的虚拟形象。

# 5.地图应用()

下一个系列成员是 mapply()，代表多元应用。这里的要点是，我们将一个函数应用于一个对象的元素，并使用该函数的不同参数，我们也将这些参数作为参数传递给 mapply()。默认情况下，它生成列表输出，但是我们得到了如 sapply()中的 simplify 参数。乍一看，mapply()似乎有点复杂，但是它非常强大和方便。下面的例子会让你看得更清楚。

```
mapply(FUN = rep , 1:4, 4:1)## [[1]]
## [1] 1 1 1 1
## 
## [[2]]
## [1] 2 2 2
## 
## [[3]]
## [1] 3 3
## 
## [[4]]
## [1] 4mapply(FUN = rep , x = 1:4 , times = 4:1)## [[1]]
## [1] 1 1 1 1
## 
## [[2]]
## [1] 2 2 2
## 
## [[3]]
## [1] 3 3
## 
## [[4]]
## [1] 4mapply(FUN = rep , times = 1:4, x = 4:1)## [[1]]
## [1] 4
## 
## [[2]]
## [1] 3 3
## 
## [[3]]
## [1] 2 2 2
## 
## [[4]]
## [1] 1 1 1 1vector_1 <- round(rnorm(n = 10 , mean = 100 , sd = 20))vector_2 <- 1:10mapply(FUN = **function**(x , y) x/y  , vector_1 , vector_2)##  [1] 96.000000 49.000000 42.000000 20.500000 15.600000 20.166667 12.428571
##  [8] 14.000000  9.111111 12.400000mapply(FUN = **function**(x , y) x*y  , x = vector_1 , y = vector_2)##  [1]   96  196  378  328  390  726  609  896  738 1240mapply(FUN = **function**(x , y) x+y  , y =  vector_2 , x = vector_1)##  [1]  97 100 129  86  83 127  94 120  91 134mapply(FUN = **function**(x , y) x-y  , y =  vector_1 , x = vector_2)##  [1]  -95  -96 -123  -78  -73 -115  -80 -104  -73 -114
```

# 6.塔普利()

首先，tapply()不能处理列表。它是 apply-family 函数中一个强大而重要的成员，我们用它来将函数应用于向量或数组。我们传递一个相同长度的向量，它代表一个类别因子，我们用它来分解实际的数据，并在其上运行我们的函数。它的工作方式有点像 base R 中的 by()函数，即使传递给 INDEX 参数的向量不是 factor，它也会被强制转换成 factor。看一下下面的例子。

```
tapply(X = vector_1 , INDEX = rep(c('a' , 'b') , 5) , FUN = sum)##   a   b 
## 469 537tapply(X = vector_1 , INDEX = rep(c('a' , 'b') , 5) , FUN = mean)##     a     b 
##  93.8 107.4tapply(X = vector_2 , INDEX = rep(c('a' , 'b') , 5) , FUN = sum)##  a  b 
## 25 30tapply(X = vector_2 , INDEX = rep(c('a' , 'b') , 5) , FUN = mean)## a b 
## 5 6example_matrix <- matrix(data = round(rnorm(n = 25 , mean = 100 , sd = 20)) , nrow = 5 , ncol = 5)tapply(X = example_matrix , INDEX = rep(c('a' , 'b' , 'a' , 'c' , 'b') , 5) , FUN = sum)##    a    b    c 
## 1013 1098  428tapply(X = example_matrix , 
       INDEX = sample(
         x = c('Michael' , 'Jim' , 'Pam' , 'Dwight' , 'Oscar') , 
         size = 25 , 
         prob = c(0.3 , 0.15 , 0.15 , 0.25 , 0.15) , 
         replace = TRUE
         ) , 
       FUN = sum)##  Dwight     Jim Michael     Pam 
##     537     599    1105     298
```

# 7.拉普利()

我们到了。我们正面临着应用程序家族中最棘手的成员。一开始看起来可能像恐怖电影，但不要担心。我们会一起克服它。rapply()函数，代表递归应用，允许我们迭代嵌套结构。这些嵌套结构可能是列表的元素、向量的整数或数据帧的列。更准确地说，它在元素的元素上运行。how 参数是 rapply()的特别之处之一。我们将立刻用例子来巩固我们的描述。

让我们找出这些 data.frame 对象中每个数字和整数列的平均值，通过删除丢失的值将这些对象存储为列表元素。

```
list_of_df <- list(mtcars , airquality , iris)
rapply(object = list_of_df , f = mean , classes = 'numeric' , na.rm = TRUE)##          mpg          cyl         disp           hp         drat           wt 
##    20.090625     6.187500   230.721875   146.687500     3.596563     3.217250 
##         qsec           vs           am         gear         carb         Wind 
##    17.848750     0.437500     0.406250     3.687500     2.812500     9.957516 
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
##     5.843333     3.057333     3.758000     1.199333rapply(object = list_of_df , f = mean , classes = 'integer' , na.rm = TRUE)##      Ozone    Solar.R       Temp      Month        Day 
##  42.129310 185.931507  77.882353   6.993464  15.803922
```

这是展示 rapply()功能的另一种方式。

```
temp <- list(a = c(1, 2, 3), 
             b = list(a = c(4, 5, 6), b = c(7, 8, 9)),
             c = list(a = c(10, 11, 12), b = c(13, 14, 15)))rapply(temp, sum)##   a b.a b.b c.a c.b 
##   6  15  24  33  42
```

how 参数指定 rapply()如何返回输出。该参数有三个选项:“列表”、“取消列表”和“替换”。让我们举例说明。

```
ls <- list(a = 1:5, b = 100:110, c = c('a', 'b', 'c')) rapply(ls, mean, how = "replace", classes = "integer")## $a
## [1] 3
## 
## $b
## [1] 105
## 
## $c
## [1] "a" "b" "c"rapply(ls, mean, how = "list", classes = "integer")## $a
## [1] 3
## 
## $b
## [1] 105
## 
## $c
## NULLrapply(ls, mean, how = "unlist", classes = "integer")##   a   b 
##   3 105
```

# 8.eapply()

我们的最后一个函数是 eapply()，代表环境应用。它非常简单，在许多情况下都能派上用场。它基本上将一个函数应用于环境中的每个命名元素。我想知道使用 eapply()进行通用脚本编写或数据分析的频率。个人觉得是 apply-family 里我用的最少的一款。当你真正开发自己的图书馆时，它会释放出它真正的力量。让我们举几个例子。

```
env <- new.env(hash = FALSE) 
env$a <- 1:10
env$beta <- exp(-3:3)
env$logic <- c(TRUE, FALSE, FALSE, TRUE)eapply(env, mean)## $logic
## [1] 0.5
## 
## $beta
## [1] 4.535125
## 
## $a
## [1] 5.5eapply(env, quantile)## $logic
##   0%  25%  50%  75% 100% 
##  0.0  0.0  0.5  1.0  1.0 
## 
## $beta
##          0%         25%         50%         75%        100% 
##  0.04978707  0.25160736  1.00000000  5.05366896 20.08553692 
## 
## $a
##    0%   25%   50%   75%  100% 
##  1.00  3.25  5.50  7.75 10.00e <- new.env()e$a <- 10
e$b <- 20
e$c <- 30eapply(e, **function**(x) x * 2)## $a
## [1] 20
## 
## $b
## [1] 40
## 
## $c
## [1] 60
```