# Drupal 8——仅在节点发生变化时保存节点

> 原文：<https://blog.devgenius.io/drupal-8-only-save-a-node-when-it-has-changes-babffc6b0dd1?source=collection_archive---------9----------------------->

![](img/7a75ee682bc14ae31f832bc491e38279.png)

照片由[克莱门特·H](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

假设您有一个外部服务(或导入)，它在 Drupal 中为您处理节点创建/更新。无论您使用队列还是批处理，都没有关系…

在很多情况下，您可能会在导入过程中循环遍历所有数据，而不管数据是否已经更改。如果您正在使用的 API 只提供更改的数据(以任何可能的方式)，那么您很幸运。你可能不需要看这篇文章:)

遍历所有数据和更新现有节点并不重要，因为这些都是在后台进程中完成的，对吗？没错，但是你想过**缓存失效吗？**

通常，如果您要使用视图或任何其他(自定义)方式构建概视图来构建任何实体列表，您会希望使用{entity_type}_list [缓存标记](https://www.drupal.org/docs/drupal-apis/cache-api/cache-tags)。但是每次保存该类型的任何实体时，您的缓存都会失效。有意义，因为，如果你不这样做，概述就会过时。但是，这意味着，如果您的导入机制保存所有节点，没有一个被改变，你是无效的缓存没有任何理由。

幸运的是，在导入循环中保存节点之前，有一种简单的方法可以检查节点是否发生了变化。我在这里添加了一个示例类向您展示:[https://github . com/NickDaelemans/Drupal-cheat-sheet/blob/master/Drupal 8/check-if-node-has-changed/check-if-node-changed . PHP](https://github.com/NickDaelemans/drupal-cheat-sheet/blob/master/drupal8/check-if-node-has-changed/check-if-node-changed.php)

我们来分解一下。

```
public function getFields() {
  return [
    'field_example_body',
    'field_example_title',
  ];
}
```

这很容易返回您想要检查节点的字段。通常，这是您在导入逻辑中更新的字段。我们稍后会用到它。

让我们看看另一个函数

```
public function isNodeChanged(NodeInterface $node) {
  $original_node = $this->entityTypeManager->getStorage('node')
    ->loadUnchanged($node->id());

  $fields = $this->getFields();
  $node_fields = array_intersect_key($node->toArray(), array_flip($fields));
  $original_node_fields = array_intersect_key($original_node->toArray(), array_flip($fields));
  return $node_fields !== $original_node_fields;
}
```

为了更好的理解，我们来分解一下。

```
$original_node = $this->entityTypeManager->getStorage('node')
  ->loadUnchanged($node->id());
```

首先，让我们通过节点 ID 获得未改变的节点。我们要用这个来检查。

```
$fields = $this->getFields();
```

从我们之前创建的函数中获取字段。

```
$node_fields = array_intersect_key($node->toArray(), array_flip($fields));
```

通过使用 [array_intersect_key](https://www.php.net/manual/en/function.array-intersect-key.php) 我们创建了一个数组，其中包含了我们需要的字段的节点更改值(来自 getFields()函数)。

```
$original_node_fields = array_intersect_key($original_node->toArray(), array_flip($fields));
```

接下来，我们对未更改的节点进行同样的操作。

最后比较它们:

```
return $node_fields !== $original_node_fields;
```

通常，您会希望将这些函数放在一个单独类中，并将其定义为一个服务，这样您就可以在需要时包含它。

下面是一个用法示例:

```
if ($this->nodeUtils->isNodeChanged($node)) {
  $node->save();
}
```

感谢阅读！