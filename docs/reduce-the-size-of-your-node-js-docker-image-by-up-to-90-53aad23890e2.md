# 将 Node.js docker 图像的大小缩小 90%

> 原文：<https://blog.devgenius.io/reduce-the-size-of-your-node-js-docker-image-by-up-to-90-53aad23890e2?source=collection_archive---------0----------------------->

**对你的下一个 Node.js docker 图像做一些简单的调整，这将大大减小图像的大小。**

![](img/676022f2f7889cd3d816094ebca29e89.png)

照片由 [Siora 摄影](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

典型的 Node.js 映像的大小大约为 350MB。加上您的 *node_modules* 和 *src* ，这通常会超过 1 GB。有很多方法可以缩小你的图片

*   选择正确的基础图像
*   仅安装生产依赖项
*   使用多阶段构建
*   仅复制所需文件
*   修剪节点模块

# 示例应用程序

我们的示例应用程序是由`npx create-react-app`创建的默认 react 应用程序，它通过 express 提供服务。我还添加了 eslint 作为开发依赖项。应用程序的代码可以在这里找到:[https://github . com/Christopher scholz/reduce _ node _ docker _ image _ size/tree/main/context](https://github.com/christopherscholz/reduce_node_docker_image_size/tree/main/context)

在下面的内容中，我将不会更改应用程序代码，而只更改 Dockerfile 代码。

# 选择正确的基础图像

首先，在创建 docker 文件时，我们可以从不同的节点 docker 映像中进行选择。最常见的标签是*最新*，是*当前*的别名，是*当前-靶心*的别名，是*靶心*的别名，是[*19 . 0 . 0-靶心*](https://hub.docker.com/layers/library/node/19.0.0-bullseye/images/sha256-2b00d259f3b07d8aa694b298a7dcf4655571aea2ab91375b5adb8e5a905d3ee2?context=explore) 的别名，写这个故事的时候。对于我们所有的 docker 文件，我们将复制应用程序，安装 react 包，构建 react 并安装 express 包。对于牛眼文件来说，看起来是这样的

使用牛眼基础图像提供 express react 应用

在 amd64 上，此图像的大小为 1.4 GB，这将是我们的起始大小。

除了*牛眼*，我们还有*牛眼细长*和*高山*两种选择。如果我们仅将 Dockerfile 的`FROM node:19.0.0-bullseye`语句更改为`FROM [19.0.0-bullseye-slim](https://hub.docker.com/layers/library/node/19.0.0-bullseye-slim/images/sha256-978417e9dd45a1b7f673077d56fc54cb7bab98f6592f12fe7861d80a4669ad4a?context=explore)`或`FROM [19.0.0-alpine3.16](https://hub.docker.com/layers/library/node/19.0.0-alpine3.16/images/sha256-e74f32d2a985f4d7d3ced0754bb1c40c68b6c0431043b154cb44127546e94f11?context=explore)`，我们会将图像分别缩小到 652.75 MB(整体缩小 53.4%)或 576.89 MB(整体缩小 58.8%，与之前相比缩小 11.6%)。

这是最大也是最容易的尺寸缩减，但这还不是我们能做的全部。

# 仅安装生产依赖项

在下一步中，我们可以确保在构建 docker 映像时不添加开发依赖项。有多种方法可以做到这一点。我认为最简单的方法是将环境变量 *NODE_ENV* 设置为 *production* 。我们可以通过在`FROM 19.0.0-alpine3.16`语句后添加行`ENV NODE_ENV=production`来做到这一点。

通过这样做，我们有效地移除了 *eslint* 包。这将图像缩小到 566.73 MB(整体缩小了 59.5%，与之前的优化相比缩小了 1.8%)

# 使用多阶段构建

另一种减少图像尺寸的方法是使用多阶段构建。想法是使用*构建器*映像构建应用程序，然后只将运行应用程序所需的文件复制到运行器*映像*中。

在我们的阿尔卑斯山图像的情况下，这可能看起来像这样

使用多阶段构建提供 express react 应用程序

这将 docker 映像的大小缩小到了 178.04 MB(总体减少了 87.3%，与之前的优化相比减少了 68.6%)。

*Alpine* 不是我们能用的最小基础图像。一个真正的捆绑了 *Node.js* 运行时的最小 Linux 是[distro less/nodejs 18-debian 11](https://github.com/GoogleContainerTools/distroless/blob/main/nodejs/README.md)。使用它的唯一方法是使用多阶段构建，因为我们只能`COPY`文件到映像，而不能`RUN`任何应用程序，因为没有安装 shell。 *Node.js* 运行时也被最小化，因为它甚至删除了 *npm* 和 *npx* 。使用 distroless 映像的 docker 文件如下所示

使用多阶段构建和无发行版运行程序提供 express react 应用程序

这将 docker 映像的大小缩小到了 162.53 MB(总体减少了 88.4%，与之前的优化相比减少了 8.7%)。

# 仅复制所需文件

我们可以通过确保只保存`COPY`相关文件来进一步减小图像的大小。在最终的 docker 映像中构建相关文件也是一个常见的安全问题。

对于我们的应用程序，这意味着用

仅复制相关文件

在我们的例子中，这只删除了两个文件(。gitignore 和 package.json)，所以大小影响最小。新的大小为 162.41 MB(整体减少了 88.4%，比之前的优化减少了 0.1%)。

# 修剪节点模块

我们可以自动化的另一个步骤是进一步删减 node_modules。https://github.com/tj/node-prune 工具[是一种简单的方法。我们只需要下载二进制文件并在 npom 包中运行它。根。我们可以用一个简单的语句来实现](https://github.com/tj/node-prune)

使用 wget 下载，安装并运行节点清理

或者如果我们使用带有卷曲的基本图像而不是 wget

使用 curl 下载，安装并运行节点修剪

这是我们缩小图像尺寸的最后一步。最终大小为 161.44 MB(整体减少了 88.5%，比之前的优化减少了 0.6%)。最终的 docker 文件如下所示

为 te express react 应用程序提供优化的 Dockerfile 文件

我们可以通过手动检查我们可以删除的文件来更深入地了解这一点。分析图像的一个好方法是使用[潜水](https://github.com/wagoodman/dive)工具。

# 额外:通过选择更好的构建顺序显著加快构建速度

docker 文件中的每条语句都会创建一个新层。图层被缓存。如果一个层发生变化，所有下游层也必须重建。因此，以这样一种方式构建 docker 文件是非常重要的，即经常更新的层在 docker 文件的末尾，而几乎静态的层在 docker 文件的开头。如果你聪明地使用这个原则，你可以显著地减少你的构建时间。

# 结论

我们将 docker 映像从 1.4 GB 降至 161.44 MB，降幅为 88.5%。

为了减小节点 docker 映像的大小并加快构建时间，您可以做很多事情。这些想法通常也适用于其他应用程序。例如，使用一个无发行版的映像可能会大大减少你的下一个 Rust 应用程序。

我希望你发现这些想法很有趣，并能够将它们应用到你的下一个或当前项目中。

也非常感谢以下故事的作者，他们帮助我学习了很多关于优化 docker 构建的知识

*   [如何将你的 Node.js docker 图片大小缩小 70%](https://blog.tarkalabs.com/how-to-reduce-nodejs-docker-image-by-70-e799b3d3396a) 作者[马达瓦雷迪 SV](https://medium.com/u/5c0d85235c3b?source=post_page-----53aad23890e2--------------------------------)
*   [我们如何通过](https://medium.com/trendyol-tech/how-we-reduce-node-docker-image-size-in-3-steps-ff2762b51d5a)[Soner kmen](https://medium.com/u/18b9b9a2b09e?source=post_page-----53aad23890e2--------------------------------)分三步缩小节点 Docker 图像大小
*   [停止使用阿尔卑斯码头工人图片](https://medium.com/inside-sumup/stop-using-alpine-docker-images-fbf122c63010)由[熊伟法尔考](https://medium.com/u/e8ced5abda1?source=post_page-----53aad23890e2--------------------------------)
*   [亲爱的，我把 node_modules 缩小了！…并提高了应用程序在此过程中的性能。关于节点模块大小](https://tsh.io/blog/reduce-node-modules-for-better-performance/)
*   [优化 Docker 构建的技巧](https://tier.engineering/Tips-for-optimising-docker-build)作者[詹姆斯·塞缪尔](https://www.linkedin.com/in/abiodunjames/)