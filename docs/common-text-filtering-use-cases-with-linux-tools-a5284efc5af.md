# Linux 工具的常见文本过滤用例

> 原文：<https://blog.devgenius.io/common-text-filtering-use-cases-with-linux-tools-a5284efc5af?source=collection_archive---------17----------------------->

使用`grep`、`awk`和`sed`导航并挖掘文本文件中的隐藏模式。

![](img/45dfe172eececc19eb2bdd8c52bb803f.png)

帕特里克·托马索在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

我们经常以代码、配置、数据或日志文件的形式处理各种文本文件，其中一些文件的大小可能是千兆字节。在像`less`这样的文本查看器中简单地浏览或搜索如此大的文本文件以获得洞察力将是具有挑战性的，尤其是当搜索的信息事先并不完全已知时。幸运的是，这里有大量的 Linux 命令行工具可以帮助我们，这可能会让我们很快获得有用的见解。在本文中，我将展示三个突出的 Linux 命令行工具的一些常见用例，即`grep`、`awk`和`sed`，它们都与文本过滤和转换有关。这里讨论的所有用例都在 Linux 环境中使用 GNU `grep`和`sed`以及 POSIX `awk`，尽管在 MacOS 上也有具有相似属性的同名工具。

为了演示这些工具的运行，我们将采用以下从这个 [IMSDb 链接](https://imsdb.com/scripts/Star-Wars-A-New-Hope.html)中摘录的文本，并存储在一个名为`starwars.txt.`的文件中

```
 It is a period of civil war. Rebel spaceships, 
    striking from a hidden base, have won their first 
    victory against the evil Galactic Empire.

    During the battle, Rebel spies managed to steal 
    secret plans to the Empire's ultimate weapon, the 
    Death Star, an armored space station with enough 
    power to destroy an entire planet.

    Pursued by the Empire's sinister agents, Princess 
    Leia races home aboard her starship, custodian of 
    the stolen plans that can save her people and 
    restore freedom to the galaxy...
```

# 常见`grep`用例

`grep`及其工具系列广泛用于显示一个或多个文本文件中的选择性行。我经常使用这个工具来了解文件中项目出现的频率，以及收集感兴趣的行之前和/或之后的内容。

其典型的用法语法是:
`grep [OPTION] 'PATTERNS' FILE`

以下是一些常见的`grep`用例:

1.`**grep 'foo.*bar' file.txt**`
显示由*任何*字符(regex `".*"`)与图案*栏*分隔的图案 *foo* 的任何行。
以下命令显示文件`starwars.txt`中带有*功率*和*行星*字样的行:

```
$ grep 'power.*planet' starwars.txt 
 power to destroy an entire planet.
```

2.`**grep '^foo' file1.txt**`
用模式 *foo* 匹配任何一行的开始(`^`)。
以下命令显示以一个或多个空格开始的任何行，后跟不区分大小写的(`-i`)模式*胜利*:

```
$ grep -i '^[ ]\+victory' starwars.txt
    victory against the evil Galactic Empire.
```

3.`**grep 'bar$' file1.txt**`
匹配以图案*条*结束的任意行尾(`$`)。
下面一行显示任何以点结束的行(点是特殊字符，因此需要转义):

```
$ grep '\.$' starwars.txt
    victory against the evil Galactic Empire.
    power to destroy an entire planet.
    restore freedom to the galaxy...
```

4.`**grep -B1 -A1 'foo' file.txt**`
查找模式为 *foo* 的任何一行，找到后显示前一行(`-B1`)、匹配行和后一行(`-A1`)。
以下命令显示任何带有图案*死星*的行，以及前后的行:

```
$ grep -B1 -A1 'Death Star' starwars.txt 
    secret plans to the Empire's ultimate weapon, the 
    Death Star, an armored space station with enough 
    power to destroy an entire planet.
```

5.`**grep -m1 'foo' file.txt**`
查找任意一条模式为 *foo* 的线，并停在第一个匹配点(`-m1`)。如果您想知道某个特定的公共模式是否存在，而不处理文件的其余部分，这是很有用的。
以下命令显示模式为 *ship* 的第一个匹配行:

```
$ grep -m1 'ship' starwars.txt 
    It is a period of civil war. Rebel spaceships,
```

6.`**fgrep 'foo' file.txt**`
显示任何带有文字字符串 *foo* 的行，不作为正则表达式。与`grep`不同的是，`fgrep`将任何包含特殊字符的模式视为文字。
以下命令显示任何带省略号的行(用三个点表示):

```
$ fgrep '...' starwars.txt
    restore freedom to the galaxy...
```

7.`**egrep 'foo|bar' file.txt**`
查找任何带有图案 *foo* 或 bar 的线条。`egrep`将模式解释为扩展正则表达式。
以下命令显示任何带有图案 *war* 或 *battle* 的行:

```
$ egrep 'war|battle' starwars.txt 
    It is a period of civil war. Rebel spaceships, 
    During the battle, Rebel spies managed to steal
```

# 常见`awk`用例

`awk`本身就是一门完整的语言。它的基本功能是在文件中搜索包含特定模式的行。由于它的灵活性和许多内置功能，甚至可以在单词级别操作行，而不仅仅是显示匹配的行。在`awk`中，每个输入行被称为一个*记录*，默认情况下，由空白分隔的每个单词被称为一个*字段*。这应该有助于理解`awk`的内在变量。

其典型的语法是:
`awk 'PROGRAM' FILE`

以下是一些常见的`awk`用例:

1.`**awk '{print $NF}' file.txt**`
用内置变量`NF`(即字段数)显示每条记录的最后一个字段。
以下命令显示了对样本文件的运行:

```
$ awk '{print $NF}' starwars.txt
spaceships,
first
Empire.
...
galaxy...
```

2.`**awk 'NF > 0 {print}' file.txt**`
通过检查一条记录中 NF 是否大于 0 来显示任何非空记录。
为了限制输出行，以下命令使用逻辑 and 运算符(`&&`)显示非空的*偶数行*:

```
$ awk '(NF > 0) && (NR % 2 == 0) { print NR,$0 }' starwars.txt 
2     striking from a hidden base, have won their first 
6     secret plans to the Empire's ultimate weapon, the 
8     power to destroy an entire planet.
10     Pursued by the Empire's sinister agents, Princess 
12     the stolen plans that can save her people and
```

3.`**awk '/foo/ {print $1,$NF}' file.txt**`
显示与模式 *foo* 匹配的记录的第一个和最后一个字段。`awk`不显示记录，而是允许轻松引用任何单个字段。
以下命令检查记录是否与模式 *ship、*匹配，并相应地打印第一个和最后一个字段:

```
$ awk '/ship/ {print $1,$NF}' starwars.txt
It spaceships,
Leia of
```

4.`**awk '{ print NR,length }' file.txt**`
打印行号及其字符数。
下面是对样本文件的运行:

```
$ awk '{ print NR,length }' starwars.txt
1 51
2 54
3 45
4 4
5 52
6 54
7 53
8 38
9 4
10 54
11 54
12 50
13 36
```

5.`**awk '$2 ~ /foo/ { print }' file.txt**` 如果第二个字段(第一个字段为`$1`)与模式 *foo* 匹配，则打印一条记录。要反转匹配，使用运算符`!~`。如果我们在一个特定的字段或列中寻找一个模式，`awk`内在的字段级支持在这里大放异彩。
下面的`awk`脚本检查第四个字段是否与模式 *Empire* 匹配，并相应地打印记录:

```
$ awk '$4 ~ /Empire/ { print }' starwars.txt 
    Pursued by the Empire's sinister agents, Princess
```

6.`**awk -F, '{if (NF > 1) print $2}' file.txt**`
打印记录中的第二个字段，如果其字段数大于 1，则用“，”分隔(`-F`)字段。
下面是一个运行示例:

```
$ awk -F, '{if (NF > 1) print $2}' starwars.txt 

 have won their first 
 Rebel spies managed to steal 
 the 
 an armored space station with enough 
 Princess 
 custodian of
```

# 常见的`sed`用例

`sed`是另一个强大的工具，可以在一行中的任意点对输入流执行文本转换。由于其灵活性和简洁的脚本，它可能会让新手望而生畏，并有一个陡峭的学习曲线。然而，`sed`提供了各种有用的特性，这些特性是其他工具无法轻易实现的，是一个值得推荐的工具。

其典型用法语法为:
`sed 'SCRIPT' INPUTFILE`

以下是一些常见的`sed`用例:

1.`**sed -n '10,20p' file.txt**`
打印(命令`p`)输入文件的第 10 行到第 20 行。选项`-n`导致`sed`抑制其默认的线条显示。对于大文件，建议在匹配后退出进一步处理。为了实现这一点，下面是带有指令`21q`的相关命令，指示`sed`在第 21 行退出:
`sed -n '10,20p;21q' file.txt` 下面是显示第 1–3 行的运行示例:

```
$ sed -n '1,3p' starwars.txt 
    It is a period of civil war. Rebel spaceships, 
    striking from a hidden base, have won their first 
    victory against the evil Galactic Empire.
```

2.`**sed '/foo/d' file.txt**`
删除(命令`d`)任何带有图案 *foo* 的行。
以下脚本删除任何带有空格的行，只使用正则表达式`"^[ ]*$"`来匹配从该行开始到结束的空格，第一行(`1d`以及最后一行(`$d`):

```
$ sed '/^[ ]*$/d;1d;$d' starwars.txt
    striking from a hidden base, have won their first 
    victory against the evil Galactic Empire.
    During the battle, Rebel spies managed to steal 
    secret plans to the Empire's ultimate weapon, the 
    Death Star, an armored space station with enough 
    power to destroy an entire planet.
    Pursued by the Empire's sinister agents, Princess 
    Leia races home aboard her starship, custodian of 
    the stolen plans that can save her people and
```

3.`**sed '/foo/,/bar/d' file.txt**`
删除从图案 *foo* 行到图案 *bar* 行的所有行。
以下命令从样本文件中删除中间段落:

```
$ sed '/During/,/planet./d' starwars.txt
    It is a period of civil war. Rebel spaceships, 
    striking from a hidden base, have won their first 
    victory against the evil Galactic Empire.

    Pursued by the Empire's sinister agents, Princess 
    Leia races home aboard her starship, custodian of 
    the stolen plans that can save her people and 
    restore freedom to the galaxy...
```

要删除空行，我们可以使用:

```
$ sed '/^[ ]*$/,/^[ ]*$/d' starwars.txt
    It is a period of civil war. Rebel spaceships, 
    striking from a hidden base, have won their first 
    victory against the evil Galactic Empire.
    Pursued by the Empire's sinister agents, Princess 
    Leia races home aboard her starship, custodian of 
    the stolen plans that can save her people and
```

4.`**sed -rn '/(foo).*\1/p' file.txt**`
显示任何带有两个模式 *foo* 的行，括号内的`\1`表示匹配的表达式，作为第二个模式 *foo* 。选项`-r`允许`sed`将脚本视为扩展正则表达式。
以下命令显示任何带有两个“The”的行:

```
$ sed -rn '/ (the).*\1/p' starwars.txt 
    secret plans to the Empire's ultimate weapon, the
```

5.`**sed 's/../&:/g' file.txt**`
用匹配的字符(指令`&`)替换(命令`s`)每两个字符，后跟一个冒号，重复执行直到行尾(命令`g`)。
以下脚本查找任何非空格字符序列(regex `"[^ ]+"`)并附加一个冒号:

```
$ sed -r 's/[^ ]+/&:/g' starwars.txt 
    It: is: a: period: of: civil: war.: Rebel: spaceships,: 
    striking: from: a: hidden: base,: have: won: their: first: 
    victory: against: the: evil: Galactic: Empire.:

    During: the: battle,: Rebel: spies: managed: to: steal: 
    secret: plans: to: the: Empire's: ultimate: weapon,: the: 
    Death: Star,: an: armored: space: station: with: enough: 
    power: to: destroy: an: entire: planet.:

    Pursued: by: the: Empire's: sinister: agents,: Princess: 
    Leia: races: home: aboard: her: starship,: custodian: of: 
    the: stolen: plans: that: can: save: her: people: and: 
    restore: freedom: to: the: galaxy...:
```

6.`**sed -re 's/^[ ]+//' -e 's/$/0/' data.txt**`
对每一行执行两次替换，第一个脚本删除该行开头的所有空格，第二个脚本在末尾附加一个 0。这个稍微复杂一点的用法说明了`sed`的多功能性和灵活性。
下面是一个运行示例，用于修剪开头的任何空白，并用虚线替换样本文件的任何空行:

```
$ sed -re 's/^[ ]+//' -e 's/^$/---------/' starwars.txt
It is a period of civil war. Rebel spaceships, 
striking from a hidden base, have won their first 
victory against the evil Galactic Empire.
---------
During the battle, Rebel spies managed to steal 
secret plans to the Empire's ultimate weapon, the 
Death Star, an armored space station with enough 
power to destroy an entire planet.
---------
Pursued by the Empire's sinister agents, Princess 
Leia races home aboard her starship, custodian of 
the stolen plans that can save her people and 
restore freedom to the galaxy...
```

# 最后的话

Linux 提供了许多文本过滤和转换工具，以便于浏览文本文件。在`grep`、`awk`和`sed`的帮助下，我们甚至可以快速发现非常大的文件中的模式或趋势，否则在文本查看器中浏览文件时是看不到的。

您使用这些工具或者甚至其他 Linux 命令行工具来发现和探索您的文件的*前景*吗？请在评论区分享。谢谢你的阅读，任何掌声都很感谢！