# Node.js 最佳实践—项目设置

> 原文：<https://blog.devgenius.io/node-js-best-practices-project-setup-bf5276a199fe?source=collection_archive---------2----------------------->

![](img/2bf74885281ea99e62a3f186659ca1ff.png)

照片由[波格丹一世·托多兰](https://unsplash.com/@todoranb_26?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 坚持小写

我们应该坚持使用小写文件名，以使文件可移植。

由于 Node 是跨平台的，如果我们的名字有不同的大小写，我们可能会遇到问题。

为了使我们的生活更容易，我们应该在任何地方都使用小写字母。

# 聚集我们的应用

为了让我们的应用在所有内核可用的情况下运行，我们应该将我们的应用集群化。

我们的应用程序的一个实例只能使用一个 CPU 核心和 1.5GB 的内存。

因此，如果我们不对它们进行集群，许多资源可能会被闲置。

为了超越极限，我们应该将我们的应用集群化。

为此，我们可以使用 [forky](https://www.npmjs.com/package/forky) 和[through](https://www.npmjs.com/package/throng)来创建我们的集群。

# 有环保意识

我们应该使我们的应用程序可配置，以便它可以在任何环境下运行。

这样，我们将所有配置读入我们的文件。

例如，我们可以写:

```
DATABASE_URL='postgres://localhost/foobar'
HTTP_TIMEOUT=10000
```

在我们的`.env`文件中。

然后我们可以使用一些像 dotenv 这样的库来读取文件。

# 避免垃圾

在回收未使用的内存之前，节点将等待达到内存限制。

你改变这一点，我们可以设置一些设置，当我们运行我们的应用程序，以回收更快的内存。

例如，我们可以通过键入以下内容来运行它:

```
node --optimize_for_size --max_old_space_size=920 --gc_interval=100 server.js
```

`optimize_for_size`选项将更快地回收内存。

这将使节点减少内存大小，但在速度上妥协，因为未使用的内存回收更快。

`gc-interval`将垃圾收集间隔设置为 100 毫秒。

`max_old_space_size`限制堆中对象的总大小。

# 钩住

我们可以通过添加生命周期脚本来添加定制挂钩。

为此，我们添加了`package.json`的所有`scripts`部分来添加脚本。

例如，我们可以写:

```
"scripts": {
  "build": "bower install && grunt build",
  "start": "grunt"
}
```

添加 2 个脚本，`build`和`start`。

然后我们可以运行`npm run build`和`npm start`来运行脚本。

为了更好地控制脚本，我们可以运行一个外部脚本文件:

```
"build": "scripts/build.sh"
```

# 只跟踪重要的部分

我们应该只跟踪我们创建的代码文件。

其余的应该放在`.gitignore`中，这样它们就被忽略了。

`node_modules`应该被忽略，所以我们用`npm install`来安装它们，而不是将大量的包提交到我们的项目库中。

这将大大减少我们项目的规模，我们可以进行任何操作，如检出、分支等。快多了。

如果我们签入了`node_modules`文件夹，我们应该通过运行以下命令来删除该文件夹:

```
echo 'node_modules' >> .gitignore
git rm -r --cached node_modules
git commit -am 'ignore node_modules'
```

我们把第一行的`node_modules`放入`.gitignore`。

然后，我们从第二行的跟踪中移除`node_modules`。

然后我们用第三行中的更改进行新的提交。

我们可以用同样的方式忽略`npm-debug.log`文件:

```
echo 'npm-debug.log' >> .gitignore
git commit -am 'ignore npm-debug'
```

![](img/28fbcc2f587dda091bbfae8345e68208.png)

照片由 [Jametlene Reskp](https://unsplash.com/@reskp?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

对于我们的节点项目，我们可以做一些事情来改进它。

我们可以让垃圾收集更频繁，我们应该忽略自动生成的文件和文件夹。