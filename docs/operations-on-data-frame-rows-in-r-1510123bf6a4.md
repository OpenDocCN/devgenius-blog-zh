# R 中 data.frame 行的操作

> 原文：<https://blog.devgenius.io/operations-on-data-frame-rows-in-r-1510123bf6a4?source=collection_archive---------18----------------------->

考虑我们有一个下面的例子( **losses_ru** 是一个代表俄罗斯军队在乌克兰每月损失的数据帧；原 csv 文件已从 http://uadata.net[下载。任务是计算每种武器类型或部队的总损失。](https://uadata.net/)

```
> losses_ru
            item  Feb   Mar  Apr  May  Jun  Jul  Aug   Sep   Oct   Nov  Dec
1         troops 5300 12200 5700 7300 5100 5230 7070 11180 12740 17060 2810
2           cars  291   910  500  574  327  300  334   532   360   301   68
3      artillery    0   311  125  213  141  126  175   300   337   174   12
4     armored PC  816   919  710  857  424  278  308   620   553   387   28
5            UAV    3    80  149  283  126   94  114   154   410   149   20
6    helicopters   29   102   24   19   11    5   14    21    28     8    3
7          ships    2     5    1    5    1    1    0     0     1     0    0
8         planes   29   106   55   18    9    6   11    30    11     5    1
9  anti-aircraft    5    49   23   16   11   13   35    24    21    13    1
10          MLRS    0    96   55   56   39   13   26    48    50    12    0
11         tanks  191   423  394  350  215  190  211   364   348   228   10
```

Base R 包提供了 **rowSums()** 函数来对一行中的值求和。(同样，函数 **rowMeans()** 将计算平均值)。从数据帧**loss _ ru**中删除第一列(" **item** ")将有助于获得矢量形式的总和:

```
> rowSums(losses_ru[,-1])
 [1] 91690  4497  1914  5900  1582   264    16   281   211   395  2924
```

该向量可通过 **cbind()** 或 **data.frame()** 函数添加到数据帧中，使其具有信息性:

```
> cbind(losses_ru, total=rowSums(losses_ru[,-1]))
            item  Feb   Mar  Apr  May  Jun  Jul  Aug   Sep   Oct   Nov  Dec total
1         troops 5300 12200 5700 7300 5100 5230 7070 11180 12740 17060 2810 91690
2           cars  291   910  500  574  327  300  334   532   360   301   68  4497
3      artillery    0   311  125  213  141  126  175   300   337   174   12  1914
4     armored PC  816   919  710  857  424  278  308   620   553   387   28  5900
5            UAV    3    80  149  283  126   94  114   154   410   149   20  1582
6    helicopters   29   102   24   19   11    5   14    21    28     8    3   264
7          ships    2     5    1    5    1    1    0     0     1     0    0    16
8         planes   29   106   55   18    9    6   11    30    11     5    1   281
9  anti-aircraft    5    49   23   16   11   13   35    24    21    13    1   211
10          MLRS    0    96   55   56   39   13   26    48    50    12    0   395
11         tanks  191   423  394  350  215  190  211   364   348   228   10  2924
```

现在， **dplyr** package 擅长使用 **mutate** 动词创建新列。事实上，stackoverflow 甚至有一个[问题](https://stackoverflow.com/questions/27354734/dplyr-mutate-rowsums-calculations-or-custom-functions)来回答这个有 6 个答案的问题。

另一个简单的解决方法是从包**看门人**中**点缀 _ 总计()**:

```
> losses_ru %>% adorn_totals("col")
          item  Feb   Mar  Apr  May  Jun  Jul  Aug   Sep   Oct   Nov  Dec Total
        troops 5300 12200 5700 7300 5100 5230 7070 11180 12740 17060 2810 91690
          cars  291   910  500  574  327  300  334   532   360   301   68  4497
     artillery    0   311  125  213  141  126  175   300   337   174   12  1914
    armored PC  816   919  710  857  424  278  308   620   553   387   28  5900
           UAV    3    80  149  283  126   94  114   154   410   149   20  1582
   helicopters   29   102   24   19   11    5   14    21    28     8    3   264
         ships    2     5    1    5    1    1    0     0     1     0    0    16
        planes   29   106   55   18    9    6   11    30    11     5    1   281
 anti-aircraft    5    49   23   16   11   13   35    24    21    13    1   211
          MLRS    0    96   55   56   39   13   26    48    50    12    0   395
         tanks  191   423  394  350  215  190  211   364   348   228   10  2924
```

但是如果我们需要比一行的总和或平均值更多的东西呢？实际上， **dplyr** 处理群体的方式很奇妙。首先，让我们用 **pivot_longer()** 合并列，并用 **group_by()** 为每个项目分组。因此，我们可以用 **mutate(** )动词对单个列(默认 pivot_longer 创建 **name** 和 **value** 列)进行操作。我们将创建所需的新列，取消分组并旋转回宽格式:

```
>   losses_ru %>% 
+     group_by(item) %>%
+     pivot_longer(-item) %>% 
+     mutate(mean = round(mean(value), 0),
+            max = max(value),
+            median = median(value)) %>%
+     ungroup() %>% 
+     pivot_wider(names_from = "name", values_from = "value") %>%
as.data.frame()
            item mean   max median  Feb   Mar  Apr  May  Jun  Jul  Aug   Sep   Oct   Nov  Dec
1         troops 8335 17060   7070 5300 12200 5700 7300 5100 5230 7070 11180 12740 17060 2810
2           cars  409   910    334  291   910  500  574  327  300  334   532   360   301   68
3      artillery  174   337    174    0   311  125  213  141  126  175   300   337   174   12
4     armored PC  536   919    553  816   919  710  857  424  278  308   620   553   387   28
5            UAV  144   410    126    3    80  149  283  126   94  114   154   410   149   20
6    helicopters   24   102     19   29   102   24   19   11    5   14    21    28     8    3
7          ships    1     5      1    2     5    1    5    1    1    0     0     1     0    0
8         planes   26   106     11   29   106   55   18    9    6   11    30    11     5    1
9  anti-aircraft   19    49     16    5    49   23   16   11   13   35    24    21    13    1
10          MLRS   36    96     39    0    96   55   56   39   13   26    48    50    12    0
11         tanks  266   423    228  191   423  394  350  215  190  211   364   348   228   10
```

注意，在没有唯一标识行的情况下，就像上面显示的那样，我们必须添加一个 rowid 列。