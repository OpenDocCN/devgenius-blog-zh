# 遗传算法:应用进化

> 原文：<https://blog.devgenius.io/genetic-algorithms-applied-evolution-1c3afa0f7f3f?source=collection_archive---------14----------------------->

![](img/d9457e240626608b9c53097624eaef7b.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Sangharsh Lohakare](https://unsplash.com/@sangharsh_l?utm_source=medium&utm_medium=referral) 拍摄的照片

上次我们开始了我们的[人工智能](/entering-the-artificial-intelligence-maze-d58fb6eb5554)之旅，通过观察[生物进化](/genetic-algorithms-applied-evolution-1c3afa0f7f3f)以及一个程序将如何实现它。由于我们只对理论感兴趣，我们的算法可能会偏离，并疯狂地转向我们想象中的任何形式。但是没关系:毕竟进化只是一种优化策略。我们如何实现以及出于什么目的并不重要。如果我们正在寻找一个最优的解决方案，并且我们知道如何判断一个结果，我们可以迭代数千代，以最小的 CPU 成本找到我们需要的东西。

但是现在让我们关注一种特殊的进化:基因进化，这种进化让我们像软件开发者一样接近生物学。上次我们考虑的解决方案是一个随机对象，比如一只狗，它有一些狗的属性，比如年龄、品种、速度等等。我们通过对这些属性进行假想的相加来计算狗的适应度:我们并不关心它到底是如何做到的，这只是理论。遗传算法更严格。遗传算法群体中的解被称为个体，它是一组称为基因的数字。这就像代表一个人和他的 DNA。

我用 Rust 写了一个简单的例子，并在我的 [Github](https://github.com/raduzaharia-medium/genetic-algorithm-simple) 上发布。这里的例子只是检查解向量的值是否都为零。您可以随意克隆存储库，但是我强烈建议您自己制作算法。试着按照这篇文章去做。这将是一个更有意义的过程。

## 赋予基因意义

![](img/0de6861bd3e09bb3ea9572e0b0f70668.png)

[Rodrigo Abreu](https://unsplash.com/@rodrigospabreu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

因此，如果我们局限于数字数组，我们可能会给它们起个奇怪的名字，但我们如何解决任何有用的问题呢？让我们考虑一个配送问题:我们有一辆卡车，它需要经过几个地点来运送货物。我们需要创建一条路线，以最佳方式穿过所有位置:每个位置一次，并且位置之间的距离最小。我们的解决方案会是什么样的？

```
let individual = [0, 1, 2, 3, 4, 5];
```

上面我们把一个个体定义为一系列基因。这些基因代表了卡车下一步要去的地方。在上面的例子中，卡车将按顺序进入所有六个位置。基因是位置，个体是一个完整的时间表，一个解决方案。在这种情况下，拥有大量这样的解决方案看起来像这样:

```
let population = [ 
  [0, 1, 2, 3, 4, 5], 
  [1, 3, 5, 0, 2, 4],
  [2, 5, 0, 3, 1, 4]];
```

上述群体有三个人，他们每个人都是我们问题的解决方案。我们希望找到最快的路线，即花费较少里程的路线，因此我们需要定义每个位置之间的距离:

```
let distances = [
  [0, 15, 7, 25, 33, 11],  // location 0
  [9, 0, 24, 30, 15, 10],  // location 1
  [11, 25, 0, 18, 10, 10], // location 2
  [33, 66, 13, 0, 95, 50], // location 3
  [14, 15, 11, 9, 0, 10],  // location 4
  [10, 10, 22, 25, 30, 0]] // location 5
```

距离矩阵给出了每个位置的距离值。如果我们想从位置 0 到位置 2，距离是 7。如果我们想从位置 2 到位置 4，距离是 10。从位置 0 到位置 0 的距离当然是 0。我们也可以通过将距离设置为无穷大来创建 no go 路线。例如，如果在位置 1 和 4 之间没有路线，我们就用`Infinity`代替 15。

## 计算适应度

![](img/27ab36dd351b8b82fbfc335cf82cc416.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[图形节点](https://unsplash.com/@graphicnode?utm_source=medium&utm_medium=referral)拍照

使用这种配置，计算解决方案的适合度很容易:

```
function fitness(solution, distances) {
  let result = 
    distances[solution[0]][solution[1]] +
    distances[solution[1]][solution[2]] +
    distances[solution[2]][solution[3]] +
    distances[solution[3]][solution[4]] +
    distances[solution[4]][solution[5]];
  return result;
}
```

路线从一个位置到另一个位置，由`solution`向量定义。因此，我们需要做的只是简单地按照这些位置在`solution`中出现的顺序添加它们之间的距离。因此，我们首先从`solution[0]`中定义的位置前往`solution[1]`。为了得到距离，我们查看`distances`矩阵，取对应于`solution[0]`的第一行和对应于`solution[1]`的列。一旦我们理解了算法，我们就可以使它通用化:

```
function fitness(solution, distances) {
  let result = 0;    for (let i = 0; i < solution.length() - 1; i++) 
    for (let j = 1; j < solution.length(); j++)
      result += distances[solution[i]][solution[j]]; return result;
}
```

上述函数将为我们提供卡车在给定路线或解决方案中行驶的总距离的原始数字。但如果我们在寻找最短路径，有一个漏洞遗传算法会很快发现:通过上述适应度函数，解决方案`[1, 1, 1, 1, 1, 1]`具有完美的适应度，零。那么，我们该怎么做才能迫使系统进化出总是去某个地方而不是静止不动的路线呢？我们唯一能做的是:在适应度函数中惩罚错误的路线:

```
function fitness(solution, distances) {
  let result = 0; for (let i = 0; i < solution.length() - 1; i++) 
    for (let j = 1; j < solution.length(); j++)
      result += distances[solution[i]][solution[j]]; for (let i = 0; i < solution.length(); i++)
    if (!result.includes(i))
      result += 1000; return result;
}
```

在上面更新的适应度函数中，我们取所有可用的位置，并检查它们是否在`solution`向量中的某处。如有任何一项缺失，`result`将被扣 1000 分。这意味着，在努力最小化群体中的适应度的同时，算法将倾向于包含所有位置的解决方案。否则，解决方案将受到严重的惩罚，地点之间的距离将不再重要。这些解决方案将很快被抛弃。

## 交叉和变异

![](img/96b19acc509ba298543d3cb16b74da87.png)

照片由[飞:D](https://unsplash.com/@flyd2069?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

遗传算法也有经典的进化操作:父母的混合在这里被称为交叉，因为我们选择两个个体并交叉他们的基因来创造孩子，然后我们有突变。因为我们总是谈论相同的表示，一个称为基因的数字向量，这两个操作可以完全推广到任何问题，任何遗传算法。我们将进化操作从问题中完全分离和抽象出来:

```
function crossover(parent1, parent2) {
  let index = Math.random() * parent1.length();
  let part = parent2.slice(index);
  let child = parent1; child.splice(index, child.length() - index, ...part);
  return child;
}function mutation(child, randomGeneProvider) {
  let result = child;for (let i = 0; i < result.length(); i++)
    if (Math.random < 0.3) result[i] = randomGeneProvider(); return result;
}
```

在`crossover`函数中，我们选择了一个名为`index`的随机分割点，我们将第一个父节点完全复制到子节点中，然后将第二个父节点从索引拼接到末尾。基本上如果我们有两个父母`[0, 1, 2, 3, 4, 5]`和`[3, 5, 1, 4, 0, 2]`，如果分裂指数是 4，那么孩子就是`[0, 1, 2, 3, 0, 2]`。请注意，交叉和变异函数并不太关心结果子代。在这种情况下，孩子提供了一个解决方案，使卡车在位置 0 和 2 行驶两次。但同样，这不是交叉或变异的责任，它完全与适应度函数有关。

在变异函数中接收到一个回调`randomGeneProvider`。它只是一个函数，给我一个对我的问题有意义的数字:在这个例子中是一个 0 到 5 之间的随机数。在这里，我将基因提供者作为一个参数，这样`mutation`函数对于任何问题都是通用的。

## 考虑不同的问题

![](img/bc0c36a04c7203a21f889f90df7a06be.png)

照片由[粘土堤](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我略读了一下解释，因为所有这些正是我们上次讨论进化算法的内容。我们唯一改变的是遗传算法的求解格式，它是一个数字向量。如果你能把问题的解表示成一个数字向量，你就能用遗传算法来解决它。

所以在我们结束这篇文章之前，让我们考虑几个其他的问题，以及如何用一种适合遗传算法的格式来表示它们。例如多项式回归。我们想找到一个多项式，一个形式为`1+x+x²+x³+…=0`的方程，它通过一组给定的点。我们知道点，我们想要方程。对于这个问题，一个解决方案可以巧妙地放在一个向量中:`[1, 6, 7, 8, 3]`表示`1+6*x+7*x²+8*x³+3*x⁴=0`。这个解决方案的适合性意味着我们在这个函数上运行我们的数据库中的所有点，并且添加差异。这些点是 x 的每个次方的值，差就是我们的结果减去正确的结果。把所有的差异加在一起，你就得到解决方案的总适合度。

另一个例子:在背包里装满不同重量的物品。解决方案可以是一个装满的背包，一个包含代表我们在背包中添加的对象的数字的向量。健身功能是将背包解决方案中的所有重量相加，然后减去背包所能容纳的最大重量的结果。例如，我们还需要惩罚两次拥有相同对象的解决方案，或者不包含某些首选对象的解决方案。

不一样的一个:学校课程表。这次的解决方案是一个矩阵，有一门课的整个时间表，例如从周一到周五，从早上 8 点到下午 3 点的每个小时。健身会检查所有的课程都在课程表中，没有两个课程重叠，并且每个课程每周都有正确的小时数。

那么这一切意味着什么呢？简而言之，当我们实现遗传算法时，我们正在创建一个智能搜索引擎。搜索的空间是我们问题的领域，所有可能存在的解决方案，正确的和不正确的。我们对适应度函数所做的就是推动算法选择越来越好的解决方案。首先，它将通过对明显的错误进行良好的惩罚来丢弃它们，然后让正确的解决方案在它们之间竞争，使每一代都有越来越好的结果。

快吗？是的，检查群体中所有个体的适应度函数应该是微不足道的。但是，它会很快达到最优解吗？不，一点也不。该算法运行速度很快，但是达到可用解所需的代数是数万，甚至更多。交易是，这种算法能够解决具有数百个变量的非常复杂的问题，因为它能够处理内存中的简单向量。如果处理如此大量的数据，任何其他直接算法都会窒息而死。

遗传算法是一种权衡:计算变得琐碎，但迭代解决方案并找到最终答案的时间很长，有时需要一整天或几周。另一个好处是让整个程序并行非常容易。同样，快速运行每一次迭代的加分。但还是那句话，还是需要几万代才能得到可用的答案。第二类人工智能算法[粒子群优化](/multi-agent-algorithms-particle-swarm-optimization-6f11e6b35736)下次见。