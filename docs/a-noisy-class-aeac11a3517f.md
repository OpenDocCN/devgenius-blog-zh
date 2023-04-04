# 吵闹的班级

> 原文：<https://blog.devgenius.io/a-noisy-class-aeac11a3517f?source=collection_archive---------24----------------------->

虽然你可能很想用 BFS/DFS 的方法来解决这个问题，但法官会很快给你上一课，告诉你这不是你想要的解决方案，你很可能会选择 MLE。这个问题需要一个不同的方法，一个复杂得多的问题陈述。

消化完问题后，你会意识到问题是要求我们找出这个图中是否存在循环。虽然您可能决定采用建议的方法来跟踪两个集合，一个是已访问的集合，另一个是当前节点有噪声时的集合，但我意识到这导致了 MLE 错误，因此我开始寻找替代方法。这是我的实现:

```
#include <vector>
#include <math.h>
#include <iostream>
#include <bitset>
#include <cstdio>
#include <algorithm>
#include <map>
#include <set>
#include <climits>
#include <stack>

using namespace std;

#define endl "\n"
#define ll long long

bool dfs(vector<vector<int>> graph, int N, int index) {
 vector<bool> visited(N, false);
 stack<int> stack;
 vector<bool> noisy(N, false);
 stack.push(index);
 while (!stack.empty()) {
  int current = stack.top();
  stack.pop();
  noisy[current] = false;
  visited[current] = true;
  for (int i = 0; i < N; i++) {
   if (!visited[graph[current][i]] && graph[current][i] == 1) {
    stack.push(graph[current][i]);
    for (int j = 0; j < N; j++) {
     if (graph[i][j] == 1) {
      noisy[j] = true;
     }
    }
   }
  }
  for (int i =0 ; i < visited.size(); i++){
   if (stack.empty() && !visited[i]) {
    stack.push(i);
    break;
   }
  }
 }
 for (int i = 0; i < noisy.size(); i++) {
  if (noisy[i]) {
   return false;
  }
 }
 return true;
}

int main()
{
 int N, M;
 cin >> N >> M;
 vector<vector<int>> arr(N, vector<int>(N));
 for (int i = 0; i < M; i++) {
  int t1, t2;
  cin >> t1 >> t2;
  arr[t1-1][t2-1] = 1;
 }

 if (dfs(arr, N, 0)) {
  cout << "Y" << endl;
 }
 else {
  cout << "N" << endl;
 }
}
```

请在下面随意评论这种方法。我得到的错误是 MLE，它导致我得到 20/100。这让我想到了拓扑排序。虽然您可能会怀疑这是如何工作的，但是添加一个计数器变量来跟踪您已经访问了多少个节点使这变得非常简单。这是因为如果你已经访问了每个学生(或节点)一次，并且你已经访问了所有的学生(告诉他们要安静),就有可能有一个安静的班级。然而，如果偶然你没有接触到一个学生，或者有一个循环，你会“看到”比班上更多的学生，这样你就会有一个循环。基本上，我采用的方法(有效)是使用标准的拓扑排序，决定因素是班上的学生人数是否与我告诉要安静的学生人数相同。这是我的下一个(也是有效的)方法:

```
#include <vector>
#include <math.h>
#include <iostream>
#include <bitset>
#include <cstdio>
#include <algorithm>
#include <map>
#include <set>
#include <climits>
#include <stack>

using namespace std;

#define endl "\n"
#define ll long long
bool top_sort(map<int, set<int>> graph, map<int, int> indegree, int N) {
 priority_queue<int, vector<int>, greater<int>> q;
 int counter = 0;
 for (int i = 1; i <= N; i++) {
  if (indegree[i] == 0) {
   q.push(i);
  }
 }
 while (!q.empty()) {
  int current = q.top();
  q.pop();
  counter++;
  for (auto next : graph[current]) {
   if (--indegree[next] == 0) {
    q.push(next);
   }
  }
 }
 if (counter == N) {
  return true;
 }
 else {
  return false;
 }
}

int main() {
 cin.tie(0); cout.tie(0); cin.sync_with_stdio(0);
 int N, M;
 cin >> N >> M;
 map<int, set<int>> graph;
 map<int, int> indegree;
 for (int i = 0; i < M; i++) {
  int t1, t2;
  cin >> t1 >> t2;
  if (graph[t1].count(t2)) {
   continue;
  }
  graph[t1].insert(t2);
  indegree[t2]++;
 }
 bool response = top_sort(graph, indegree, N);
 if (response) {
  cout << "Y" << endl;
 }
 else {
  cout << "N" << endl;
 }
}
```

如你所见，这是一个标准的拓扑排序，记录了到目前为止访问过的人数。请在下面的评论中留下 DMOJ(或其他评委)的任何其他问题，并确保留下一个赞。