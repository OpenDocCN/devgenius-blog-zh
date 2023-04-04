# 用 JavaScript 增强 HTML 音频标签

> 原文：<https://blog.devgenius.io/enhancing-html-audio-tag-with-javascript-ff4b5fce1b55?source=collection_archive---------12----------------------->

![](img/05cfa641700ca51aec54567b00249a20.png)

弗洛里安·奥利佛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

HTML 中的音频标签允许将音频嵌入到网页中，并根据它提供的功能提供一些选项。例如，您可以启用控件，以便用户可以更改音量、播放和暂停等。您可以让它自动播放，或者(假设控件已启用)由用户来播放。这是一件非常简单的事情。

通常你会在 HTML 中实现它，就像这样。

> <来源 src = " media/song 1 . MP3 " type = " audio/mpeg ">
> </audio>

您还可以提供多个源标签，它将播放第一个有效的标签。

> <源 src = " media/song 1 . MP3 " type = " audio/mpeg ">
> <源 src = " media/song 2 . MP3 " type = " audio/mpeg ">
> <源 src = " media/song 3 . MP3 " type = " audio/mpeg ">
> </audio>

在该示例中，如果歌曲 1 和歌曲 2 不存在，但歌曲 3 存在，则音频元素将加载歌曲 3。如果歌曲 1 没有，但是歌曲 2 和歌曲 3 有，那么它将播放歌曲 2。它只播放第一个有效的。

事实上，我们可以为一个音频标签指定多个源，这对于我们来说是一个优势，如果我们想增强音频控件的功能的话。

![](img/bcd11e3a8d8720c8a4c4fc0b4ed376fb.png)

布鲁斯·马尔斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 增强音频标签

因此，让我们利用多个源标签和一些简单的 JavaScript 来让音频标签播放我们提供的歌曲播放列表。

首先为您想要的可用音频设置带有多个源标签的音频标签。

> <来源 src = " media/song 1 . MP3 " type = " audio/mpeg ">
> <来源 src = " media/song 2 . MP3 " type = " audio/mpeg ">
> <来源 src = " media/song 3 . MP3 " type = " audio/mpeg ">
> <来源 src = " media/song 4 . MP3 " type = " audio/mpeg ">
> <来源 src

在这个例子中，我有 5 首歌曲，但是如果你愿意，你可以有 10 首、20 首或数百首。

我们要做的是编写一个简单的脚本，这样一旦音频元素播放完音频，它就会自动移动到列表中的下一个。所以，它播放歌曲 1，然后移动到歌曲 2，3，4，5。一旦歌曲 5 结束，它将跳回起点，并再次从头开始。

在 out HTML 中，就在

*musicSources* 将是我们在音频标签下的那些资源的数组。

*musicControl* 将引用音频元素本身。

musicIndex 将只是简单地指我们正在寻找的特定音乐来源。

现在，让我们创建一个函数，我们将在最后调用它来定义音频控件的行为。

> 函数 SimpleMusicController() {

然后我们将填充之前创建的变量。

> music sources = document . query selector all("音源")；
> music index = 0；
> music control = document . query select(" audio ")；

这里，我们使用一个内置的 JavaScript 函数“querySelectorAll ”,该函数将返回一个数组，该数组包含它找到的作为音频标签的子元素的所有源对象。

我们将索引重置为从 0 开始。

然后我们使用“querySelector”来获取页面上的第一个音频标签。(是的，我们假设在这种情况下我们只有一个想要控制的。)

现在，我们希望向音频标签添加一个事件侦听器，当正在播放的音频文件结束时，该事件侦听器将被触发。这可能是因为它已经结束播放，或者用户已经跳到结尾以强制它结束播放，或者因为脚本已经告诉它结束播放。然而，这不包括用户暂停音频。

出于我们的目的，当音频结束时，我们希望获得列表中的下一个，并告诉它开始播放那个。

> music control . addevent listener(' ended '，function(e)
> {
> music index++；
> if(music index>music sources . length—1){
> music index = 0；
> }
> 
> var mSrc = music sources[music index]。src
> music control . src = mSrc；
> music control . play()；
> 
> }，假)；

正如您在上面的函数中看到的，我们添加了 eventListener，并为它提供了一个在事件触发时运行的函数。

在此范围内，我们从递增索引开始。

然后，我们检查索引是否走得太远，如果走得太远，我们将它设置回起点。零。

确定了新的指数。然后，我们从相关的源标签获取音频文件源，将其设置为音频标签的源，并告诉音频标签播放它。

简单。

如果我们关闭 SimpleMusicController 函数

> }

并在 web 浏览器中加载页面。假设所有的音频文件都可以访问，你应该可以播放它，跳到最后，然后看着它开始自动播放下一首歌。

很好。

![](img/c1a3efe33ff7c352a0da175bb0c81048.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 但是更多呢？

更多？

哦好吧。再补充一点吧。

让它显示当前正在播放的歌曲，以及下一首正在播放的歌曲。

为此，我们需要在我们的标签下添加几个

标签。(或者任何适合你的页面的地方)。我们还需要利用源标签上的数据属性。

添加一个

标签来显示当前正在播放的内容。

我们现在只引用 id。

另一个是为了接下来的事。

布局，风格和 id 都可以由你决定。我们将在代码中引用 id，所以如果您在此处更改了 id，请确保进行更新。

**什么是数据属性？**

数据属性允许我们向 HTML 中添加额外的属性，这些属性可以很容易地在代码中使用，而实际上并没有脱离 HTML 标准。您可以在 MDN 上阅读更多关于数据属性的内容。

使用数据属性，我们将添加内容，为我们提供歌曲标题和艺术家。(这假设您正在播放歌曲。如果没有，您可以调整它以适应您的使用情况。)所以我们的源标签看起来会更像这样。

> <”music/song1.mp3">
> 
> <source src = " music/Song 2 . MP3 " type = " audio/mpeg " data-title = " Song Two " data-artist = " Jane Doe ">
> …等等…

你可以看到非常有想象力的标题。使用这些数据属性，我们可以轻松地引用和显示我们之前创建的 div 中当前和下一首歌曲的艺术家和歌曲名称。

让我们添加一个新的 eventListener 来监听“play”事件。我们将在结束之前在 *SimpleMusicController* 函数中做这件事。

> music control . addevent listener(' play '，function(e) {
> //更新现在播放的标题和艺术家
> document . getelementbyid(' now _ playing ')。innerHTML = '正在播放:'+music sources[music index]. dataset . title+' by '+music sources[music index]. dataset . artist；
> 
> //获取下一首要播放的歌曲的信息并显示
> tmpIndex = music index+1；
> if(tmpIndex>music sources . length—1){
> tmpIndex = 0；
> }
> document . getelementbyid(' up _ next ')。innerHTML = '即将到来:'+music sources[tmp index]. dataset . title+' by '+music sources[tmp index]. dataset . artist；
> }，假)；

我们首先更新 now_playing div 中显示的 HTML。将其设置为“正在播放:”然后通过引用这些数据属性添加歌曲标题和艺术家。

您会注意到我们设置的数据属性是:

> data-title = " Song One "
> data-artist = "无名氏"

然而，在我们的代码中，我们是通过以下方式访问它的:

> music sources[music index]. dataset . title
> music sources[music index]. dataset . artist

当使用 JavaScript 访问这些属性时，我们不需要使用“数据”位。这是扩展 HTML 标签的一种非常好的方式，可以包含额外的属性，这些属性可以很容易地通过代码中的名称来访问。

设置完当前播放的歌曲后，我们创建一个临时索引来确定下一首歌曲。就像我们在“end”函数中做的一样，也检查我们是否已经过去，如果需要的话循环回到起点。

然后我们更新“up _ next”div 来显示下一首将要播放的歌曲的标题和艺术家。

调用页面上的 SimpleMusicController 函数，您应该会看到当您开始播放标题和艺术家节目时，以及下一首歌曲的标题和艺术家，当一首歌曲结束时，它会自动移动到下一首歌曲。

太好了！

# 结论

非常好。因此，我们采用了一个基本的 HTML 音频标签，并编写了一个非常简单的脚本来创建一个播放列表，并播放音频源列表。它现在非常简单，未来有很大的扩展空间。在不久的将来，我可能会做更多的工作，在此基础上进行扩展并添加额外的功能。比如显示播放列表的能力，随机播放歌曲的能力，在结束时停止而不是循环播放的能力等等。我们也可以看看一些更先进的控制和…我有一个很酷的想法，我们可以用它来变成一些额外的特殊，我会努力把它们放在一起。稍后会有更多细节。