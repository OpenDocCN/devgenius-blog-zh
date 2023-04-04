# 以当前连续作为递归返回的调用

> 原文：<https://blog.devgenius.io/call-with-current-continuation-as-recursive-return-ac58244c445?source=collection_archive---------11----------------------->

## 在方案中使用 Call/CC 从递归定义早期“返回”

![](img/3a2d565f9cd6317c1192fd0eee830749.png)

## 什么是延续

当一个函数调用另一个函数时，它会等待一个返回值来做一些事情。例如:表达式`(+ 1 (f x))`，`(+ 1 _)`是执行`(f x)`的延续。继续就是要处理的事情和等待`(f x)`返回值的事情。

## 呼叫/抄送

call/cc 获取一个带有单个参数的 lambda 表达式，并将当前的延续传递给该函数作为其参数。

```
(+ 1 (call/cc 
       (lambda (cont)
         (cont 5))))
```

这段代码相当简单，但是我们可以看到当前的延续`( + 1 _)` 被传递给λ表达式作为`cont`。

`cont`本身就是一个 lambda 表达式，如果我们用一个参数调用它，它将把这个参数作为延续的参数，并执行延续。我们可以像从函数返回一样使用它。我们可以直接将一个值传递给函数的延续，而不是等待函数正常退出。

在上例中，我们只需将 5 传递给`cont`。`( + 1 5)`然后被执行，这个片段以值 6 退出。

让我们看看另外两个代码片段

```
(+ 1 (call/cc 
       (lambda (cont)
         (+ 3 3))))
```

和

```
(+ 1 (call/cc 
       (lambda (cont)
         (cont 5)
         (+ 3 3))))
```

第一个代码片段简单地以值 7 退出，不做任何延续。然而，当执行`cont 5`时，第二个直接传递 5 到λ的延续。因此，这个 lambda 将永远不会实际执行`(+ 3 3)`，并且代码片段将以值 6 退出。在这种情况下，`cont 5`类似于另一种语言中的`**return** 5`。我们已经很早就有效地从功能中恢复了。

延续有多种用途，但一个有趣的用途是递归函数的早期返回。

## 列表乘法元素的应用

首先定义一个函数`multListNoCC`，该函数获取一个数字列表，并将它们递归相乘。

```
(define multListNoCC 
  (lambda (lat)
    (cond
      ((null? lat) 1)
      (#t (* (car lat) (multListNoCC (cdr lat)))))))
```

然而，这个算法可以变得更有效。如果列表中的任何元素是零，我们知道最终的乘积必须是零。因此，我们可以停止乘以元素，只返回零作为乘积，而不是继续遍历列表。然而，如果我们在函数中添加一个 case 来返回零，那么零将会被传递回递归堆栈。我们可以使用 call/cc 来“弹出”所有的递归调用，并向原始调用方返回 0。例如:

```
(define multList 
  (lambda (lat)
    (call/cc (lambda (cont)
      (letrec ((helper (lambda (x)
        (cond
          ((null? x) 1)
          ((eq? (car x) 0) (cont 0))
          (#t (* (car x) (helper (cdr x))))))))
        (helper lat))))))
```

在`multList`功能中，`helper`与`multListNoCC`几乎完全相同。我们将 helper 封装在几个 lambdas 中，让它能够访问调用该函数的原始 CC 以及我们想要相乘的列表。然后，我们可以向 helper 添加另一个条件:当当前元素等于零时，立即停止递归，并向原始延续传递一个零。即:`((eq? (car x) 0) (cont 0))`
`cont`是递归开始前的 CC——最初的调用者，因此向`cont`传递一个值实际上停止了递归并直接返回一个值。

如果从未遇到零，函数将通过沿递归堆栈向上传递值来正常返回。

## 更多示例和应用

```
(define isNumInList
  (lambda (lat num)
    (call/cc (lambda (cont)
      (letrec ((helper (lambda (l n)
        (cond
          ((null? l) #f)
          ((eq? (car l) n) (cont #t))
          (#t (helper (cdr l) n ))))))
        (helper lat num))))))
```

如果我们已经找到了数字的一个实例，就没有必要继续遍历了。
如果我们一直到列表的末尾，还没有找到元素，那么一直向上传递一个#f，表示我们还没有找到这个数字。

```
(define findFirstNumInList
  (lambda (lat num)
    (call/cc (lambda (cont)
      (letrec ((helper (lambda (l n index)
        (cond
          ((null? l) -1)
          ((eq? (car l) n) (cont index))
          (#t (helper (cdr l) n (+ index 1)))))))
          (helper lat num 0))))))
```

很像 isNumInList，但是返回找到的元素的索引(如果元素不在列表中，则返回-1。

## 关于作者

Eric Breyer 是莱斯大学的计算机科学本科生。你可以在他的[网站](http://www.ericbreyer.com/)，以及 [GitHub](https://github.com/ericbreyer) 和 [LinkedIn](https://www.linkedin.com/in/eric-breyer/) 上找到他。