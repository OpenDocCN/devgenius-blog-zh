# Rust 中 GTK4 小部件的事件处理

> 原文：<https://blog.devgenius.io/event-handling-for-gtk4-widgets-in-rust-d3c3f89b092f?source=collection_archive---------6----------------------->

![](img/be5befd5fb9bffcfd9fb3f86a6db2f63.png)

Johannes Plenio 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们已经完成了这个[教程](/initial-setup-for-a-gtk4-app-with-libadwaita-in-rust-using-vscode-b6f8c127a75e)的最后一部分。在构建我们的应用程序时，我们探索了桌面 UI 编程的许多概念，这个应用程序是一个普通的 RSS 阅读器，目前甚至不读取 RSS 数据。一些概念更复杂，另一些更容易理解，但所有这些概念就像一组齿轮一样一起工作，一个接一个地连接起来，产生一个完全工作的机器。你不能有一个概念而没有另一个，否则，整个事情会变得更加复杂和丑陋。代码将更难重构和维护。我希望通过阅读这篇教程，你能对 GTK 和所有桌面 UI 框架有所了解。

好了，上次我们构建了[数据模型](/using-models-to-bind-data-to-gtk4-custom-widgets-in-rust-379dd9d1bf4d)来加载我们的 UI 列表。这很重要，因为它们允许组件和数据之间的解耦，这是一条通向 MVC 的曲折道路。但是我们不会纠缠于 MVC，也不会纠缠于它的 merrits。我们要做的是朝着我们的目标前进一点:在我们的 UI 上实现用户交互。我们有我们的[列表](/refactoring-gtk4-ui-templates-in-rust-68cbef1a1778)，我们有我们的[响应](/using-the-libadwaita-leaflet-widget-for-a-responsive-gtk4-ui-in-rust-73bbc2f4025) UI，让我们处理鼠标点击。每当我们在左侧窗格中选择一个提要时，我们都希望用与所选项相关的数据重新加载右侧窗格。代码一如既往地可以在 [github](https://github.com/raduzaharia-medium/gtk-rss-reader-signals) 上获得，你可以随意查看、派生和使用。开始了。

## GTK 和拉斯特的信号

![](img/1b5a6038c56b23e9be833fccf75299e5.png)

我们的 RSS 阅读器应用程序

在 GTK，一个事件被称为信号，通过连接到信号来处理事件。信号的问题是它们可能发生在不同于主窗口的线程上，这涉及到一些管道问题，因为 GTK 不是线程安全的。不具备线程安全性意味着您必须自己进行检查和锁定，以确保应用程序数据的完整性。

但我们也在使用 Rust，它会进行自己的检查，并需要自己的保证，确保变量不会消失或在它们的作用域之外被使用，因此这项工作更加繁琐。幸运的是，拉斯特和 GTK 都提供了一些技巧来帮助我们，稍后我们会看到。

我们的信号工作流程是这样的:我们点击了定制部件中的一个项目，但是这个点击实际上转化为点击了里面的部件。所以我们需要处理`ListBox`点击，并把它冒泡到`FeedList`。我们还想发送一些与事件相关的信息，这样`FeedList`就会知道哪个条目被点击了。

## 处理列表框单击事件

![](img/ec016cb2e7153b5e5b89bfaeb170a496.png)

应用程序的更新结构

我喜欢发布应用程序目前在 VSCode 中的截图，因为它让您了解我们的开发进度。如您所见，我们需要对`template.rs`文件中的`FeedList`小部件进行更改。我们需要为`FeedList`创建一个自定义信号来报告项目选择，我们需要处理`ListBox`项目点击。为了将事件添加到 FeedList，我们将`signals()`函数添加到它的`ObjectImpl`:

```
impl ObjectImpl for FeedListTemplate {
  fn properties() -> &'static [ParamSpec] {
    static PROPERTIES: Lazy<Vec<ParamSpec>> = Lazy::new(|| {
      vec![ParamSpecBoolean::new(
        "show-end-title-buttons",
        "show-end-title-buttons",
        "Shows the title buttons in the header bar",
        true,
        ParamFlags::READWRITE)]
      }); PROPERTIES.as_ref()
  } ... fn signals() -> &'static [Signal] {
    static SIGNALS: Lazy<Vec<Signal>> = Lazy::new(||
      vec![
        Signal::builder("changed", 
          &[String::static_type().into()], 
          <()>::static_type().into()).build()]); SIGNALS.as_ref()
  }
}
```

您可以看到定义信号和定义属性之间的相似性。我们为我们的信号使用了方便的`SignalBuilder`,我们给它一个名称、一个参数列表和一个返回类型。参数列表定义了信号将向其处理程序传送什么数据。发送选定的项 id 和其他信息有助于事件处理程序做进一步的工作。在我们的例子中，我们将发送所选 RSS 提要的名称。这样，事件处理程序将知道点击了什么，并将从正确的 URL 加载 RSS 内容。

我们为`FeedList`定义了信号，因此它可以报告它的选择被改变了，但是现在我们还需要在用户点击包含的`ListBox`中的项目时调用该信号。我们在`constructed()`函数中对此进行了设置:

```
fn constructed(&self, obj: &Self::Type) {
  self.parent_constructed(obj); self.list_box.connect_row_selected(|source, selected_row| {
    let selection = selected_row.unwrap();
    let feed_list = source.parent().unwrap().parent().unwrap();
    let name: String = selection.property("title"); feed_list.emit_by_name::<()>("changed", &[&name.clone()]);
  });
}
```

除了调用`parent_constructed`函数，我们还添加了一些新的东西。我们为我们的`ListBox`的`row_selected`事件分配一个处理程序。所以每次在`ListBox`中选择一个项目，分配的代码就会运行。默认情况下，所有信号都将`source`作为信息发送。并且`row_selected`信号将`selected_row`作为额外信号发送。这就是为什么我们的事件处理程序把两者都列为参数。

事件处理程序简单地从`ListBox`中获取选中的行，同样:方便地通过信号发送给我们，通过导航部件树获取`feed_list`并对其调用`emit_by_name`。这指示我们的`FeedList`发出我们刚刚用`signals()`函数定义的信号。我们发出`changed`信号，并将其作为参数发送给选定的提要名称。很简单。

## 处理 FeedList changed 事件

![](img/03ca50d64b85cd3e1129052513295e01.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

`ListBox`发送它的信号，`FeedList`捕捉它并发送它自己的:`changed`。谁会捕捉到`changed`信号？谁能用它做任何事呢？谁能够将数据模型分配给`FeedList`和`ArticleList`？正如我们在[上一篇文章](/using-models-to-bind-data-to-gtk4-custom-widgets-in-rust-379dd9d1bf4d)中看到的，它的`MainWindow`:

```
impl ObjectImpl for MainWindowTemplate {
  fn constructed(&self, obj: &Self::Type) {
    self.parent_constructed(obj); let feed_model = vec![
      FeedItem::new("The Verge", "https://www.theverge.com/rss/index.xml"),
      FeedItem::new("Ars Technica", "https://feeds.arstechnica.com/arstechnica/features"),
      FeedItem::new("Hacker News", "https://news.ycombinator.com/rss"),];
    self.feed_list.set_model(feed_model); let article_model = vec![
      ArticleItem::new("The Verge - Article 1", "Article 1 summary placed in a handy label widget"),
      ArticleItem::new("The Verge - Article 2", "Article 2 summary placed in a handy label widget"),
      ArticleItem::new("The Verge - Article 3", "Article 3 summary placed in a handy label widget"),
      ArticleItem::new("The Verge - Article 4", "Article 4 summary placed in a handy label widget"),];
    self.article_list.set_model(article_model);
  }
}
```

我们当时做的是给`FeedList`和`ArticleList`加载一些随机数据，只是为了测试我们的数据模型。我们现在要做的是，每当用户点击一个`FeedList`项目时，用一些随机数据加载`ArticleList`但是与`FeedList`选择相关。那么让我们来处理一下`FeedList`事件:

```
impl ObjectImpl for MainWindowTemplate {
  fn constructed(&self, obj: &Self::Type) {
    self.parent_constructed(obj);

    let feed_model = vec![
      FeedItem::new("The Verge", "https://www.theverge.com/rss/index.xml"),
      FeedItem::new("Ars Technica", "https://feeds.arstechnica.com/arstechnica/features"),
      FeedItem::new("Hacker News", "https://news.ycombinator.com/rss")];
    self.feed_list.set_model(feed_model.clone()); let feed_model_rt = Rc::new(feed_model.clone());
    let article_list_rt = Rc::new(self.article_list.clone()); self.feed_list.connect_local("changed", false,
      clone!(
        @strong feed_model_rt, 
        @strong article_list_rt => move |values| { let value: String = values[1].get().unwrap();
       let selection = feed_model_rt.iter().find(
         |x| x.property::<String>("name") == value).unwrap();
       let feed_name:String = selection.property::<String>("name");
       let feed_url:String = selection.property::<String>("url");

       let article_model = vec![
         ArticleItem::new(&format!("{} - Article 1", feed_name), &format!("Article 1 from {}", feed_url)),
         ArticleItem::new(&format!("{} - Article 2", feed_name), &format!("Article 2 from {}", feed_url)),
         ArticleItem::new(&format!("{} - Article 3", feed_name), &format!("Article 3 from {}", feed_url)),
         ArticleItem::new(&format!("{} - Article 4", feed_name), &format!("Article 4 from {}", feed_url)),]; article_list_rt.set_model(article_model);
       None
     }),
   );
  }
}
```

我们仍然创建`FeedItem`模型并像往常一样分配它，然后我们使用`connect`函数来处理`changed`事件。请注意，因为这是一个自定义事件，我们没有`connect_changed`函数，我们使用通用的`connect`，我们还注意到了`connect_local`变量。这个变体强制在本地线程上处理`changed`事件。正如我们之前说过的，GTK 不是线程安全的，不幸的是，事件是在独立于主窗口的线程上触发的。当我们只是将数据从信号发送到处理程序时，这基本上没问题，但是在我们的例子中，我们想要做更多。为了摆脱数据封送和其他困难的 UI 和线程概念，我们使用`connect_local`在本地线程上处理事件。

但这还不够。在我们的处理程序中，我们从信号中得到一个提要名称，但是我们还需要存储在`feed_model`中的提要 URL。我们无法访问它，因为我们处于关闭状态。或者更确切地说，虽然它是可访问的，但不能安全使用。Rust 知道这一点，并设置了许多检查，迫使我们在`feed_model`上使用`Rc`包装器。我们还想给`article_list`分配一个数据模型，所以我们也需要一个`Rc`包装器。

`clone!`宏是一个方便的包装器，允许我们捕获`Rc`包装变量的`@strong`或`@weak`克隆。我不打算开始解释这一切意味着什么，因为它与铁锈的工作原理密切相关。这里唯一需要的是记住我们不能简单地访问处理程序之外的变量。首先是因为螺纹，我们可以通过使用`connect_local`来避免；其次是因为 Rust 处理处理器闭包的方式，使其与其借用机制兼容。例如在 C 语言中，我们不会有这些麻烦，但另一方面，C 语言不会强制任何类型的内存管理。

仅此而已。我们正在处理一个提要选择事件，我们能够根据我们选择的提要来加载文章列表。我们可以说我们的应用程序基本上完成了。不过我不会把它留在这里，下次我们将看到如何真正地[加载 RSS 提要](/using-threads-and-messages-to-load-data-in-a-gtk4-widget-5e1da3b0621d)并在我们的`ArticleList`中显示适当的文章。这也将结束本教程。

在那之前，我希望你今天学到了一些有用的东西，并随时使用评论区来回答我在这里错过的任何问题或进一步的解释。下次见！