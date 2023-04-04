# 如何简单地在 Github 分支中设置一个带有可重用字段的表单和自动测试 push？

> 原文：<https://blog.devgenius.io/how-to-set-up-a-form-with-reusable-fields-and-automatic-tests-on-push-in-github-branch-simply-fe4576b05c93?source=collection_archive---------3----------------------->

本教程的灵感来自 Laith Academy 的视频“React Testing Library 速成班”(没有定制钩子或可重用组件)。

为了让教程尽可能贴近现实，我做了这个练习，但是把它放在一个真实的专业背景下。因此，适应→可重用输入(真实世界的用例)和关注点分离的定制钩子。

首先，让我们创建一个 React 应用程序:npx create-React-app automation-testing-React

![](img/aebfe605dc6576ac892487c57fc4addb.png)

现在你有了一个新创建的 react 应用，让我们开始编码吧…

然后，我创建了一个组件“CardContainer ”,简单地赋予表单一些样式。

![](img/2da932253d1b950012894bfd50311246.png)

这个组件有两个参数，children(这将是包含在两个标记之间的代码。)和背景，以便根据所关注的卡放置不同的背景。

然后我创建一个这样的表单组件。

![](img/d5a8c0f02f206e1f8bd3d73be641a907.png)

然后我添加一些 css 来给我的应用添加一些风格。

![](img/ac175743f3113aca7e090ea4df4a4d95.png)

我是这样导入和使用 App.js 中的组件的。

![](img/8e8af2ff24de96adfd483a63eca5bc7b.png)

你有一张标题卡&一张表格卡。

在这个阶段，你有这个…

![](img/b99bab44d3e709177edd324ab10eba06.png)

现在我像这样创建可重用组件“输入”…

![](img/37199d6d00d75b79f2b7febea0e56b31.png)

代码解释:

在这个组件中，有几个参数作为道具，例如:

![](img/6c9e6164441a62700d4cd6e52ddf3b83.png)

-nameLabel:将是字段的名称，
-type:将是字段的类型，
占位符:将是字段中的默认文本，
keyState:将是字段所关注的状态，
setter:顾名思义，这是更改每个输入的状态的 setter，
和 defaultValues:是初始或当前状态

handleChange 函数:

![](img/425d0e1f6a42ef372b765ea06812fe6b.png)

handleChange 函数是动态的，它传播当前状态并将输入的名称和值添加到我们将在表单组件中创建的对象中。

现在让我们修改表单组件！

![](img/49ad015425431e4f6e9c1ee9954b5a38.png)

代码解释:

![](img/2e699515687860df482808cdadcd27bf.png)

我创建了一个状态，用 email、password 和 password2 作为键，并用一个空字符串来初始化每个字段的状态

然后我像这样创建第一个输入

![](img/ea16c6218d7aab77dcfe7f0540119eb9.png)

这一阶段的结果如下所示

![](img/eb7198840f71d5e2e5de165cb09c43bb.png)

现在让我们添加所有必需的字段和按钮:

![](img/453c28aaefd0422756847a8931ee0e04.png)

对于样式，我使用在本练习中声明为 style 的变量。

![](img/959599c194682247e3d3c1a9189cc65d.png)

这一步的结果是:

![](img/63002a8720416ad1e167fd7ecc21733a.png)

创建我们价值观的控制台日志

![](img/b19c6bd6d2e09806979beef020a5de2b.png)

结果…一切都很好！

![](img/1ec8526e2ed2be6b1c8ec2c9c710195f.png)

好了，让我们开始实现一些错误消息

在此之前，我们将使用关注点分离…
我创建了一个自定义钩子，我将其命名为 useForm.js

![](img/24f22f48836f3169abf33953876e9943.png)

首先，我将我的状态移动到自定义钩子中，就像这样……小心地将状态和 setter 导出到一个对象中。

然后，我用从自定义钩子导入的对象(state & setter)替换我的本地状态。

![](img/860100e73405dd02bceb0cd308037291.png)

一切正常，不会改变结果，除了您的州不再是本地的，而是变成了全球的(多亏了自定义钩子)

![](img/aca2620723888f9d8a75076953227eac.png)

现在，让我们在我的自定义钩子中添加一些简单情况下的错误消息。

![](img/63e4052458c21f809abd541328fc7ca5.png)

代码解释:

![](img/507def002c44f4fb960b96c04a2bf5c3.png)

第一步，我为每种类型的消息(电子邮件和密码)创建一个状态

然后，我用 preventDefault 创建了 handleClick 函数，以便不触发链接到按钮所在表单的事件。

![](img/9b4fb0cba7b56fd42778570615341957.png)

然后，我为 email 字段添加了两个简单的条件:
1——如果字段为空，以及
2——如果地址不包含@符号。

![](img/fafede92dc274f874f2a6bb6edc644a6.png)

最后在 handleClick 函数中:
我这次为密码字段创建了另外两个条件，
1 —如果密码或密码 2 字段为空，以及
2 —如果两个密码不匹配。

![](img/058052330cfd70ae64f81fb45e03b4a8.png)

所有这些条件对于本练习来说都是非常简单的情况。

![](img/9fb0cd377b0a40871272d51b4aaa2fd7.png)

不要忘记返回新值和 handleClick 函数，以便在需要它的组件中可以访问它。

然后转到表单文件，然后更改您的自定义钩子导入。

![](img/b8de37ce0938b839c952608be6eed20c.png)

添加代码，使消息如下所示…

![](img/00b036437fa93469d15a4d6b8c3d7de8.png)

代码解释:

![](img/1118f779d56f76d84537f96e42012d88.png)

如果关于 email 字段的错误消息不是空字符串，那么给我一个包含消息的段落(在自定义钩子中出现的条件中的那个)

![](img/a91d8db54beab1fe57491c69161a5334.png)

密码字段也是如此

最后，像这样给按钮添加 handleClick 函数。

![](img/8f873dd6f4ac0c8676ef4a0bb0914b56.png)

现阶段的结果是…

![](img/5742d8587ff3fb38c8ae8f698b3aa33a.png)![](img/8aa37bac4e1e35a26768016259e67c2b.png)

如果我点击提交按钮时该字段为空

![](img/41b4d43472f8977a6e28d150748f1aab.png)

如果我点击提交按钮时密码字段为空

对于其他情况…

![](img/67632a29ea4c9773dcb1a8e791f71e0d.png)![](img/ef90c3676fa7b5c4a76fa34c6f47bbb2.png)

如果电子邮件不包含@符号

![](img/23b3628a28d2e2acb847bf780e651b06.png)

如果密码不一样。

好的，一切都如预期的那样工作

现在让我们实施第一批测试…

如果您不知道 react 测试库如何工作，请参见 React 测试库文档以了解更多信息:
[https://testing-library . com/docs/React-testing-library/intro/](https://testing-library.com/docs/react-testing-library/intro/)

转到 App.test.js 文件
默认情况下你有这个内容:

![](img/b126237fbf851c9a05fa19211749a562.png)

将内容替换为以下内容:

![](img/c4e68302ae7969ba463727acfba5d68b.png)

代码解释:

所有的测试都以相同的方式实现:
一个“test”函数，它有两个参数，一个描述测试的字符串，后跟一个回调。

![](img/19bd9596394025bcdebe62794852da03.png)

在这个回调函数中，您有 render 方法，在这里您可以选择想要测试的组件。

*   在这个测试中，我们将测试 App 组件。

我确保表单组件由应用程序简单地呈现。

我定义了一个函数“FormComponent ”,并通过它的 testid 请求它，

testid 以这样的组件形式实现(这个值对于测试识别相关组件是必需的):然后 expect 方法接受我的变量，我对它应用 toBeInTheDocument()方法。

![](img/8b5674c127363549f249ed553bee40eb.png)

第一个测试的结果是通过:

![](img/9ed8842b6444c53efe6269747256f8fe.png)

让我们创建第二个测试套件。

您会注意到，一旦创建并保存了文件，测试就会再次实时运行，这将在测试期间生成一个错误，因为它尚未实施。

创建名为 input.test.js 的文件

![](img/c4a9b391bbafb7a42887369d71623159.png)

代码解释:

对于此测试，测试的是输入组件。
我创建了变量 inputElements，我选择通过 getByRole 的角色定位元素，所有输入字段的角色都是“textbox”。然后 expect 方法获取字段的值，我用一个空字符串应用 toBe 方法，因为所有输入在呈现时都必须为空。

结果，2 项测试通过:

![](img/1c8456efa9e92d9bc50991b3366c146f.png)

好了，接下来的测试…

创建 form.test.js 文件

在这一系列测试中，我将从模拟电子邮件字段的操作开始……
测试是否应该能够键入电子邮件:

![](img/853ae4cf36c2aee9266b98bb2be904e7.png)

代码解释:

对于这个测试，测试的是表单组件。
我创建了变量 emailComponent，我选择通过带有标签字段名称的 getByLabelText 标签来定位元素，然后我确保它存在并且字段为空。

然后，我使用 userEvent.type 在字段中输入电子邮件地址，然后确保该值就是输入的值。

第三个测试的结果是:

![](img/b3dba249b38130b8ba1c9e52326bb93b.png)

所有测试都通过了，太好了！

然后，我在字段 password 和 password2 上创建了另外两个测试

![](img/d1047f0ae41e340adbf4501e964bda8f.png)

结果是:

![](img/78a02e36447f2b83f4e249cbe964f62f.png)

3 个测试套件通过(每个文件是一个套件)
5 个测试通过(所有测试分为 3 个文件)

我们将在这里停止测试(如果你好奇的话，回购协议中还有其他更复杂的测试)，我们将通过 github action 继续自动化，因为这篇文章有点长…

转到您的 github 存储库，为该应用程序创建一个 repo。

![](img/9d78c20b74692840c93bd9c835924150.png)

点击回购上的行动按钮:

![](img/330c86a2002744196fbda3c241667713.png)

选择节点

![](img/de063062b48182e267e6b10e75059d6d.png)

用自动生成的内容创建文件(对于这个例子，我将保留默认内容)

![](img/ff7c264ab639fbb6c2637e5ff3947e66.png)

开始提交:

![](img/42e4ef8d865cd55b39a3884ae4c2d2e4.png)

让 git 拉你的本地应用

![](img/5023fc0fea1a220f6be8fc8996f9d390.png)![](img/c1742ad0a088952b30140866bd51dfa1.png)

我做了一点修改(添加了标题教程)

![](img/7cc23c42498a19462dca3a09fd6d6246.png)

所有测试在本地都正常

在 repo 上推送您的代码后，github 工作流开始…

![](img/2efd09580de9ff1b9178d3b412e0befd.png)![](img/80dddfce00adfa07187dab2442566da5.png)

所有的步骤都很顺利。

该应用程序已使用 node 12、14 和 16 自动构建和测试，并将使用您的每次推送进行测试…
不同的测试套件都已成功通过。

就这样，这是这篇文章的结尾，如果你更喜欢它，我会写第二篇文章，介绍如何进行更复杂的测试，以及如何在你的应用程序中组织你的测试文件。

包含本文代码的存储库:

[https://github.com/rodolphe37/auto-test-react](https://github.com/rodolphe37/auto-test-react)

包含代码的存储库(加上一些额外的测试)。

[](https://github.com/rodolphe37/automation-testing-react) [## GitHub-rodolphe 37/自动化-测试-反应

### 如何测试你的可重用输入组件的小例子(测试每个输入是否为空，电子邮件和密码错误…

github.com](https://github.com/rodolphe37/automation-testing-react) 

感谢你阅读这篇文章。