# DRY 规划原理的更好定义

> 原文：<https://blog.devgenius.io/a-better-definition-of-the-dry-programming-principle-13a3d0af4d40?source=collection_archive---------5----------------------->

🚀[《T4》一书打造分层微服务](https://learnbackend.dev/books/build-layered-microservices) 已经过时了！现在就在[learn backnd . dev](https://learnbackend.dev/books/build-layered-microservices)购买自己的副本。

![](img/d9161f75de75c677d89f6f6a02c9b0dd.png)

低级开发人员最常忽略和误解的一个编码原则是 DRY 原则，它代表*不要重复自己。*

通常被视为 WET(WET 代表*将所有内容写两次*)的直接对立面，DRY 原则实际上并不完全是关于通过使用复杂的抽象不惜一切代价避免代码重复，而是主要关于避免知识重复。

尽管人们普遍认为，在整个应用程序中复制粘贴同一段代码是导致不一致和错误的最佳方法，但仅仅关注这一方面实际上是一个错误，因为过早的优化尝试通常会导致复杂性和代码耦合的增加。

# 更好的 DRY 定义

话虽如此，DRY 的更好定义将是选自《实用主义程序员》一书的以下定义，该书指出:

> 在一个系统中，每一条知识都必须有一个单一的、明确的、权威的表示。

其中一条知识是业务领域中的特定逻辑或算法。

现在让我们用三个例子来说明这一点，它们将让您更好地了解什么时候应该根据上下文进行重构，什么时候不应该进行重构，首先从常量开始。

# 示例 1:常量

重构常量可能是尝试应用 DRY 原则时首先想到的。

假设我们有以下三个用于显示文本的 React 组件，它们都使用相同的深灰色代码。

```
/*
 * File: text.js
 */function Title(props) {
  return (
    <h1 style={{color: "#3A3B3C"}}>
      {props.title}
    </h1>
  );
}function Subtitle(props) {
  return (
    <h2 style={{color: "#3A3B3C"}}>
      {props.title}
    </h2>
  );
}function Paragraph(props) {
  return (
    <p style={{color: "#3A3B3C"}}>
      {props.text}
    </p>
  );
}
```

在这种情况下，如果你要决定明天采用哪种不同的灰色调，你就必须手工更换每一个出现的颜色代码，确保不要忘记任何一个，这确实需要投入比它应该投入的更多的精力和工作。

正确的方法是创建一个名为`colours`的新常量文件，其中包含该色阶的值，并在整个应用程序中代表唯一的真值来源。

```
/*
 * File: colours.js
 */export default {
  DARK_GRAY: '#3A3B3C'
};
```

因此，下一次您想要更新它时，只有一个快速变化适用。

```
/*
 * File: text.js
 */import { DARK_GRAY } from '../constants/colours';function Title(props) {
  return (
    <h1 style={{color: DARK_GRAY}}>
      {props.title}
    </h1>
  );
}function Subtitle(props) {
  return (
    <h2 style={{color: DARK_GRAY}}>
      {props.title}
    </h2>
  );
}function Paragraph(props) {
  return (
    <p style={{color: DARK_GRAY}}>
      {props.text}
    </p>
  );
}
```

# 示例 2:相同的域逻辑

现在让我们考虑以下两个函数`editPost`和`deletePost`，它们分别用于在博客平台如 media 上编辑和删除帖子。

```
function editPost(userId, postId) {
  const post = database.fetch(postId); if (post && post.ownerId === userId) {
    // Edit post
  } else {
    // Throw error
  }
}function deletePost(userId, postId) {
  const post = database.fetch(postId);

  if (post && post.ownerId === userId) {
    // Delete post
  } else {
    // Throw error
  }
}
```

正如您在这里看到的，用于检查用户是否被允许执行这些操作的相同验证逻辑在两个函数中被复制，这导致了两个问题。

第一个是，如果逻辑随着时间的推移而改变，它将不得不再次复制，如果不小心的话，这可能会引入错误。

第二个是，这些功能被赋予了处理其范围之外的业务逻辑的责任，而不是单独做它们应该做的事情——即更新和删除。

因此，合适的做法是将这个逻辑提取到一个单独的组件中，这反过来将使这些功能同时摆脱这两个问题。

```
function isUserPermitted(userId, postId) {
  const post = database.fetch(postId);

  return post && post.ownerId === userId;
}function editPost(userId, postId, data) {
  if (!isUserPermitted(userId, postId)) {
    // Throw error
  }
  // Edit post
}function deletePost(userId, postId) {
  if (!isUserPermitted(userId, postId)) {
    // Throw error
  }
  // Delete post
}
```

# 示例 3:不同的域逻辑

最后，让我们假设我们正在运行一个实现以下两个功能的电子商务平台，`getAmountWithVAT`计算客户必须为一个订单支付的金额(包括增值税)和`getLatePaymentPenalties`计算如果客户没有按时付款将在账单上应用的罚款。

```
function getAmountWithVAT(amount) {
  const VAT = 0.20; return amount + (amount * VAT);
}function getLatePaymentPenalties(amount) {
  const penalty = 0.20; return amount + (amount * penalty);
}
```

由于这两个函数似乎共享相同的逻辑，例如，您可能想通过将它们合并成一个名为`addPercentage`的函数来重构代码，但是在实践中，这被证明是一个非常糟糕的想法，因为这两个函数实际上并不共享相同的业务领域。

```
function addPercentage(amount, percentage) {
  return amount + (amount * percentage);
}
```

事实上，如果明天您决定不仅对逾期付款应用固定费用，而且应用每周增加 10%的累进费用，您将不得不将这个`addPercentage`函数再次拆分成两个不同的函数，以便能够实现您的新逻辑并摆脱您之前创建的耦合。

因此，请确保您选择重构的代码片段实际上共享相同的业务领域，并努力记住，有时看似重复的知识只是纯粹的巧合。

# 结论

最后，为了避免陷入创建不必要或错误的抽象的陷阱，这些抽象最终可能会破坏另一个重要的编程原则，称为 YAGNI——代表*你不会需要它*——请记住代码复制并不自动意味着干违反原则，但知识复制会。

# 下一步是什么？

不要忘记👏🏻如果你喜欢读我的作品！

👉你喜欢这种内容？在 [https://learnbackend.dev](https://learnbackend.dev/) 查看《如何使用 Express framework 构建生产就绪的分层认证微服务》一书 [**构建分层微服务**](https://learnbackend.dev/books/build-layered-microservices) ，该书从第一行代码到最后一行文档都符合开发实践和软件架构方面的行业标准。