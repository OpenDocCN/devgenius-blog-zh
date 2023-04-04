# DWITE’09 R6 第 5 期—航空旅行规划

> 原文：<https://blog.devgenius.io/dwite-09-r6-5-air-travel-planning-b6c57103457d?source=collection_archive---------17----------------------->

为了解决这个问题，我将不得不引入两个新概念。数据结构和贝尔曼-福特算法。

然而，在我开始解释这些概念之前，我想让你尝试一下你自己解决这个问题的方法。我将给出一些解决这个问题的提示:给你一个带权边的有向图，要求你找出单源最短路径。

现在让我们开始讨论这些概念。我们将从数据结构开始。这些在 C++中是真实存在的，你可以如下初始化它们:

```
struct type_name {
member_type1 member_name1;
member_type2 member_name2;
member_type3 member_name3;
};
```

这允许你创建一个新的数据结构。在这个数据结构中，您可以定义变量，这些变量是 member_nameX，其类型为 member_type。基本上，它允许您将一堆不同的数据结构组合在一起，以简化您的代码。对于这个问题，我们将使用这个来帮助制作我们的图表。我们将创建一个名为 Edge 的结构，它有 3 个变量，to、from 和 cost。这将描述从两个地方去的费用。我们需要使用这个，因为没有其他(据我所知)数据结构可以让你轻松地存储 3 种类型，特别是考虑到我们使用两个字符串和一个整数。该结构的代码如下所示:

```
struct Edge {
  string to, from;
  int cost;
};
```

现在我们已经有了结构，我们必须做算法。对于这个问题，我决定使用贝尔曼福特算法，因为它更容易理解，它有一个快速的实现和略快于 Dijkstra 的时间复杂度。我们首先创建一个 hash_map 结构，它将存储从源点到该点的距离。在这种情况下，收到输入后，我们将立即将该节点的值设置为 INT_MAX，这样就可以清楚地看到您还没有访问过它，并且它的值不能进入最短路径。我们将把图形表示为边的数组(向量)。注意边上的大写 E，因为我们将使用新的数据结构。

然后是贝尔曼-福特算法。我们首先将源节点的“成本”设置为 0。然后我们循环所有的成本，然后通过边嵌套一个循环，并找到最小距离。请允许我详细说明。你遍历所有的成本，并根据它是否在图中找到一个更便宜的成本来最小化它。让我们看看这个算法的伪代码:

```
costs = {place:cost}
array = [Edge, Edge... Edge]
Loop through all the costs:
    Loop through the edges:
        cost[current_edge.to] = min(costs[current_edge.from] + current_edge.cost, costs[current_edge.to])
The algorithm consists of several phases. Each phase scans through all edges of the graph, and the algorithm tries to produce relaxation along each edge  $(a,b)$  having weight  $c$ . Relaxation along the edges is an attempt to improve the value  $d[b]$  using value  $d[a] + c$ . In fact, it means that we are trying to improve the answer for this vertex using edge  $(a,b)$  and current response for vertex  $a$ .
```

总而言之，该算法由几个“阶段”组成。每个阶段扫描图的所有边，算法尝试沿着权重为 C 的每条边(a，b)产生松弛。沿着边的松弛是尝试使用价值成本[a] + c 来改善价值成本[b]。实际上，这意味着我们正在尝试使用边(a，b)和顶点 a 的当前响应来改善该顶点的答案。现在让我们用 C++编写一个解决方案:

```
#include <vector>
#include <math.h>
#include <iostream>
#include <bitset>
#include <cstdio>
#include <algorithm>
#include <map>
#include <string>
#include <queue>
#include <set>
#include <climits>
#include <unordered_map>
#include <stack>
#include <cmath>
#include <iomanip>
#include <list>
#include <numeric>
#include <random>

using namespace std;
#define PI 3.14159265358979323851

#define endl "\n"
#define ll long long
char _;
#define scan(x) do{while((x=getchar())<'0'); for(x-='0'; '0'<=(_=getchar()); x=(x<<3)+(x<<1)+_-'0');}while(0)

struct Edge {
 string from;
 string to;
 int cost;
};

int main()
{
 for (int www = 0; www < 5; www++) {
  int N;
  cin >> N;
  map<string, int> costs;
  vector<Edge> edges;
  for (int i = 0; i < N; i++) {
   string S1, S2;
   int cost;
   cin >> S1 >> S2 >> cost;
   Edge New;
   New.from = S1;
   New.to = S2;
   New.cost = cost;
   edges.push_back(New);
   costs[S1] = INT_MAX;
   costs[S2] = INT_MAX;
  }
  costs["YYZ"] = 0;
  for (int i = 0; i < costs.size(); i++) {
   for (Edge e : edges) {
    int cost = min(costs[e.from] + e.cost, costs[e.to]);
    costs[e.to] = cost;
   }
  }
  cout << costs["SEA"] << endl;

 }
}
```

我希望这能帮助你更好地理解这个问题。请随意对你希望看到的其他问题发表评论，并留下赞。