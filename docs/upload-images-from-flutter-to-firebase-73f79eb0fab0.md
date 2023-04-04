# 将图片从 Flutter 上传到 Firebase

> 原文：<https://blog.devgenius.io/upload-images-from-flutter-to-firebase-73f79eb0fab0?source=collection_archive---------5----------------------->

![](img/a8cf4c80a16205781dbb7c2f98d91b2b.png)

颤动+火焰基

如果你想上传图片到 [Firebase](https://firebase.google.com) ，那么这篇文章就是为你准备的！在这里，您将了解如何将 Firebase 安装到您的 Flutter 应用程序，以及如何将图像上传到 Firebase。在我们开始使用 Firebase 之前，我们必须创建一个新的 Flutter 项目，并安装我们将要使用的所有包。您可以复制并运行该命令:

```
flutter create flutter_firebase && cd flutter_firebase && flutter pub add image_picker && flutter pub add firebase_core && flutter pub add firebase_storage && flutter run
```

现在我们的 Flutter 应用程序已经创建，我们将设置 Firebase。为此，你应该去 Firebase.com 的[为自己创建一个新项目。之后，你应该在你的 Flutter 应用程序中添加 Firebase。有一个很棒的视频，你应该看看，就是这个:](https://console.firebase.google.com/u/0/)

非常感谢 Firebase 制作了这个视频，它对我帮助很大👏

当你把 Firebase 添加到你的 Flutter 项目中之后。在 Firebase 控制台中，我们还需要做一件事来确保我们可以上传图像。这是编辑我们的存储规则，我们之所以需要这样做，是因为我们没有对用户授权访问 Firebase。因此，您应该转到 Firebase 中的存储，然后转到规则。接下来，我们可以将代码更改为:

对于我们的 iOS 应用程序，我们必须对我们的 Info.plist 进行一些更改，这是必要的，因为我们正在使用我们的 Android**image _ picker**包，您不必再做任何事情。因此，打开文件 iOS/Runner/Info.plist，将这些键添加到其中:

请注意，这种语言是 XML，这是因为。plist 对于 Github gist 来说只是陌生

终于！一切都已正确安装，我们可以开始编码。因为我们使用 Firebase，所以在运行我们的应用程序之前，我们需要首先初始化它，所以让我们打开 **main.dart** 文件，删除所有已经预编码的内容。我使用的代码很容易理解，请看:

你可能会得到一些错误信息，这是因为我们没有创建一个 **MyHomePage** 小部件。我们需要创建一个名为 **homepage.dart** 的新文件来创建这个小部件。然后我们将创建一个名为 **MyHomePage** 的新 StatefulWidget，这将是我们上传图片的页面。让我们首先开始布局，这将是一个非常基本的布局。

这就是我们的应用程序的外观，现在你可能无法运行这个应用程序，因为我们需要创建一个名为 **getImage()** 和 **uploadImage()** 的函数。我们将首先创建函数 **getImage()** 这是一个函数，我们在本地存储上传的图像，并在这里显示选中的图像。如果您想上传照片，您可以更改图像来源，这完全由您决定:

现在我们已经选择了一个图像，我们将把它上传到 Firebase。我们还会给图片取一个随机的名字，这样我们就可以上传尽可能多的图片。函数 uploadImage()将如下所示:

这是我们的应用程序需要做的最后一件事。如果您有任何问题，请随时问我，您也可以查看我的 GitHub 资源库:

[](https://github.com/turboteun2/flutter_firebase) [## GitHub-turboteun 2/flutter _ firebase:用 flutter 上传图片到 fire base

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/turboteun2/flutter_firebase)