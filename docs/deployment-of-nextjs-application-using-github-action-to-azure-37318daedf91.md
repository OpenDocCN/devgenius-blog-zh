# 使用 GitHub action 将 NextJS 应用程序部署到 Azure

> 原文：<https://blog.devgenius.io/deployment-of-nextjs-application-using-github-action-to-azure-37318daedf91?source=collection_archive---------1----------------------->

正如我在上一篇文章中承诺的那样，我写了这篇文章来展示 Azure、GitHub 和 NextJS 是如何很好地协同工作的。我想使用 **Github actions 将我的应用程序部署到 Azure** ，但是在我的旅程中，我在一些问题上遇到了*。所以我希望下面我的经验总结能对每一个想让*走同样道路*的人有所帮助。*

实际上，找到一种部署我的应用程序的方法感觉就像是一个侦探的调查。我需要在各种不同的 GitHub 问题和博客中搜索信息。

所以，让我们从一个基本问题开始:

什么是**发布神器**？

理论上，NextJS 构建的应用程序包含了"*的全部内容。下一个*文件夹在你运行命令*下一个构建*之后实际上，情况并非如此。该文件夹包含“ *cache* ”文件夹，看起来像是 NextJS 编译器的临时缓存。文件夹可能会变得非常大，因为每次构建都会增加文件夹的大小，因此绝对应该从您部署的工件中忽略。

真正让我觉得有趣的是文档中根本没有提到它。我不得不竭尽全力在 GitHub 问题中找到它。

正如 Vercel 的一位高级软件开发人员[评论的那样](https://github.com/vercel/next.js/issues/20840):

> 你需要缓存。跨构建的 next/cache，但是它不需要上传到发布工件中。这台自动取款机没有相关文件。

天哪，这些信息看起来太基本了，不可能不被官方文件所涵盖。

**上传到服务器**

在弄清楚我的应用程序的真实版本之后，我必须弄清楚如何将它上传到服务器。

首先，我尝试了名为 [webapps-deploy](https://github.com/Azure/webapps-deploy) 的经典动作。

可惜没用，因为不允许我设置目标文件夹。如果你读过我以前的博文，你就会知道这个特性是至关重要的，因为我必须设置一个不同于预设根文件夹的文件夹(文件夹“site/wwwroot”)。

当经过一些尝试和错误后，我终于意识到我已经走进了死胡同，我高兴地切换到一个良好的旧 FTP 行动。

我选择了方便的[操作 simple-ftp-deploy](https://github.com/kevinpainchaud/simple-ftp-deploy-action) ，所以我的最终配置如下所示:

```
- name: Upload ftpuses: kevinpainchaud/simple-ftp-deploy-action@v1.2.1with:ftp_host: ${{ secrets.FTP_SERVER_PROD }}ftp_username: ${{ secrets.FTP_USERNAME_PROD }}ftp_password: ${{ secrets.FTP_PASSWORD_PROD }}local_source_dir: ".next"dist_target_dir: "app/.next"delete: "true"other_flags: "--dereference --no-perms"
```

我希望它是不言自明的。只需注意我设置的路径:" **dist_target_dir: "app/。下一个"**"

然后我在 package.json 中的脚本如下所示:

*“剧本”:{*

*“开始”:“cd。/app & &下一次开始】，*

那是命令*的光盘。/app&&*这就行了。

**重启 Azure Web App**

我需要做的最后一件事是在复制新版本后在 Azure 中重启 Docker 容器。

在寻找一种简单的方法来重启我的网站后，当我意识到修改文件"*config/restart trigger . txt***"**使得 Azure 容器重启时，我停止了寻找。

我试着用另一种方式搜索，例如，使用 Rest API，但是因为它需要你处理认证，我决定重用 FTP 动作。

但是，有一个**小取舍**。为了让它工作，我必须将一个名为“*azure start*”(或者任何你想要的名字)的文件夹提交给我的源代码。在这个文件夹中，我创建了一个名为“ *restartTrigger.txt* 的文件。”该文件的文本内容是任意的，但是名称必须与文本内容完全相同。我在 CI 管道的最后一步中使用了这个文件来重启容器。见下文。

```
- name: Restart the siteuses: kevinpainchaud/simple-ftp-deploy-action@v1.2.1

with:
ftp_host: ${{ secrets.FTP_SERVER_PROD }}
ftp_username: ${{ secrets.FTP_USERNAME_PROD }}
ftp_password: ${{ secrets.FTP_PASSWORD_PROD }}
**local_source_dir: “AzureRestart”
dist_target_dir: “../config”** delete: “true”
other_flags: “ — dereference — no-perms”
```

请注意，源文件夹和目标文件夹都不是以斜杠结尾的。使用斜杠，它将复制整个目录，而不仅仅是文件。

仅此而已。也许这看起来有点古怪，但它对我很有帮助。