# 准备离开 Twitter

> 原文：<https://blog.devgenius.io/move-away-from-twitter-1dbee71383fc?source=collection_archive---------16----------------------->

![](img/72a901d7e275ddce0686757be69a5013.png)

13 年前，也就是 2009 年 8 月，我开通了自己的 Twitter 账户。12 年来，我一直专注于与专业相关的内容:Java、JVM、编程等。我建立了自己的读者群，试图推广好的技术内容，无论是我自己的还是我喜欢阅读的内容。

然后，2 月 24 日，俄罗斯入侵乌克兰。我第一次去乌克兰是在 2014 年，就在独立广场革命之后。在这八年中，我经常回到那里，交了很多朋友。

当然，我想支持他们，并开始使用我的 Twitter 账户来对抗俄罗斯的虚假信息。在从一开始就置身政治之外之后，我发现 Twitter 有多毒:不守信用、逻辑谬误、彻头彻尾的谎言、反向指控、人身攻击等等。

随着埃隆·马斯克收购 Twitter，恐怕情况会变得更糟——一个恰当的例子是:

![](img/46e2f27ea40a146d66e0a0e4ebd39468.png)

[https://Twitter . com/sarahk Silverman/status/1589418271308386304](https://twitter.com/SarahKSilverman/status/1589418271308386304)

一些(大部分？)我认识的计划或已经搬走的人。目标似乎是乳齿象，一个使用 [ActivityPub](https://en.wikipedia.org/wiki/ActivityPub) 协议的替代分散式开源软件:

> *乳齿象是运行自托管社交网络服务的免费开源软件。它具有类似于 Twitter 服务的微博功能，由大量独立运行的乳齿象节点(技术上称为实例)提供，每个节点都有自己的行为准则、服务条款、隐私选项和审核政策。*
> 
> *每个用户都是特定 Mastodon 实例(也称为服务器)的成员，该实例可以作为联合社交网络进行互操作，允许不同节点上的用户相互交互。这是为了让用户能够灵活地选择他们喜欢的服务器策略，同时保持对更大的社交网络的访问。Mastodon 也是 Fediverse 服务器平台的一部分，这些平台使用共享协议，允许用户与其他兼容平台上的用户进行交互，如 PeerTube 和 Friendica。*
> 
> *—*[*https://en . Wikipedia . org/wiki/mastosdon _(软件)*](https://en.wikipedia.org/wiki/Mastodon_(software))

预先警告就是预先武装。我计划尽可能长时间地呆在 Twitter 上，同时用同样的内容建立我的乳齿象账户。然后，如果(什么时候？)一切都乱套了，我可以直接跳槽。

# 评估备选方案

让我们把事情说清楚:我相信我是一个好的开发人员，因为我很懒。我不可能在两个频道都复制粘贴内容。另外，我正在使用 Twitter 的日程安排功能，所以我需要别的东西。

我是许多想涉足这两个领域的人之一。例如，我发现马丁·福勒也在遵循[同样的策略](https://martinfowler.com/articles/exploring-mastodon.html)。然而，他的方法是“特定的”:

> *我想在乳齿象上做的一件主要事情是在那里复制我的 twitter feed，这样那些愿意在乳齿象上关注我的人就可以得到一切。为了做到这一点，我使用了 moa.party。你必须给它凭证才能访问你的 Twitter 和乳齿象订阅源，这有点令人担忧，但我的乳齿象感知同事使用它没有问题。*

我不可能把我的证件给第三方！我进一步寻找，发现了这个宝石:

> *这个工具可以同步乳齿象和 Twitter 之间的帖子。你把你的东西贴在哪里并不重要——它会和其他东西同步！*
> 
> *—* [*乳齿象推特同步*](https://github.com/klausi/mastodon-twitter-sync)

它看起来正是我在寻找的！

# 乳齿象推特同步

该工具提供了两个执行选项:

*   码头工人的形象
*   从源头运行—生锈

在 [Docker 图像](https://hub.docker.com/r/klausi/mastodon-twitter-sync/tags)没有标签，保存`latest`，我有一些映射卷的问题。因此，我决定逃离源头。还是那句话，我懒，不想手动运行工具。几年来，我一直使用 GitHub Actions 来安排我的脚本。

先从下面说起:

```
name: Sync Twitter to and from Mastodon
on:
  schedule:
    - cron: "24 */2 * * *"                               #1
  workflow_dispatch:
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Check out the synchronization code         #2
        uses: actions/checkout@v3
        with:
          repository: klausi/mastodon-twitter-sync
      - name: Install Rust                               #3                            
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          profile: minimal                               #4
      - name: Execute synchronization                    #5
        uses: actions-rs/cargo@v1
        with:
          command: run
          args: --release
```

1.  每两小时安排一次，在整点后 24 分钟
2.  签出同步项目的代码
3.  安装防锈工具链
4.  工具链有不同的风格，称为[概要文件](https://rust-lang.github.io/rustup/concepts/profiles.html)。对于脚本来说，`minimal`就足够了，只提供`rustc`、`rust-std`和`cargo`
5.  运行代码

# 管理凭据

剧透:工作流程不行。默认情况下，代码以交互方式运行:它将要求凭据来连接 Twitter 和 Mastodon。或者，项目接受一个包含所有数据的配置文件— `mastodon-twitter-sync.toml`。

我的建议是在本地以交互方式运行该项目一次。如果 TOML 文件不存在，可执行文件将要求凭证并生成一个包含凭证的新凭证。但是我们不应该在 Git repo 上添加包含纯文本凭证数据的文件。相反，我们应该:

1.  加密文件
2.  添加并提交加密文件
3.  在工作流运行期间，使用 GitHub 操作密码解密文件

```
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Install GPG to decrypt the configuration file
        run: sudo apt-get update && sudo apt-get install -y gnupg
      - name: Decrypt the configuration file
        run: gpg --quiet --batch --yes --decrypt --passphrase="$GPG_PASSPHRASE" --decrypt mastodon-twitter-sync.toml.gpg > mastodon-twitter-sync.toml 
        env:
          GPG_PASSPHRASE: ${{ secrets.GPG_PASSPHRASE }}
```

此时，我们已经将 Rust 源代码与我们的配置文件混合在同一个 Git 存储库中。处理这样的项目涉及到很多`git rebase`，我想避免。让我们用本地专用的生命周期保持代码独立。

```
mastodon-twitter-sync-job               #1
|_ .github
|  |_ workflows
|    |_ sync.yml                        #2
|_ mastodon-twitter-sync.toml.gpg       #3

mastodon-twitter-sync                   #4
|_ src
|_ ...
```

1.  我的项目
2.  GitHub 操作
3.  加密的凭据文件
4.  独立同步项目

我们需要改变检查代码的方式:

```
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo itself
        uses: actions/checkout@v3
        with:
          path: job
      - name: Check out the synchronization code
        uses: actions/checkout@v3
        with:
          repository: klausi/mastodon-twitter-sync
          path: code
```

当我们运行工作流时，布局如下:

```
|_ job
|  |_ .github
|  |  |_ workflows
|  |    |_ sync.yml
|  |_ mastodon-twitter-sync.toml.gpg
|
|_ code
|  |_ src
|  |_ ...
```

从今以后，我们应该更新解密并相应地运行这些步骤:

```
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Decrypt the configuration file
        run: |
          gpg --quiet --batch --yes --decrypt --passphrase="$GPG_PASSPHRASE"
              --decrypt job/mastodon-twitter-sync.toml.gpg > mastodon-twitter-sync.toml #1
        env:
          GPG_PASSPHRASE: ${{ secrets.GPG_PASSPHRASE }}
      - name: Execute synchronization
        uses: actions-rs/cargo@v1
        with:
          command: run
          args: --manifest-path=./code/Cargo.toml --release                             #2
```

1.  从当前根文件夹中的`job`子文件夹解密
2.  使用`code`子文件夹在当前文件夹中运行

# 仅同步一次

该项目创建一个包含所有以前同步的内容的`post_cache.json`文件，以避免在每次执行期间复制相同的内容。我们需要考虑到:

```
jobs:
  sync:
    runs-on: ubuntu-latest
      - name: Update post cache
        run: >
          cp ./post_cache.json ./job/ 2>/dev/null || :    #1
      - name: Commit and push post cache
        uses: EndBug/add-and-commit@v7                    #2
        with:
          cwd: './job'
          add: post_cache.json
          default_author: github_actions
          message: Update post cache
```

1.  将`post_cache.json`复制到`job`子文件夹中。仅当作业不同步任何内容且文件已生成时，才成功执行该步骤。
2.  如果文件已更改，则提交回文件

# 工作流程优化

在当前状态下，每次运行都下载依赖项并编译项目，即使源代码仍然是代码；效率非常低。

该平台提供了一个通用的[缓存 GitHub 动作](https://github.com/marketplace/actions/cache)。然而，我发现了 [rust-cache](https://github.com/Swatinem/rust-cache) ，一个 rust 特有的缓存，它为 Rust 提供了适当的默认值。让我们使用它来缓存工作流执行之间的依赖关系和可执行文件(假设一些参数保持不变):

```
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Install Rust
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          profile: minimal
      - name: Cache executable             #1
        uses: Swatinem/rust-cache@v2
        with:
          workspaces: code                 #2
```

1.  必须在 Rust 安装后安装，因为缓存密钥包含特定于 Rust 的数据
2.  缓存位于`code`子文件夹中的工件

# 最终注释

有了这个设置，在提交对工作流的任何更改之前，我需要用新的 JSON 缓存文件更新 repo。我可以为它创建一个专门的回购来改善这种情况，但目前来说已经足够好了。

与乳齿象的联系是易变的；许多操作都会失败，并显示以下消息:

```
Error connecting to Mastodon: Http(
    reqwest::Error {
        kind: Request,
        url: Url {
            scheme: "https",
            cannot_be_a_base: false,
            username: "",
            password: None,
            host: Some(
                Domain(
                    "mastodon.top",
                ),
            ),
            port: None,
            path: "//api/v1/accounts/verify_credentials",
            query: None,
            fragment: None,
        },
        source: TimedOut,
    },
)
```

这不是一个问题*本身*；这只是意味着同步滞后。我应该转移到更可靠的实例，甚至托管自己的实例吗？

到目前为止，我一直把 Twitter 作为我的真理来源。我在那里发布内容，它应该会出现在乳齿象上。然而，同步应该是双向的。一旦我把乳齿象作为我的主渠道，我就不需要改变上面的工作了。

# 结论

Twitter 的新主人声称要推广“喜剧”,但暂停了取笑他的账户。与此同时，他声称自己是言论自由的支持者，但却把观点和信息混为一谈。广告市场可能会抑制他被误导的观点，但它仍然是确定的。

同时，我也不愿意无所事事。乳齿象的势头越来越大。在这篇文章中，我解释了如何跨越鸿沟，同时保持你在 Twitter 上的存在，直到你不想这样做。感谢 klausi 的奇妙同步项目和对我的失误的耐心。

您可以找到它的源代码

GitHub 上提供了源代码:

[](https://github.com/nfrankel/mastodon-twitter-sync-job) [## GitHub-nfrankel/乳齿象-Twitter-同步-作业

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/nfrankel/mastodon-twitter-sync-job) 

**更进一步:**

*   [乳齿象上的我](https://mastodon.top/web/@frankel)
*   [马丁·福勒的乳齿象历险记](https://martinfowler.com/articles/exploring-mastodon.html)
*   [乳齿象文献](https://docs.joinmastodon.org/)
*   [恐鸟桥(小心！)](https://moa.party/)
*   [乳齿象推特同步](https://github.com/klausi/mastodon-twitter-sync)

*原载于* [*一个 Java 极客*](https://blog.frankel.ch/move-away-twitter/)*2022 年 12 月 11 日*