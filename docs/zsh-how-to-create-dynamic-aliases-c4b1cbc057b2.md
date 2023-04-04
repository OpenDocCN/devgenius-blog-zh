# ZSH:如何创建动态别名

> 原文：<https://blog.devgenius.io/zsh-how-to-create-dynamic-aliases-c4b1cbc057b2?source=collection_archive---------4----------------------->

![](img/e470638455a51eb13836d0ad00b8d4a8.png)

照片由 [Sai Kiran Anagani](https://unsplash.com/@_imkiran?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

是不是经常说“Devops”不是工作，是一种文化和心态的转变。Devops 的主要租户之一是“自动化一切”。这个租户不应该只适用于你写的代码和你放在一起的解决方案；理想情况下，它应该适用于你工作的方方面面。

对我来说，这意味着我喜欢在我的工作笔记本电脑上自动完成一些事情，并且再也不用去想它们了。例如，我有一个脚本，可以在几分钟内将一台全新的笔记本电脑设置为工作状态。我也是化名的超级粉丝！它节省了我的打字时间，也意味着我不必记住每个开关来使命令工作。我使用“OhMyZSH ”,默认情况下它提供了很多别名(git、kubectl 等等),但是我也创建了自己的别名。

我笔记本电脑上的一组别名用于与我们内部创建的跑步者进行交互。跑步者允许我们跑地形。kubectl，以及我们所有环境中的许多定制工具。最近，我们更新了跑步者的工作方式，更新打破了我现有的所有别名。我没有更新所有损坏的别名，而是决定创建动态别名，这样，如果我不得不再次更改它们，只需更改一行。

这里是我如何设置动态别名。

我们的环境被定义为文件系统中的单个文件夹。

```
ls -h ~/infra-envs
env1 
env2 
env3 
enva 
envb 
envc
```

我想为每个 env 创建一组别名，我希望这是动态的。这意味着如果添加了一个新的 env，我将自动获得一组新的别名。

我在`~/.zshrc`中做的第一件事是创建一个 for 循环，遍历我所有的 env。

```
for directory in ~/infra-**/*
do
 somethingdone
```

`~/infra-**/*` —告诉 ZSH 匹配任何以`infra-`开头的目录，并返回这些目录中的对象。

上面的查询返回绝对路径(例如`/User/ken/infra-envs/env1)`，我只想要目录的基本名称(例如`env1`)。下一步是只删除我想要的组件的路径。

```
basedir=$(echo $directory| xargs -n 1 basename)if [[ "$basedir" == "undesired-dir" ||  "$basedir" == "undesired-dir2" ]]
then
   continue
fi
```

我还借此机会排除了我不需要的目录。

此时，我们有了一个想要迭代的 env 的格式化列表。由于我们采取的方法，将来任何新的 env 都将自动包含在内。现在来看看实际的混叠。

```
for action in command_1 command_2 command_3 tf_plan tf_apply
    do
        tempalias="alias runner-$basedir-$action='bash  ~/runner.sh $basedir $action'"
        eval $tempalias
    done
```

对于这一节，我想在解释之前先展示代码。对于 env，我遍历一个“命令”列表，并以`runner-$ENV-$CMD`的格式创建一个匹配的别名。在决定命名结构时，我希望命名是唯一的，易于预测，并且易于制表完成。

在向您展示最终结果之前，这里是我的`~/.zshrc`的完整代码片段。

```
for directory in ~/infra-**/*
do
    basedir=$(echo $directory| xargs -n 1 basename)if [[ "$basedir" == "undesired-dir" ||  "$basedir" == "undesired-dir2" ]]
    then
        continue
    fifor action in command_1 command_2 command_3 tf_plan tf_apply
    do
        tempalias="alias runner-$basedir-$action='bash  ~/runner.sh $basedir $action'"
        eval $tempalias
    done
done
```

需要注意的是，这段代码位于`~/.zshrc`中。这意味着每次你加载一个新的终端窗口，这段代码都会被执行。在现有的终端窗口上，您可以运行`source ~/.zshrc`来强制手动执行。

这里是最终结果！如果我输入`runner-env`，然后按 tab 键完成，我会返回:

```
➜  ~ runner-env
runner-env1-command_1 
runner-env1-command_2
runner-env1-command_3  
runner-env1-tf_plan     
runner-env1-tf_applyrunner-env2-command_1
runner-env2-command_2  
runner-env2-command_3  
runner-env2-tf_plan  
runner-env2-tf_apply {TRUNCATED}runner-envc-command_1  
runner-envc-command_2
runner-envc-command_3  
runner-envc-tf_plan
runner-envc-tf_apply
```

检查其中一个别名，我们可以看到它被设置为所需的值。

```
➜  ~ alias | grep "runner-env1-tf_pl"
runner-env1-tf_plan='bash  ~/runner.sh env1 tf_plan'
```

现在，启动跑步者就像输入`runner`一样简单，选择 env，然后点击完成想要的任务！

# 结论

如上所述，我非常喜欢化名。自动化/动态别名甚至更好！我不必花时间键入命令、记住开关或循环浏览我的历史记录。

浏览你的历史，看看是否有你可以创造的别名，以节省时间，并把脑力用在其他地方！

*为了获得无限的故事，你也可以考虑* [*注册*](https://blog.rhel.solutions/membership) *成为中等会员，只需 5 美元。如果您使用* [*我的链接*](https://blog.rhel.solutions/membership) *注册，我会收到一小笔佣金(无需额外付费)。*