# 如何用 Javascript 提取 YouTube 或 Vimeo URL 的 ID

> 原文：<https://blog.devgenius.io/how-to-extract-the-id-of-a-youtube-or-vimeo-url-with-javascript-ad5e2d1049a?source=collection_archive---------2----------------------->

从 YouTube 或视频 URL 中轻松提取 id 的指南。

![](img/5c0323aed69d34b2a5ac92e7120908cd.png)

有时您会发现自己需要从 URL 中提取 ID。例如，如果您的最终用户在 CMS 系统中工作，他们可以通过输入视频的 URL 来添加视频，所以让他们更容易。这样做的问题是，他们输入的 URL 不是您生成 embed/iframe 所需的 URL。在这些情况下，您会发现自己需要提取 ID，这样您就可以自己生成嵌入的 URL。

# 正则表达式

为此，我们需要使用正则表达式。正则表达式使用起来非常强大。简而言之，正则表达式基本上是描述搜索模式的特殊文本字符串。我喜欢使用像[https://regex101.com/](https://regex101.com/)这样的服务来测试我的正则表达式模式。

在这种情况下，我们将获取字符串(用户输入的字符串)，应用正则表达式并找到 ID，然后返回 ID，并用 ID 构建我们自己的嵌入 URL。

# 获取 YouTube URL 的 ID

我们需要做的第一件事是创建 regex 模式，我们将使用它来匹配传递给函数的字符串。

然后我们将匹配字符串和正则表达式，最后返回结果。

```
const getYouTubeVideoIdFromUrl = (url: string) => {
  // Our regex pattern to look for a youTube ID
  const regExp = /^.*(youtu.be**\/**|v**\/**|u**\/**\w**\/**|embed**\/**|watch\?v=|&v=)([^#&?]*).*/;
  //Match the url with the regex
  const match = url.match(regExp);
  //Return the result
  return match && match[2].length === 11 ? match[2] : undefined;
};
```

# 获取 Vimeo URL 的 ID

从 Vimeo URL 获取 ID 与上面的 YouTube 示例非常相似，只是使用了不同的正则表达式模式。

```
const getVimeoIdFromUrl = (url: string) => {
  // Look for a string with 'vimeo', then whatever, then a
  // forward slash and a group of digits.
  const match = /vimeo.***\/**(\d+)/i.exec(url);
  // If the match isn't null (i.e. it matched)
  if (match) {
    // The grouped/matched digits from the regex
    return match[1];
  }
};
```

生成您自己的嵌入 URL

有了上面的函数，你现在可以为 YouTube 和 Vimeo 生成你自己的 URL 了:

```
const generateYouTubeUrl = (videoId: string): string => {
  return `//www.youtube.com/embed/${videoId}?autoplay=1&autohide=1&showinfo=0&modestbranding=1&controls=0&mute=0&rel=0&enablejsapi=1`;
};const generateVimeoUrl = (videoId: string): string => {
  return `https://player.vimeo.com/video/${videoId}?&autoplay=1&loop=1&title=0&byline=0&portrait=0&muted=1&#t=235s`;
};
```

# 结论

正如您可能注意到的，在处理代码时，正则表达式是一个强大的工具。使用正则表达式，可以在字符串中搜索非常具体的条件。非常有用，搜索/匹配模式的可能性非常大。

感谢阅读，我希望你喜欢这篇文章，如果是的话，请点击按钮或订阅来支持我。

如果你还不是一个中等成员，考虑成为一个！你可以从成千上万的作者那里获得像这样的好内容！它有助于支持作家，你也有机会通过写作赚钱。[**在这里注册每月只需 5 美元。**](https://nickychristensen.medium.com/membership)

***如果你想订阅我的全部内容，可以点击*** [***这里的***](https://nickychristensen.medium.com/subscribe)

[](https://javascript.plainenglish.io/stop-using-momentjs-for-dates-this-is-the-better-alternative-2709afb229a0) [## 停止使用 Moment.js 来表示日期——这是更好的选择

### 在 JavaScript 中使用 Date 对象可能会非常麻烦！如果你没有使用现成的数据库，你可以…

javascript.plainenglish.io](https://javascript.plainenglish.io/stop-using-momentjs-for-dates-this-is-the-better-alternative-2709afb229a0) [](https://blog.bitsrc.io/10-things-tips-i-would-tell-my-younger-self-as-a-frontend-developer-and-a-human-being-7959fb8eaf96) [## 作为开发人员，我会给年轻时的自己 10 个建议

### 作为前端开发人员和人类，我会给自己 10 件东西！

blog.bitsrc.io](https://blog.bitsrc.io/10-things-tips-i-would-tell-my-younger-self-as-a-frontend-developer-and-a-human-being-7959fb8eaf96) [](https://medium.com/front-end-weekly/a-closer-look-on-array-reduce-with-useful-examples-34f222664e66) [## 更详细地了解 array.reduce()，并提供有用的示例

### 提高你的 javascript 技能，并通过一些有用的例子来学习如何使用 Array.prototype.reduce()

medium.com](https://medium.com/front-end-weekly/a-closer-look-on-array-reduce-with-useful-examples-34f222664e66) [](https://javascript.plainenglish.io/typescript-generics-explained-in-2-minutes-c95e49783347) [## 类型脚本泛型将在 2 分钟内通过示例进行解释

### 对类型脚本泛型的深入研究

javascript.plainenglish.io](https://javascript.plainenglish.io/typescript-generics-explained-in-2-minutes-c95e49783347) 

***如果你想在某个时候赶上我，跟我上*** [***推特***](https://twitter.com/nickycdk)***|***[***LinkedIn***](https://www.linkedin.com/in/dknickychristensen/)***或简单地访问我的*****网站**|***(这是丹麦语)***