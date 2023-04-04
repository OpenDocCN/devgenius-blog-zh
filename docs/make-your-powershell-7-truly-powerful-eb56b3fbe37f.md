# 让您的 PowerShell 7 真正强大

> 原文：<https://blog.devgenius.io/make-your-powershell-7-truly-powerful-eb56b3fbe37f?source=collection_archive---------0----------------------->

注意:本文讨论的是“哦，我的天哪”的一个过时版本。最新版本请参考 https://ohmyposh.dev/docs/upgrading。

![](img/255eee0010eb3c8f681931b4982e9923.png)

[https://Twitter . com/powershell hero/status/1166871786782371841](https://twitter.com/PowerShellHero/status/1166871786782371841)

终于，在 PowerShell Core 6 发布三年后，PowerShell 的新版本发布了。很多 PowerShell 粉丝一直在等最新版本，包括我。新的 PowerShell 7 最大限度地缩小了 Windows PowerShell 5 和跨平台版本 PowerShell Core 6 之间的差距。此外，最新版本增加了更强大的功能。

我想分享一些希望将 PowerShell 环境配置得更强大但更友好的技巧。

# 哦，我的豪华

受 oh-my-zsh 的启发，oh-my-posh 为您的 PowerShell 会话带来了强大的控制台指示器和功能。

在 PowerShell Core 6 中，PSReadLine 不是内置模块。所以在你安装 oh-my-posh 之前，你应该先安装 PSReadLine 模块。这个额外的步骤有点麻烦。但是在 PowerShell 7 中，PSReadLine 包含了默认分发，所以你可以直接设置 oh-my-posh。

您可以运行这些命令来为 PowerShell 配置 oh-my-posh。

```
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

接下来，启用您的提示并更改主题。

```
Set-Prompt
Set-Theme Paradox
```

> 注意:您的默认终端字体第一次可能无法正确显示电力线字符。请查找[https://github.com/ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)库并选择你最喜欢的电力线补丁字体。另外，如果你在寻找 CJK 平衡字体，请考虑使用 Naver 的 D2Coding 字体([https://github.com/naver/d2codingfont](https://github.com/naver/d2codingfont))。

然后，使用`vim $PROFILE`命令打开 PowerShell 配置文件脚本，并粘贴以下代码。

```
Import-Module posh-gitImport-Module oh-my-posh
Set-Theme Paradox
```

这些代码使您的默认 PowerShell 看起来和感觉上有了更大的改进。你可以改变你想要的主题。

# PSFzf 模块

如你所知，fzf 为你提供了强大的搜索功能。尤其是当 fzf 与历史搜索和使用`Ctrl + R`组合键的键绑定相结合时，它大放异彩。

PSFzf 正好解决了这个问题。另外，fzf 已经用 Golang 写了。所以你可以在 macOS，Linux，甚至 Windows 中使用 fzf。

要使用 PSFzf 模块，您应该首先在机器上安装 Fzf。

*   在 Windows 上，您可以选择 Chocolatey 软件包管理器或 Scoop 软件包管理器。
*   在 Linux 上，大多数包管理器都提供了 fzf 包。(除了 Ubuntu 不提供该包。您也可以使用 Git 安装方法。)
*   在 macOS 上，家酿是你最好的朋友。

安装 fzf 二进制文件后，只需将 PSFzf 模块安装到 PowerShell 中。

```
Install-Module PSFzf -Scope CurrentUser
```

最后，让 PSFzf 在启动时覆盖默认的键绑定。粘贴下面的代码。

```
Remove-PSReadlineKeyHandler 'Ctrl+r'
Remove-PSReadlineKeyHandler 'Ctrl+t'
Import-Module PSFzf
```

# k、kubectx 和 kubens 的 PowerShell 版本

如果你每天都坚持使用 Kubernetes，你可能会知道或考虑 kubectl 的速记表达。

我每天都在用化名`K`、`kubectx`和`kubens`。但是这些脚本不是为 PowerShell 设计的。所以我试着为每一个速记惯例制作自己的版本。当然，您可以将下面的代码片段用于您的其他环境，包括 Windows。

## “K”的别名

```
Set-Alias k kubectl
```

## “选择-库贝克文本”(又名库贝克文本)

```
function global:Select-KubeContext {
  [CmdletBinding()]
  [Alias('kubectx')]
  param (
    [parameter(Mandatory=$False,Position=0,ValueFromRemainingArguments=$True)]
    [Object[]] $Arguments
  )
  begin {
    if ($Arguments.Length -gt 0) {
      $ctx = & kubectl config get-contexts -o=name | fzf -q [@Arguments](http://twitter.com/Arguments)
    } else {
      $ctx = & kubectl config get-contexts -o=name | fzf
    }
  }
  process {
    if ($ctx -ne '') {
      & kubectl config use-context $ctx
    }
  }
}
```

## “精选库贝纳斯空间”(又名库本斯)

```
function global:Select-KubeNamespace {
  [CmdletBinding()]
  [Alias('kubens')]
  param (
    [parameter(Mandatory=$False,Position=0,ValueFromRemainingArguments=$True)]
    [Object[]] $Arguments
  )
  begin {
    if ($Arguments.Length -gt 0) {
      $ns = & kubectl get namespace -o=name | fzf -q [@Arguments](http://twitter.com/Arguments)
    } else {
      $ns = & kubectl get namespace -o=name | fzf
    }
  }
  process {
    if ($ns -ne '') {
      $ns = $ns -replace '^namespace/'
      & kubectl config set-context --current --namespace=$ns
    }
  }
}
```

将这些代码粘贴到您的`$PROFILE`脚本代码中，您将获得更舒适的 PowerShell devo PS 工作空间。

# 结论

我认为 PowerShell 的真正强大之处在于，您可以为包括 Windows 在内的所有操作系统编写可读性更强、更易管理且可重用的 Shell 自动化代码。这意味着不再需要考虑更多的极端情况。您只需编写一次 PowerShell 代码片段，就可以在任何地方共享代码。

PowerShell 的下一个版本将会提供越来越多的功能。我等不及了！

[![](img/6d60b235fcc46a4bd696b90e886419ee.png)](https://www.buymeacoffee.com/rkttu)