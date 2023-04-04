# Cobra 和 Go 的外壳完井

> 原文：<https://blog.devgenius.io/shell-completion-with-cobra-and-go-c8368074d8f7?source=collection_archive---------0----------------------->

## 一个简短的例子，说明如何获得一个有效的 shell 完成脚本来将您的 CLI 推向下一个级别。

![](img/67475b964a5a05d91244319f9f9843df.png)

照片由[琼·加梅尔](https://unsplash.com/@gamell?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

您可能每天都在使用 CLI。根据 CLI 适用的解决方案，可用选项的数量可能会多得令人不知所措。因为没有视觉识别可用，我们依靠帮助来找到正确的参数。CLIs 的一个吸引人的扩展是为用户提供 shell 补全。通过不间断地显示命令的预期参数，简化了用户的生活。这篇文章解释了如何使用 Cobra 库在 Go 中获得一个 shell 完成 CLI。

如果您正在使用 Go 进行开发，Cobra 库是一个非常值得花时间的解决方案。这篇文章假设你熟悉这个库和它的主要概念(命令、标志等)。).Shell 完成不是由 Go 程序本身提供的。实际上，这是通过 Cobra 生成的 shell 脚本完成的，该脚本捕获用户与我们的 CLI 的交互。您可以在 15_cli 文件夹的 [Build Systems with Go repo](https://github.com/juanmanuel-tirado/savetheworldwithgo) 中找到这些示例的源代码以及更多内容。

以下 CLI 是一个简单的 Cobra 示例，它使用命令在控制台中打印一条消息(hello 或 bye)。

```
package main

import (
    "fmt"
    "github.com/spf13/cobra"
    "os"
)

var RootCmd = &cobra.Command{
    Use: "say",
    Long: "Root command",
}

var HelloCmd = &cobra.Command{
    Use: "hello",
    Short: "Say hello",
    Run: func(cmd *cobra.Command, args []string) {
        fmt.Println("Hello!!!")
    },
}

var ByeCmd = &cobra.Command{
    Use: "bye",
    Short: "Say goodbye",
    Run: func(cmd *cobra.Command, args []string) {
        fmt.Println("Bye!!!")
    },
}

func init() {
    RootCmd.AddCommand(HelloCmd,ByeCmd)
}

func main() {
    if err := RootCmd.Execute(); err != nil {
        fmt.Fprintln(os.Stderr, err)
        os.Exit(1)
    }
}
```

# 创建一个 Shell 完成脚本

Cobra 为 Bash、Zsh、Fish 和 PowerShell 生成 shell 完成脚本。`Command`类型提供了带有类似`GetXXXCompletions`签名的方法，为外壳`XXX`生成相应的完成脚本。通常，CLI 中会添加一个`completion`命令，允许用户为他们的 shells 生成相应的完成脚本。当脚本加载到 shell 中时，按 tab 键两次会显示有效的命令和帮助。以下示例显示了使用 root 命令的完成命令的可能实现(查看[文档](https://github.com/spf13/cobra/blob/master/shell_completions.md)了解更多详细信息)。

```
package cmd

import (
    "github.com/spf13/cobra"
    "os"
)

var CompletionCmd = &cobra.Command{
    Use:   "completion [bash|zsh|fish|powershell]",
    Short: "Generate completion script",
    Long: "To load completions",
    DisableFlagsInUseLine: true,
    ValidArgs:             []string{"bash", "zsh", "fish", "powershell"},
    Args:                  cobra.ExactValidArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
        switch args[0] {
        case "bash":
            cmd.Root().GenBashCompletion(os.Stdout)
        case "zsh":
            cmd.Root().GenZshCompletion(os.Stdout)
        case "fish":
            cmd.Root().GenFishCompletion(os.Stdout, true)
        case "powershell":
            cmd.Root().GenPowerShellCompletionWithDesc(os.Stdout)
        }
    },
}
```

如果我们使用

```
RootCmd.AddCommand(HelloCmd,ByeCmd,CompletionCommand)
```

我们可以为 Bash 生成并加载 shell 完成脚本，如下所示。

```
>>> ./say completion bash > /tmp/completion
>>> source /tmp/completion
```

现在，按下 tab 键(表示为[tab])两次会显示以下内容:

```
>>> ./say [tab][tab]
bye         -- Say goodbye
completion  -- Generate completion script
hello       -- Say hello
help        -- Help about any command
```

可以显示命令参数以获得更多帮助。有效参数的列表可以由类型为`Command`的`ValidArgs`字段提供。我们的完成命令已经为我们填充了这个字段。

```
>> ./say completion [tab][tab]
bash        fish        powershell  zsh
```

# 在运行时获取命令参数

在某些情况下，命令的参数只能在运行时确定。例如，假设我们有一个应用程序，它使用某个用户的标识符在数据库中查询她的信息。用户 id 只有在数据库中存在时才是有效的参数。对于这些场景，可以使用字段`ValidArgsFunction`中的函数定义有效参数列表。下面的例子用`UserGet`函数中的随机选择器模拟不同用户的可用性。`ShellCompDirective`是一个二进制标志，用于修改外壳行为。有关此标志及其作用的更多信息，请查阅文档。

```
package mainimport (
    "fmt"
    "github.com/juanmanuel-tirado/savetheworldwithgo/15_cli/cobra/advanced/example_05/cmd"
    "github.com/spf13/cobra"
    "os"
    "math/rand"
    "time"
)var RootCmd = &cobra.Command{
    Use: "db",
    Long: "Root command",
}var GetCmd = &cobra.Command{
    Use: "get",
    Short: "Get user data",
    Args: cobra.ExactValidArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
        fmt.Printf("Get user %s!!!\n",args[0])
    },
    ValidArgsFunction: UserGet,
}func UserGet (cmd *cobra.Command, args []string, toComplete string) ([]string, cobra.ShellCompDirective) {
    rand.Seed(time.Now().UnixNano())
    if rand.Int() % 2 == 0 {
        return []string{"John", "Mary"}, cobra.ShellCompDirectiveNoFileComp
    }
    return []string{"Ernest", "Rick", "Mary"}, cobra.ShellCompDirectiveNoFileComp
}func init() {
    RootCmd.AddCommand(GetCmd, cmd.CompletionCmd)
}func main() {
    if err := RootCmd.Execute(); err != nil {
        fmt.Fprintln(os.Stderr, err)
        os.Exit(1)
    }
}
```

在生成并加载 shell 补全脚本之后，补全使用如下所示的`UserGet`函数动态建议用户 id。

```
>>> ./db get [tab][tab]
John  Mary
>>> ./db [tab][tab]
completion  -- Generate completion script
get         -- Get user data
help        -- Help about any command
>>> ./db get [tab][tab]
Ernest  Mary    Rick
```

**注意**:如果这个例子不适用于您的终端，您可能需要安装 bash 完成包(`sudo apt install bash-completion`)。

如您所见，使用 Cobra 生成 shell 完成脚本非常简单。我希望您考虑在您的下一个项目中添加 shell 完成功能。

感谢阅读。

[](https://jmtirado.net/build-systems-with-go/) [## 使用 GO 构建系统

### 岁月流逝，围棋不再是积木上的新玩意儿，它已经成为一种成熟的语言围绕着…

jmtirado.net](https://jmtirado.net/build-systems-with-go/)