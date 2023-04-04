# 将数据分组，第 1 部分

> 原文：<https://blog.devgenius.io/clustering-data-into-groups-part-1-2a438c46de95?source=collection_archive---------19----------------------->

## 文章

## *来自* [*数据科学图书营*](https://www.manning.com/books/data-science-bookcamp?utm_source=medium&utm_medium=organic&utm_campaign=book_apeltsin_data_9_6_19) *作者 Leonard Apeltsin*

*这个由 3 部分组成的文章系列包括:*

*   *通过中心性对数据进行聚类*
*   *按密度聚类数据*
*   *聚类算法之间的权衡*
*   *使用 scikit-learn 库执行聚类*
*   *使用熊猫遍历集群*

在[manning.com](https://www.manning.com/?utm_source=medium&utm_medium=organic&utm_campaign=book_apeltsin_data_9_6_19)的结账处，将 **fccapeltsin** 输入折扣代码框，即可享受 [*数据科学图书营*](https://www.manning.com/books/data-science-bookcamp?utm_source=medium&utm_medium=organic&utm_campaign=book_apeltsin_data_9_6_19)35%的折扣。

*聚类*是将数据点组织成概念上有意义的组的过程。是什么让一个给定的群体“概念上有意义”？这个问题没有简单的答案。任何集群输出的有用性都取决于我们被分配的任务。

想象一下，我们被要求对一组宠物照片进行分类。我们是否将鱼和蜥蜴归为一类，而将毛绒绒的宠物(如仓鼠、猫和狗)归为另一类？还是应该给仓鼠、猫和狗分配三个独立的类别？如果是这样，也许我们应该考虑将宠物按品种分类。因此，吉娃娃和大丹狗分成不同的群体。区分不同品种的狗并不容易。然而，我们可以很容易地根据品种大小来区分吉娃娃和大丹犬。也许我们应该妥协:我们将根据蓬松度和大小来分组，从而绕过凯恩梗和长相相似的诺威奇梗之间的区别。

妥协值得吗？这取决于我们的数据科学任务。假设我们为一家宠物食品公司工作，我们的目标是估计狗粮、猫粮和蜥蜴粮的需求。在这种情况下，我们必须区分毛茸茸的狗、毛茸茸的猫和有鳞的蜥蜴。然而，我们不需要解决不同犬种之间的差异。或者，想象一下一个兽医办公室的分析师正试图按品种对宠物患者进行分组。第二个任务需要更精细的组解析。

不同的情况取决于不同的聚类技术。作为数据科学家，我们必须选择正确的集群解决方案。在我们的职业生涯中，我们将使用各种聚类技术对数千个(如果不是数万个)数据集进行聚类。最常用的算法依赖于某种中心性概念来区分聚类。

## **利用中心性发现集群**

数据的中心性可以用平均值来表示。假设你已经计算了两组鱼的平均长度，并通过分析它们平均值之间的差异来比较它们。我们可以利用这种差异来确定是否所有的鱼都属于同一组。直觉上，一个组中的所有数据点应该围绕一个中心值聚集。同时，两个不同组中的测量值应该集中在两个不同的平均值上。因此，我们可以利用中心性来区分两个不同的群体。让我们具体探讨一下这个概念。

假设我们去当地一家热闹的酒吧实地考察，看到两个镖靶并排挂着。每块镖靶上都布满了飞镖，飞镖也从墙上突出来。酒吧里喝得醉醺醺的玩家瞄准了其中一方的靶心。他们经常失手，这导致观察到飞镖分散在两个靶心周围。

让我们用数值模拟散射。我们将把每个靶心位置视为一个 2D 坐标。飞镖被随机扔向那个坐标。因此，省道的 2D 位置是随机分布的。模拟省道位置最合适的分布是正态分布，原因如下:

*   典型的飞镖运动员瞄准靶心，而不是靶的边缘。因此，每个飞镖更有可能击中接近棋盘中心的地方。这种行为与随机正态样本一致，在随机正态样本中，更接近平均值的值比其他更远的值出现得更频繁。
*   我们希望飞镖相对于中心对称地击中棋盘。飞镖将以相同的频率击中中心左侧 3 英寸和中心右侧 3 英寸。这种对称性被钟形正态曲线所捕捉。

假设第一个靶心位于坐标`[0, 0]`。向该坐标投掷飞镖。我们将使用两个正态分布来模拟镖的 x 和 y 位置。这些分布的平均值为 0，我们还假设它们的方差为 2。以下代码生成镖的随机坐标。

**清单 1。使用两种正态分布模拟省道坐标**

```
import numpy as np
 np.random.seed(0)
 mean = 0
 variance = 2
 x = np.random.normal(mean, variance ** 0.5)
 y = np.random.normal(mean, variance ** 0.5)
 print(f"The x coordinate of a randomly thrown dart is {x:.2f}")
 print(f"The y coordinate of a randomly thrown dart is {y:.2f}")
 The x coordinate of a randomly thrown dart is 2.49
 The y coordinate of a randomly thrown dart is 0.57
```

我们可以使用`np.random.multivariate_normal`方法更有效地模拟省道位置。该方法从*多元正态分布中选择一个随机点。*多元正态曲线就是延伸到多个维度的正态曲线。我们的 2D 多元正态分布将类似一座圆山，其顶点位于`[0, 0]`。

让我们模拟向`[0, 0]`的靶心随机投掷 5000 个飞镖。我们还模拟在第二个靶心(位于`[0, 6]`)投掷 5000 个随机飞镖。然后，我们生成所有随机 dart 坐标的散点图(图 10.1)。

**清单 2。模拟随机投掷飞镖**

```
import matplotlib.pyplot as plt
 np.random.seed(1)
 bulls_eye1 = [0, 0]
 bulls_eye2 = [6, 0]
 bulls_eyes = [bulls_eye1, bulls_eye2]
 x_coordinates, y_coordinates = [], []
 for bulls_eye in bulls_eyes:
     for _ in range(5000):
         x = np.random.normal(bulls_eye[0], variance ** 0.5)
         y = np.random.normal(bulls_eye[1], variance ** 0.5)
         x_coordinates.append(x)
         y_coordinates.append(y)

 plt.scatter(x_coordinates, y_coordinates)
 plt.show()
```

清单 2 包含一个嵌套的五行`for`循环，以`for _ in range(5000)`开始。只用一行代码就可以使用 NumPy 来执行这个循环:运行`x_coordinates, y_coordinates = np.random.multivariate_normal(bulls_eye, np.diag(2 * [variance]), 5000).T`返回从多元正态分布中采样的 5000 个 x 和 y 坐标。

![](img/d13f5256e65f81f322f0d2a643d43908.png)

图一。模拟飞镖随机散布在两个靶心周围

图中出现两个重叠的 dart 组。两组代表一万个飞镖。一半的飞镖瞄准了左边的靶心，其余的瞄准了右边。每个镖都有一个预定的目标，我们可以通过查看图来估计。更靠近`[0, 0]`的飞镖很可能瞄准了左边的靶心。我们将把这个假设结合到我们的飞镖图中。

让我们把每个镖分配到它最近的靶心。我们首先定义一个`nearest_bulls_eye`函数，该函数将保存 dart 的 x 和 y 位置的`dart`列表作为输入。该函数返回最接近`dart`的靶心的索引。我们使用*欧几里德距离*来测量省道的接近度，欧几里德距离是两点之间的标准直线距离。

欧几里得距离源于勾股定理。假设我们相对于位置`[x_bull, y_bull]`处的靶心来检查位置`[x_dart, y_dart]`处的镖。根据勾股定理，`distance` 2 `= (x_dart - x_bull)` 2 `+ (y_dart - y_bull)` 2。我们可以使用自定义的欧几里德函数来求解距离。或者，我们可以使用 SciPy 提供的`scipy.spatial.distance.euclidean`函数。

下面的代码定义了`nearest_bulls_eye`，并将其应用于飞镖`[0, 1]`和`[6, 1]`。

**清单 3。将飞镖分配到最近的靶心**

```
from scipy.spatial.distance import euclidean
 def nearest_bulls_eye(dart):
     distances = [euclidean(dart, bulls_e) for bulls_e in bulls_eyes] ❶
     return np.argmin(distances) ❷

 darts = [[0,1], [6, 1]]
 for dart in darts:
     index = nearest_bulls_eye(dart)
     print(f"The dart at position {dart} is closest to bulls-eye {index}")
```

❶ **使用从 SciPy** 导入的欧几里德函数获得飞镖和每个靶心之间的欧几里德距离

❷ **返回与距离**中最短靶心距离匹配的索引

现在我们将`nearest_bulls_eye`应用于所有计算出的飞镖坐标。每个省道都用两种颜色中的一种绘制，以区别两种靶心分配(图 10.2)。

**清单 4。根据最近的靶心给飞镖着色**

```
def color_by_cluster(darts): ❶
     nearest_bulls_eyes = [nearest_bulls_eye(dart) for dart in darts]
     for bs_index in range(len(bulls_eyes)):
         selected_darts = [darts[i] for i in range(len(darts))
                           if bs_index == nearest_bulls_eyes[i]] ❷
         x_coordinates, y_coordinates = np.array(selected_darts).T ❸
         plt.scatter(x_coordinates, y_coordinates,
                     color=['g', 'k'][bs_index])
     plt.show()

 darts = [[x_coordinates[i], y_coordinates[i]]
          for i in range(len(x_coordinates))] ❹
 color_by_cluster(darts)
```

❶ **辅助函数，绘制输入飞镖列表的彩色元素。飞镖游戏中的每个飞镖都作为最近靶心的输入。**

❷ **选择最接近靶心的飞镖【bs _ index】**

❸ **通过转置所选飞镖的数组来分离每个飞镖的 x 和 y 坐标。转置用 2D 数据结构交换行和列的位置。**

❹ **将每个镖的独立坐标组合成一个 x 和 y 坐标列表。**

![](img/d06c9752ca1f4dbc08a36be239fe4272.png)

图二。飞镖根据与最近靶心的接近程度进行着色。聚类 A 表示最靠近左靶心的所有点，聚类 B 表示最靠近右靶心的所有点。

彩色飞镖明显分成两个均匀的簇。如果不提供中心坐标，我们如何识别这样的星团？一个原始的策略是简单地猜测靶心的位置。我们可以随机选择两个飞镖，并希望这些飞镖以某种方式相对靠近每个靶心，尽管这种情况发生的可能性非常低。在大多数情况下，根据两个随机选择的中心来给飞镖着色不会产生好的结果(图 3)。

**清单 5。给随机选择的中心分配飞镖**

```
bulls_eyes = np.array(darts[:2]) ❶
 color_by_cluster(darts)
```

❶ **随机选择前两个飞镖作为我们的代表性靶心**

![](img/d9ef8bd59a7e5fab72834ddc7e35e14c.png)

图 3。根据与随机选择的中心的接近程度给飞镖上色。集群 B 向左延伸得太远。

我们不加选择的中心感觉质量上是错误的。例如，右边的星团 B 似乎向左延伸得太远了。我们指定的任意中心似乎与它实际的靶心点不匹配。但是有一种方法可以弥补我们的错误:我们可以计算拉伸的右聚类组中所有点的平均坐标，然后利用这些坐标来调整我们对组中心的估计。在将聚类的平均坐标分配给靶心之后，我们可以重新应用基于距离的分组技术来调整最右边聚类的边界。事实上，为了最大限度地提高效率，在重新运行基于中心性的聚类之前，我们还会将最左边的聚类中心重置为其平均值(图 4)。

当我们计算 1D 数组的平均值时，我们返回一个值。我们现在将这个定义扩展到多维度。当我们计算 2D 数组的平均值时，我们返回所有 x 坐标的平均值和所有 y 坐标的平均值。最终输出是一个包含 x 轴和 y 轴平均值的 2D 数组。

**清单 6。根据平均值将飞镖分配到中心**

```
def update_bulls_eyes(darts):
     updated_bulls_eyes = []
     nearest_bulls_eyes = [nearest_bulls_eye(dart) for dart in darts]
     for bs_index in range(len(bulls_eyes)):
         selected_darts = [darts[i] for i in range(len(darts))
                           if bs_index == nearest_bulls_eyes[i]]
         x_coordinates, y_coordinates = np.array(selected_darts).T
         mean_center = [np.mean(x_coordinates), np.mean(y_coordinates)] ❶
         updated_bulls_eyes.append(mean_center)

     return updated_bulls_eyes

 bulls_eyes = update_bulls_eyes(darts)
 color_by_cluster(darts)
```

❶ **取分配给给定靶心的所有飞镖的 x 和 y 坐标的平均值。这些平均坐标然后被用来更新我们估计的靶心位置。我们可以通过执行 np.mean(selected_darts，axis=0)来更高效地运行这个计算。**

![](img/0f4bb64c4d9127e0c5b13cfd49fa8f91.png)

图 4。根据与重新计算的中心的接近程度对飞镖进行着色。这两个集群现在看起来更加均匀。

结果已经看起来更好了，尽管它们并没有达到预期的效果。星团的中心看起来仍然有些偏离。让我们通过在 10 次额外迭代中重复基于平均值的中心性调整来补救结果(图 5)。

**清单 7。经过 10 次迭代调整靶心位置**

```
for i in range(10):
     bulls_eyes = update_bulls_eyes(darts)

 color_by_cluster(darts)
```

![](img/40712ec7b4f873ce58b69617c9b4cf55.png)

图 5。基于与迭代重新计算的中心的接近度对飞镖进行着色

这两套飞镖现在完美地组合在一起了！我们基本上复制了 K 均值聚类算法，它使用中心性来组织数据。

点击查看[第 2 部分。感谢阅读。](https://manningbooks.medium.com/clustering-data-into-groups-part-2-3f7e1d25e67e)