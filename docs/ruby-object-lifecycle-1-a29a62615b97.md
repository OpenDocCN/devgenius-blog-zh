# Ruby:对象生命周期(1)

> 原文：<https://blog.devgenius.io/ruby-object-lifecycle-1-a29a62615b97?source=collection_archive---------2----------------------->

![](img/aa8287ad3895cf3db7100e0ba0d9c4ef.png)

[杰森·D](https://unsplash.com/@jasondeblooisphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在通过 Skilled 进行了一次模拟技术面试后，我意识到我的基础编程知识中缺少了很多东西。我现在要开始把我目前知道的写出来，以后再回头看。首先是 Ruby 中的对象生命周期。开始时，我最大的困难之一是使用未定义变量的方法。这是可以解决的，一旦定义了，就可以供以后使用。

# 什么是课？

在面向对象编程中，类本质上是一个带有数据的模板，数据包含信息和行为。在一个类中，我们可以实例化一个新的对象并赋予它属性。我们甚至可以用新的属性在一个类中实例化多个对象。

# 类别和对象

Ruby 中的这几行代码允许我们创建一个新类:

```
class Videogame
end 
```

在 Ruby 中创建一个新类实质上是为我们创建一个新的数据类型来添加属性。属性是我们为新类的给定示例提供的信息。这些属性可以写成这样:

```
class Videogame
 attr_accessor :name, :studio, :genre, :price
end
```

然后，我们可以看到视频游戏的属性包含名称、工作室、流派和平台。既然我们已经将这些信息告诉了 Ruby，那么我们就可以创建代表我们的视频游戏的单个实例的对象。我们想要创建一个新的视频游戏并存储该数据类型。我们已经定义了它将具有这些属性，所以现在我们可以为它创建一个新的对象。

```
class Videogame
attr_accessor :name, :studio, :genre, :price
endvideogame1 = Videogame.new()
videogame1.name = "Cyberpunk 2077"
videogame1.studio = "CD Projekt Red"
videogame1.genre = "RPG"
videogame1.price = "$60"
```

一个对象基本上是一个类的实例。所以现在 videogame1 对象在我们的程序中，我们可以通过给它信息来与之交互。如果您在控制台中输入对象及其属性，则可以看到以下信息。我们将在该类中创建一个具有新属性的附加对象，并接收我们输入的内容:

```
class Videogame
attr_accessor :name, :studio, :genre, :price
endvideogame1 = Videogame.new()
videogame1.name = "Cyberpunk 2077"
videogame1.studio = "CD Projekt Red"
videogame1.genre = "RPG"
videogame1.price = "$60"# puts videogame1.name
# "Cyberpunk 2077"videogame2 = Videogame.new()
videogame2.name = "Sims 4"
videogame2.studio = "Electronic Arts"
videogame2.genre = "Sandbox"
videogame1.price = "$60"# puts videogame2.genre
# "Sandbox"
```

接下来，我们可以使用 initialize 方法调用一个新对象。

```
class Videogame
attr_accessor :name, :studio, :genre, :price def initialize(name, studio, genre)
  @name = name
  @studio = studio
  @genre = genre
  @platform = platform
 endend videogame1 = Videogame.new("Cyberpunk 2077", "CD Projekt Red", "RPG", "$60")
```

在这个例子中，我们告诉 Ruby 这些是用户将要传递参数的属性。属性可以是用户输入的任何东西，也可以在被调用时返回给我们。注意到与以前相比，videogame1 对象现在都在一行中了吗？

接下来，我们可以向我们的类添加一个新方法。

```
class Videogame
attr_accessor :name, :studio, :genre def initialize(name, studio, genre)
  @name = name
  @studio = studio
  @genre = genre
 end def on_sale
  # will return a boolean value
 end end
```

我们可以定义什么将为我们的对象返回 true 或 false。比方说，如果一个视频游戏是一个 RPG，那么它将返回 true，如果不是，那么它就是 false。

```
class Videogame
attr_accessor :name, :studio, :genre, :price def initialize(name, studio, genre, :price)
  @name = name
  @studio = studio
  @genre = genre
  @price = price
 end def rpg_game
  if @genre == "rpg"
    true 
  else 
    false 
  end 
 end end
```

我们在这里比较一个对象的属性，看看某个视频游戏是否是 RPG 类型。我们知道赛博朋克 2077 是一个 RPG，所以它将返回 true。然而，如果我们有另一个对象，比如 video game 3 . genre of“Sports ”,那么它将返回 false，因为它不是一个 RPG 游戏。

# 调试错误

让我们看看这个代码的例子，看看它有什么问题:

```
class Videogame
attr_accessor :name, :studio, :genre, :price def initialize(name, studio, genre, :price)
  @name = name
  @studio = studio
  @genre = genre
  @price = price
 end def rpg_game
  if @genre == "rpg"
    true 
  else 
    false 
  end 
 end def indie_game
   if @studio == indie
    true 
   else 
    false 
   end 
 end end
```

如果我们运行这段代码，它将返回一个错误，因为我们还没有定义一个方法，让 Ruby 能够理解一个工作室是独立的工作室还是公司运营的工作室。在过去，当制作这样的东西时，我会添加额外的属性来定义工作室是否独立运行。这样，如果我们使用“indie ”,那么它可以返回一个布尔值。否则，我们会遇到错误，因为它从未被定义过。

在我的下一篇文章中，我会写一些关于用 Ruby 进行面向对象编程的关键概念，以及一些避免像我这样的错误的方法。

_____________________________________________________________

# 社会联系

[LinkedIn](https://www.linkedin.com/in/shirlend)

[GitHub](https://www.github.com/Ro5hi)

[推特](https://www.twitter.com/len_deta)

[Instagram](https://www.instagram.com/_sceptral_)

[YouTube](https://www.youtube.com/channel/UC_0nik4oj1T1Q160XVr0ZlA?view_as=subscriber)