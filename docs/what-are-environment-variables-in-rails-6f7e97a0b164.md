# Rails 中的环境变量是什么？

> 原文：<https://blog.devgenius.io/what-are-environment-variables-in-rails-6f7e97a0b164?source=collection_archive---------0----------------------->

![](img/6f44232d730829303a2a1edb6f86eebe.png)

铁轨！

当我学习开发 Rails 应用程序时，我了解到我们应该将应用程序的密钥保存在应用程序的**‘环境变量’**中。Git 不跟踪环境变量，因此它们不会提交给公共存储库。因此，我们的密钥和应用程序都是安全的。

我现在已经开发了许多 Rails 应用程序，每次使用环境变量时，我都会感到有点失落。我知道把它们存储在那里是安全的，但是我不太确定如何设置环境变量，环境变量存在于哪里，以及它们到底是什么。我在一些快速的 google 搜索中读到，环境变量存在于本地系统中，并且在所有应用程序中都可用。这让我对电动汽车既困惑又好奇。

在本文中，我们将讨论如何在本地系统中使用 EVs——如何使用 Bash 终端访问和设置临时和永久 EVs。其次，我们将讨论 Rails 如何访问 EVs。第三，我们如何设置特定于 Rails 应用程序的 EVs(在开发和生产阶段)？

## 基础知识:您的本地系统中有哪些环境变量？

环境变量就是存储在本地系统中的变量，计算机用它来配置应用程序。例如,“LANG”环境变量决定了计算机程序向用户显示的语言。

如果我们打开一个 bash 终端，我们可以通过执行:`printenv`看到系统上现有的环境变量

![](img/0f2a3d92ceec4a28fce0f8115d6f121f.png)

以下是 printenv 返回的前几行！

如果继续向下滚动，可以看到环境变量 LANG，它的赋值为“en_US”。UTF-8”(针对美国的典型计算机)。

我们可以在系统上创建一个新的 env 变量，如下所示:

`export TEST_NAME=hahaha`

我们现在可以在跑`printenv.`的时候看到我们的新电动车了

![](img/39ea882680eee4c5d10daa7745ef5e37.png)

使用`export VAR_NAME=VAR_VALUE` **临时设置一个环境变量值，直到设置它的 shell 会话关闭。**

我们可以不设置临时变量，而是设置**“会话范围”的环境变量，**为某个用户在本地系统中保留的变量。我们可以通过修改 **~/来设置会话范围的 ev。pam_environment** 或 **~/。配置文件**文件。

在这里，我们将使用 **~/。轮廓文件**。 **~/。配置文件**在桌面会话的启动过程中由 DisplayManager 自动执行，因此在加载桌面时，此文件中设置的任何变量都将可用！

1.  我们可以使用`gedit ~/.profile`在编辑器中打开文件

![](img/4383f5af269a26590efcab1bdc6b5907.png)

2.创建一个环境变量，将其添加到打开的文件`export ARTY=i_am_session_wide!`的末尾

![](img/67e9dc3419d8b62624e63b2df933fe6a.png)

3.现在，保存文件并重新登录到我们的用户。执行`printenv`显示了我们新的环境变量`ARTY=i_am_session_wide!`

![](img/b74e9061656adf437b7fbd5f1e49d5ca.png)

现在，我们了解了本地系统中 EVs 的基础:如何查看 EVs，设置临时和会话范围的 EVs。我们还可以设置永久 ev！要查看更多细节，请查看关于在您的操作系统中使用 EVs 的文档(我已经离开了这篇[文章](https://help.ubuntu.com/community/EnvironmentVariables)

# Rails 如何访问本地系统中的环境变量？

当 Rails 应用程序启动时(例如`rails c`或`rails s`命令)，Rails 将本地系统中设置的环境变量加载到一个名为 ENV 的散列中。

1.  创建一个新的 Rails 应用程序和 cd: `rails new test_api — api`
2.  使用`rails c`启动应用程序控制台
3.  查看 Rails 加载的`ENV`散列

![](img/d665ceb75eabe710d6298327a5072876.png)

我们可以看到它是一个散列，包含了我们在终端中执行`printenv`时列出的所有环境变量。它还有其他变量。

**ENV 只是一个基本的 Ruby 哈希**，我们可以像访问常规 Ruby 哈希中的键值对一样访问变量。例如，我们可以用`ENV['LANG']`访问 LANG

![](img/14295e1d808f155001bb6de7b28239df.png)

# 我们如何为 Rails 应用程序设置 Env 变量？？

如果我们愿意，我们可以在 **~/中安全地设置 Rails 应用程序所需的任何变量。profile** 文件，但是这个文件会变得不必要的大，因为它包含了我们开发的每个 Rails 应用程序的密钥和秘密。将应用程序的键设置在本地系统中让所有其他应用程序看到也是一种不好的做法。相反，最好在我们的 Rails 应用程序中完成。

## 设置 Rails ENV 的一个简单方法:Figaro Gem

Figaro 创建了一个添加到**的 YAML 文件。gitignore** 文件(Git 不会跟踪该文件)。当应用程序加载时，任何保存到 YAML 文件的键值对都将被添加到 Rails 的 ENV 散列中。

1.  将 Figaro 添加到您的 gem 文件中`gem figaro` &运行`bundle install`
2.  执行`bundle exec figaro install`

该命令自动创建一个带注释的 **config/application.yml** 文件，并将其添加到您的`.gitignore`中。在这里，我们可以为我们的`ENV`散列指定键/值对。

![](img/305e644537bd2becd0905e7bae904ac2.png)

现在，当我们运行我们的`rails c`时，我们可以访问`ENV[‘my_secret’]`和`ENV[‘my_other_secret’]`。

![](img/c8fcc30a388b42fa64fe1b6bb98a6cfc.png)

## 我们也可以手动设置和加载 ENV 变量！

我认为使用 Figaro gem 要干净得多，但是演示如何手动设置`ENV`会解构这个过程并分解实际发生的事情。

如前所述，`**ENV**` **只是一个散列值**，当 Rails 应用程序加载时，它使用用户本地系统上的 env 变量进行设置。我们可以通过创建一个在应用程序初始化期间运行的文件来手动为 ENV 设置一个键值对。我们还需要确保`**gitignore**`文件，这样它就不会被提交到存储库中。

1.  在`/config`中创建一个 Ruby 文件并设置`ENV`键值。
2.  在`require`和`Rails.application.initialize`线之间的`/config/environment.rb`处装载我们的文件。(`config/environment.rb`在 Rails 启动之前加载应用程序环境，所以这是设置`ENV)`的好地方。
3.  将我们的`/config/test_set.rb`添加到。gitignore 文件。

使用这种方法，我们可以为 Rails 应用程序手动设置环境变量！

# 为什么环境变量是安全的？

我们使用 env 变量，而不是将这些键“硬编码”到我们的应用程序代码中，因为我们希望防止敏感信息被提交到像 GitHub 这样的公共源代码库中。

在本例中，我们对用于创建用户身份验证令牌的应用程序的密钥进行了硬编码。如果我们将这些代码提交并推送到 GitHub 库，公众就可以访问这个密钥。任何人都可以使用公开的密钥为任何用户生成有效的身份验证令牌。我们的应用程序不会非常安全。

开发人员隐藏 Rails 应用程序的密钥和其他应用程序信息的方法是创建一个被 git 忽略的文件(这个文件不会被 Git“跟踪”,也不会被提交/推送到您的源代码库)。当应用程序加载时，该文件会将应用程序需要的任何变量设置到一个名为 ENV (Rails 的环境变量)的散列中，以便以后在应用程序中访问。现在，在应用程序的任何地方，我们都可以使用`ENV[‘secret_key’]`来访问密钥，而不是显式地写出密钥。

# 在部署的应用程序中设置环境变量(Heroku)

最后，因为我们用来设置环境变量的文件在。gitignore，文件只存在于我们的本地目录中！我们如何确保当我们将应用程序部署到 Heroku 时，应用程序能够访问必要的变量？

Heroku 使用简单的命令在系统上设置环境变量:

`$ heroku config:set my_id=1444`

我们还可以使用。我们生成的 yml 文件:

`$ figaro heroku:set`

有关使用 Figaro 设置配置 env 变量的更多信息，请查看文档！[https://github.com/laserlemon/figaro](https://github.com/laserlemon/figaro)

你可以通过进入 Heroku 网站上你的应用程序的“设置”来检查你的 Heroku 系统的环境变量。您还可以在那里为您的应用程序设置 env 变量！

现在，我们部署的应用程序可以访问 env 变量了！！！

## 结论:

环境变量是位于计算机中的变量，计算机的应用程序使用这些变量进行配置。Rails 应用程序中的`ENV`散列是在 Rails 应用程序启动时设置的。Rails 将任何存储在您计算机中的环境变量和您使用 Figaro gem 添加的任何其他键值对加载到`ENV`中。将 Rails 应用程序的秘密信息存储在 ENV 中是安全的，因为实际的密钥隐藏在 Git 忽略的文件中！

感谢阅读！