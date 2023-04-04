# 如何修复 Commons Lang“包不存在”错误

> 原文：<https://blog.devgenius.io/how-to-fix-commons-lang-package-does-not-exist-error-db4051b8a46b?source=collection_archive---------0----------------------->

![](img/8aac31c2897d1939056d65597d199d0e.png)

如果您在这里，那么您可能会遇到一个 Java 堆栈跟踪，其中有类似这样的行:

```
package org.apache.commons.lang does not exist...static import only from classes and interfaces
```

我在一些使用 Maven 的 Spring Boot 应用程序中看到了这一点，好消息是修复应该很容易。

# 修复

在源代码中，对以下`import`前缀进行全局搜索:

```
import static org.apache.commons.lang
```

然后，将所有实例替换为以下内容:

```
import static org.apache.commons.lang3
```

# 例子

假设一个程序有下面的 Commons Lang `import`语句:

```
import static org.apache.commons.lang.StringUtils.isBlank;
import static org.apache.commons.lang.StringUtils.isNotBlank;
```

简单的在`lang`后面加一个`3`。你最后的`import`陈述将会是这样的:

```
import static org.apache.commons.lang3.StringUtils.isBlank;
import static org.apache.commons.lang3.StringUtils.isNotBlank;
```

您的应用程序现在应该可以构建了。

# 为什么？

基本上，Commons Lang `3.x`用`lang3`中的`3`引入了这个新的包命名。更多细节以及从`2.x`到`3.x`的其他变化在[这里](https://commons.apache.org/proper/commons-lang/article3_0.html)。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)