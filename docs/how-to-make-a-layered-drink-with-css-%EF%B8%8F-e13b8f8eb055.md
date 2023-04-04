# 如何用 CSS☕️制作分层饮料

> 原文：<https://blog.devgenius.io/how-to-make-a-layered-drink-with-css-%EF%B8%8F-e13b8f8eb055?source=collection_archive---------14----------------------->

如果你最近在 Medium 上，你可能会看到这个代码笔:

也许你和我一样，认为“我能做到！这将是一个简单快捷的小项目。”只是看了一眼 SCSS，不知道从哪里开始，也不知道如何用普通的 CSS 重新创建它。嗯，我已经做到了。这将是对 CodePen 的一个松散的改编。

## 第一步:用 HTML 制作杯子

好的，让我们从基础开始。如果你想制作多个不同内容的杯子，我们将需要使用道具。在这种情况下，我们将使用 itemName 作为名称，itemContents 作为饮料本身，itemIngredients 列出饮料的成分。类名必须是 itemContents，这样我们就可以在 CSS 中制作不同的饮料，并将该名称作为道具传递到卡片中。

![](img/009351187ec4cc35dbb980da08cb131e.png)

## 第二步:给杯子添加样式

现在我们需要使用 CSS 制作杯子。我把我的做成 150px 高，150px 宽，但是你做你的。边框风格需要立体。选择边框的颜色。我使我的边界半径为 5% 5% 30% 30%，但是你可以随意调整以适应你的需要。我的边框宽度设置为 0.5 雷姆，你可以随意使用。您的代码应该如下所示:

![](img/8d06aa670251cba0379644e0ce27c9ce.png)

你的杯子应该是这样的:

![](img/8181e6646a8728f0488e7d6eb57ad0cf.png)

## 第三步:调用卡/杯，并填写道具

在这里，我们将 itemName、itemIngredients 和 itemContents 传递到我们制作的卡片中。

![](img/092b67fe2b42657ddebdca2cb3ca6b6a.png)

## 第四步:图层时间！！

好了，现在我们可以开始构建这种饮料的层次了。首先我们调用饮料的类名(应该等于 itemContents)。我选拿铁。背景图像需要是线性梯度。旋转渐变 0 度，让它看起来像分层的饮料。根据你制作的饮料种类，你需要调整颜色的比例。对于拿铁，我让 expresso 色(#4d211d)从 0%到 40%，蒸奶色(#f0eada)从 40%到 90%，最后奶泡色(#fdfdfd)从 90%到 100%。玩背景尺寸，直到层适合杯子，134px 为我做到了。确保杯子和饮料上的边框半径匹配，否则看起来不像是杯子里的饮料。我还把高度设置成比杯子短 20px，宽度少 20px(130 px 给今天不想做数学的人)。我把我的饮料放在杯子中央，边距为 10px，右边边距为 10px。您的代码应该如下所示:

![](img/bfc11f063883abde2a20f5fc038ec585.png)

饮料应该是这样的:

![](img/5f521c51bc795188589bf028d9152045.png)

我希望这有所帮助。如果我有什么不清楚的地方，我的 GitHub 会链接到下面，还有一个 YouTube 演示，展示你可以用它做的不同变化。编码快乐！👩‍💻

YouTube 演示:[https://youtu.be/1YZ0V0PKmq8](https://youtu.be/1YZ0V0PKmq8)

GitHub 回购:【https://github.com/ReaganADavenport/hummingbird-cafe 