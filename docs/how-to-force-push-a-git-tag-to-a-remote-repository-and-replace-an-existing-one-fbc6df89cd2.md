# 如何将 Git 标签强制推送到远程存储库

> 原文：<https://blog.devgenius.io/how-to-force-push-a-git-tag-to-a-remote-repository-and-replace-an-existing-one-fbc6df89cd2?source=collection_archive---------1----------------------->

![](img/3d68923c42aafd3af8f94db4632445d7.png)

[Yancy Min](https://unsplash.com/@yancymin) 在 [Unsplash](https://unsplash.com/photos/842ofHC6MaI) 上拍照

这有点具体，但是假设您想要将 Git 标签强制推送到远程存储库。

这可能是因为您遇到了这样的情况:一个标签已经远程存在，但是您想要更新它而不删除它。

没错:替换远程标记的一种方法是实际删除远程(和现有的)标记，然后将您的标记推送到远程存储库。您可能有各种各样的原因不想这样做(例如，当您删除标签和推送新标签之间的时间是标签根本不存在的时间，这在某些情况下可能会有问题)。

这篇文章不会讨论你是否应该这样做，而是简单地讨论你应该如何做。当在 Git 中使用任何类型的 force 命令时(老实说，一般来说)，您应该非常自信地知道自己在做什么并承担风险。

说到这里，假设标签`1.0.0`存在于 Git 中。

当您运行`git tag`(您可以选择使用`-l`或`--list`)时，您会看到它的输出:

```
$ git tag
1.0.0
```

如果存在其他标签，您也可以看到它们。

如果您想用已存在的相同标记`1.0.0`来标记本地提交，您将无法:

```
$ git tag 1.0.0
To git@provider.com:username/repo-name.git
 ! [rejected]        1.0.0 -> 1.0.0(already exists)
error: failed to push some refs to 'git@provider.com:username/repo-name.git'
hint: Updates were rejected because the tag already exists in the remote.
```

因此，为了解决这个问题，您将使用`-f`或`--force`标志。它们是可以互换的。如果你想要短一点的，就选`-f`；如果你想要一些对新手来说更加清晰易读的东西，使用`-force`。

首先，您将强制标记现有的标记:

```
$ git tag -f 1.0.0
```

然后，您会想要将标签推送到远程存储库，但是为了避免由于标签存在于那里而产生的任何问题，您将在推的时候使用相同的标志:

```
$ git push -f origin 1.0.0
```

然后，您应该会收到一条确认消息，表明强制推送已完成。一个好的下一步是验证您想要标记的提交散列实际上被正确地推送并替换了旧的散列。

现在你知道了！希望这是直截了当的，并愉快地负责任地使用那面`-f`旗。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)