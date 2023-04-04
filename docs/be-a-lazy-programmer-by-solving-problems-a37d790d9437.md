# 通过解决问题做一个懒惰的程序员

> 原文：<https://blog.devgenius.io/be-a-lazy-programmer-by-solving-problems-a37d790d9437?source=collection_archive---------27----------------------->

![](img/89454001087850f9936507ded46b313a.png)

把一件事做好一次，这样你就不必再做一次。

> “解决一次问题，再也不去想。”
> 
> —我希望在开始编程时听到的话。

## 例子

这个概念更容易形象化，而不是阅读。所以我来分解一下一个简单的问题。假设您想使用 javascript 打开一个网站的弹出窗口。这方面的代码是:

```
window.open(url,'_blank','height=570,width=520,scrollbars=yes,status=yes')
```

谁记得所有这些参数都是逗号分隔的字符串？不是我，我说。就我个人而言，我很懒。我再也不想谷歌“我如何用 javascript 打开一个新窗口”。所以取而代之的是:

## 做工作

干脏活，批判性地思考这个问题。解决问题，让你未来的自己骄傲。如果你记录了你的解决方案，你的同事也会很高兴。(也许不是为了打开一个弹出窗口，我知道这是一个几乎无用的例子，但我用它来表达我的意思。)

## 编写您希望可以使用的代码

在这个例子中，我希望能够这样做:

```
openPopup('google.com'); // Open and I don't care how big or small
openPopup('google.com', {height:570, width:520}) // Set the size
openPopup('google.com', {scrollbar:false}) // I don't want a scroll
.. etc
.. etc
```

花点时间构建这个方法，让它完全按照您的意愿使用:

```
function openPopup(url, params){
  if(typeof params !== 'string'){
    let string = '';
    params.forEach((k, v) => {
      v = v == false ? 'no' : (v == true ? 'yes' : v );
      string += k + '=' + v
    });
    params = string;
  }
  window.open(url, '_blank', params)
}
```

现在你再也不用考虑这个问题了。你再也不需要记住必须使用' no '而不是 false，或者参数必须以字符串格式出现。这是一个已经解决的问题。

## 解决问题

一旦问题解决了，忘掉它，继续前进。下次出现类似问题时，回到您的函数并使用它。少浪费时间去构建你已经构建好的东西。老问题不好玩，新问题好玩，所以那才是你该花精力的地方。

您的清单中“解决的问题”越多，您发布代码和交付特性的效率就越高。

## 不要忘记保存

在 GitHub 中创建一个新的存储库，在那里添加辅助函数。如果您是一个大型项目的一部分，请确保将这些函数适当地放在源代码的上下文中。有些助手是全局的，有些专门用于代码库的某些部分。不要创建大量的帮助函数文件。务必在最有意义的地方添加方法。这是你应该在“做工作”步骤中考虑的事情，但是重申它似乎是一个好主意。

## 当你在这里的时候。在推特上给我一个关注！

[https://twitter.com/andyhartnett12](https://twitter.com/andyhartnett12)