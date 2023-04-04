# DMPG 2017 S2 动漫大会

> 原文：<https://blog.devgenius.io/dmpg-17-s2-anime-conventions-d045289745d7?source=collection_archive---------14----------------------->

在学习图论的时候，我碰到了[这个问题](https://dmoj.ca/problem/dmpg17s2)。问题的前提是给你指令，要么在两个节点之间添加一条边，要么检查两个节点之间是否存在边。最初，每当我必须检查两个节点之间的边时，我都进行广度优先搜索，当我被提示添加边时，我像平常一样添加它。然而，我很快意识到，对于给定的限制，BFS/DFS 不够快。这促使我寻找更好的方法。这让我想到了不相交集合数据结构。最初，我不知道这是什么数据结构，所以我看了 Abdul Bari 的视频，视频对它进行了精彩的解释。

请务必观看视频，以便了解数据结构的前提。我将在这里解释它。基本上，在这种数据结构中，您使用一个数组来表示图形。图中的每个节点都属于一个集合，该集合是一组节点。然后，当添加边时，在边连接的地方将这两个集合合并在一起。这个数据结构使用两个函数，merge (union)和 find。merge 函数将组合这两个集合，find 函数将查找该集合的根。

现在让我们实现合并和查找功能。我们从寻找开始。

```
vector<int> arr
int find(int x) {
 while (x != arr[x]) {
  arr[x] = arr[arr[x]];
  x = arr[x];
 }
 return x;
}
```

在本例中，arr 是包含图形节点的数组。在这个问题中，我们将用大小 100001 初始化这个数组。然后，我们搜索整个图，以找到该集合的根。这是 find 函数最有效的实现。请注意，这也可以递归完成，但这意味着您将有递归函数的堆栈开销。

接下来，我们需要实现合并功能。这个函数接受两个输入，x 和 y。这是我们想要合并的集合的两个节点。然后我们寻找集合 x 和 y 的根，然后检查它们是否已经合并。如果不是，我们将 arr 中的任一根设置为另一根。这实质上是将它们结合在一起。如有需要，请参考 Abdul Bari 的视频了解更多信息。

```
void merge(int x, int y) {
 int x_root = find(x);
 int y_root = find(y);
 if (x_root == y_root) {
  return;
 }
 arr[x_root] = y_root;
}
```

现在我们有了主函数。我们首先需要初始化 arr 数组，让每个节点都指向自己。这意味着它们不会都指向节点 0(就像数组的常规初始化一样)。我们将使用 iota()函数来做到这一点。接下来，我们将检查每个查询，如果是“添加”查询，我们将使用 merge()函数，如果是“查找”查询，我们将使用 find 函数并检查它们是否有相同的 set root。这是完整的代码:

```
vector<int> arr(100001);

int find(int x) {
	while (x != arr[x]) {
		arr[x] = arr[arr[x]];
		x = arr[x];
	}
	return x;
}

void merge(int x, int y) {
	int x_root = find(x);
	int y_root = find(y);
	if (x_root == y_root) {
		return;
	}
	arr[x_root] = y_root;
}

int main()
{
	int N, Q;
	cin >> N >> Q;
	iota(arr.begin(), arr.end(), 0);
	while (Q--) {
		char q_type;
		int t1, t2;
		cin >> q_type >> t1 >> t2;

		if (q_type == 'A') {
			merge(t1-1, t2-1);
		}
		else {
			if (find(t1-1) == find(t2-1)) {
				cout << "Y" << endl;
			}
			else {
				cout << "N" << endl;
			}
		}
	}

}
```

如果这有帮助，请留下赞，并随时评论你想让我尝试的任何其他问题。

![](img/1681f2faaeb46166ec01fa30a4a2e390.png)