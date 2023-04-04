# 将 Rust 集成到 Android 开发中——最终部分

> 原文：<https://blog.devgenius.io/integrating-rust-with-android-development-final-part-7c752a7cab1d?source=collection_archive---------3----------------------->

![](img/ea9eea56cc894629de2be4022296655a.png)

这是本系列的第二部分，也是最后一部分，它解释了如何将 **Rust** 代码添加到您的 android 项目中，而无需在您的项目中学习或使用 JNI 或 C。在 [**系列的第一部分**](/integrating-rust-with-android-development-ef341c2f9cca) 中，我们创建了一个简单的 Android Studio 项目和一个用 Rust 编写的简单库来记录信息。在第二部分，也是最后一部分，我们将通过一个实际的例子来应用 Rust 函数。

在本演练中，我们将创建一个简单的计算器应用程序，它在用户按下“等于”按钮时计算结果。应用程序将接受 2 个输入，然后对输入执行 3 个操作:加、减和乘。这一简单的知识在现实生活中会很有用，在现实生活中，您可能需要在 Android 上执行耗时或低级的操作。没有进一步的工作，让我们创建我们的应用程序的后端(您可以最初设计 U.I，但是对于这个演练，我觉得首先编写 Rust 后端更方便)

## 设计后端⚙️

如前所述，我们将创建一个保存 2 个值的`struct`,并创建返回相应的加法、减法和乘法的函数。我们会把这个`struct`加到`lib.rs`上。

方法参数中的`Self`是方法接收类型的*语法糖*，也就是该方法所在的`impl`的类型，在本例中，UIit 是`Inputs`。您可以用`Inputs`替换 Self，代码会运行良好。

记住`#[generate_interface]`来源于`[**rifgen**](https://crates.io/crates/rifgen)` 机箱，如系列第一部分[所述。](/integrating-rust-with-android-development-ef341c2f9cca)

现在让我们实施 U.I

## 设计 U.I📱

如前所述，U.I .将有 2 个输入文本字段和 3 个输出文本字段。这是一个简单的布局来说明:

就是这样！这就是[](https://github.com/Kofituo/RustApplication/blob/main/app/src/main/java/com/example/rustapplication/Illustration.jpg)**应用应该有的样子。这是上面的 gif 图**

**每当用户点击“等于”按钮时，来自文本框的数字被传递给写在`Rust`中的结构。然后计算这些值，结果显示在文本框中。请随意修改代码，并尝试其他方式向您的 android 项目添加一些`Rust`变化。**

## **后续步骤👣**

****如果你在尝试这个演练时遇到了任何问题或者有任何疑问，请随时通过** [**Upwork 联系我。如果你正在做自己的项目，你需要帮助，你可以通过 Upwork**](https://www.upwork.com/freelancers/~0196d30a485de56f48) 联系我！**

## **[我还编辑图像(修图、合成、图像增强等。)所以随时联系我！](https://www.upwork.com/freelancers/~0196d30a485de56f48)😄😄**

## **进一步阅读📖**

**了解有关本演练中使用的依赖项的更多信息:**

*   **[**Dushistov/flapigen-rs:用 Rust 编写的程序或库与其他语言连接的工具**](https://github.com/Dushistov/flapigen-rs)**
*   **Kofituo/rifgen:把用 Rust 编写的库翻译成接口文件的程序。它与 flapigen 和一起工作**
*   **[sfackler/rust-JNI-sys](https://github.com/sfackler/rust-jni-sys)**

## **[储存库](https://github.com/Kofituo/RustApplication)**