# Bash 别名和函数:程序员的生产力黑客

> 原文：<https://blog.devgenius.io/bash-aliases-and-functions-a-programmers-productivity-hack-1027c65f37?source=collection_archive---------3----------------------->

作为一名大学生、实习生和相对较新的程序员，我总是向我的教授、导师和经验丰富的队友寻求编码和工作流技巧。最近，我对 Bash 别名和函数有了新的认识，它帮助我简化了工作流程，减少了麻烦。

![](img/83c87ea19d3491c5ad3e7298ad5148b9.png)

照片由 [Rich Tervet](https://unsplash.com/@richtervet?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 什么是 Bash &。巴沙尔？

如果您是一名程序员，这可能不是您第一次听说 Bash。Bash 是一个 shell 实际上，它从命令行或文件中读取和执行命令(它可以读取和执行的文件称为 shell 脚本)。

说到 shell 脚本…这就是`.bashrc`文件！这是一个特殊的程序，Bash 在交互启动时运行(例如，当你打开一个终端窗口时)。因为它是一个 shell 脚本，Bash 可以读取和执行文件中包含的任何命令，所以您可以在`.bashrc`中设置变量、别名和函数，并在命令行中执行它们。

# 你为什么要在乎？

因为`.bashrc`中的任何东西都可以通过命令行执行，这意味着你可以*配置你自己的快捷键*和*完全定制命令* …这就是我所说的生产力黑客。

这不仅仅是为了程序员；如果我们在命令行中有很多业务，我们可能会得到更多的使用，但是自动化过程和快速打开应用程序/文件夹/文件是每个人的。

另外，它让你觉得自己是超级黑客。

# 正在设置。bashrc

> 附注:很多人更喜欢 Zsh 而不是 Bash，你可以在两者中使用相同的快捷方式，只是使用不同的格式。我个人使用 Bash，因为它是我使用的不同系统的标准。

开始，查找或创建您的。bashrc 文件。我在 Mac 上，一开始没有，所以我在`/Users/your-username`目录中创建了一个，并在顶部添加了这些简单的别名，作为开始:

```
alias reload = 'source ~/.bashrc'
alias edbash = 'vi ~/.bashrc'
```

关闭文件并返回到`/Users/your-username`目录。在命令行中执行`source ~/.bashrc`来重新加载`.bashrc`文件。现在执行`.bashrc`文件中的一个别名，比如`edbash`，应该可以工作了。

这里有一个问题(如果你在 Mac 上)；如果您关闭并重新打开终端会话，然后尝试使用其中一个别名，则会出现错误`zsh command not found`。因为 Mac 使用 Zsh，所以您需要 Zsh 在每个终端会话中重新加载`.bashrc`,以便它能够识别该文件中的别名。

要解决这个问题，从您的`/Users/your-username`目录创建/编辑文件`.zshrc`,并添加:

```
[[ -s ~/.bashrc ]] && source ~/.bashrc
```

关闭文件，再次返回`/Users/your-username`目录。现在尝试从您的`.bashrc`文件中执行别名；应该能行！

你很快就会让巴什成为你最好的朋友。

# 现在有趣的是…

`edbash`和`reload`很棒，但是它们只是触及表面，只适用于`.bashrc`文件。以下是一些在不同情况下派上用场的别名和函数:

> 完全披露:这些别名和功能中的一些是由在线资源&朋友( [Lannie Hough](https://medium.com/u/fd975fd489b2?source=post_page-----1027c65f37--------------------------------) )提供或启发的。我从零开始想出了一些，但我的大部分精力都花在了想出应用基本操作的创造性方法上。

## 日常 Linux 操作

```
#Lazy way to list files
alias l = 'ls -la'#Jump to often-used directories
alias sr = 'cd ~/Documents/School/Senior\ Research'#Change directory & list files 
cl() {
   cd $1
   ls -la
}#Make directory & enter it
mkcd() {
   mkdir -p $1
   cd $1
}#Search for a specific file 
#Use: "findfile example"
#Results: prints any files that begin with "example", is not case-sensitive, picks up any file type (ex. result: ExampleTest.docx)
findfile() {
   file = "$@"
   file += "*"
   find . -iname $file 2>&1 | grep -v "Operation not permitted"
}#Search for all files with a specific extension
#Use: "fondest swift"
#Results: prints all .swift files
findext() {
   ext = "*."
   ext += "$@"
   find . -iname $ext 2>&1 | grep -v "Operation not permitted"
}
```

## Git 和 CocoaPods 处理

```
#Git
alias pull = 'git pull origin master'
alias add = 'git add --all'
alias push = 'git push --all'gitcombo() {
   add
   message=""
   for arg in "$@"
   do
      message += "$arg"
      message += " "
   done
   git commit -m "$message"
   push
}#CocoaPods
pod refresh() {
   pod deintegrate
   pod update
   pod install
}
```

## 过程控制

这是我暴露自己懒惰的地方……我能说什么呢？我讨厌手动关闭我打开的 30 个应用程序。此外，我意识到如果你没有保存你的工作，那么杀死一个程序，这些可能是危险的，这就是为什么我会选择在这些函数中包含哪些应用程序。一些应用程序，如 VSCode，响应不好被杀死(老实说，不要责怪他们)，所以那些将不得不手动关闭。

```
stopcoding() {
   pkill Trello
   pkill Xcode #seems to save changes (I tested it)
   pkill Simulator
   pkill Discord
   pkill SF\ Symbols
}freeram() {
   pkill Mail
   pkill Calendar
   pkill Music
   pkill Notes
   pkill Pages
   pkill Messages
   pkill -9 Microsoft\ Outlook #complained without the -9
   #pkill literally any application... you get the point
   #but I don't include Safari/Chrome or apps used constantly
}
```

## 项目设置

每次我坐下来做代码项目时，我发现自己不得不打开相同的目录、网页和应用程序。为什么不自动化呢？

```
#Project Shortcuts (commands generalized)
alias projectgit = 'open https://github.com/mgipson/MyProject'
alias project = 'cd ~/Documents/ProjectFolder; open -a Xcode MyProject.xcworkspace'
project() {
   projectgit
   project
   open -a Trello
}
```

# 黑客快乐！