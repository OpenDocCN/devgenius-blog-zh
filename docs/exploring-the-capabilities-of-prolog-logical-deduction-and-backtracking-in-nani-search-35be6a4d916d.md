# 探索 Prolog 的能力:在“纳尼亚搜索”中的逻辑演绎和回溯

> 原文：<https://blog.devgenius.io/exploring-the-capabilities-of-prolog-logical-deduction-and-backtracking-in-nani-search-35be6a4d916d?source=collection_archive---------10----------------------->

![](img/255d1e700b96bd190ff7749cfcb54bac.png)

# 什么是“纳尼搜索”？

游戏“纳尼搜索”是一个简单的基于文本的冒险游戏，用 Prolog 实现。该游戏通过输入简单英语句子的命令来控制玩家的动作，玩家是一个三岁的孩子，试图找到他们的安全毯(“纳尼”)。玩家可以在房间之间移动，观察周围环境，拿取和放下物品，吃东西，并以各种方式与环境互动。当玩家找到纳尼或者放弃时，游戏结束。要理解这个游戏，你必须知道 Prolog 编程的基础。你可以在这里看到完整的游戏:

 [## 阿姆兹！in Prolog 教程中的冒险

### 本附录包含书中描述的四个程序的示例版本。这些是冒险游戏(纳尼…

www.amzi.com](https://www.amzi.com/AdventureInProlog/appendix.php) 

# 游戏逻辑

该游戏使用一系列 Prolog 谓词来实现，这些谓词定义了控制游戏世界和玩家可以采取的行动的规则和事实。序言代码定义了控制游戏世界的规则和事实，以及玩家可以采取的行动。该程序使用一系列谓词来表示游戏中可用的不同房间、对象和动作。

程序从定义`main`谓词开始，它充当游戏的入口点。`main`谓词调用`nani_search`谓词，后者初始化一些动态事实，然后打印游戏简介。然后它调用`command_loop`谓词，该谓词反复提示玩家输入命令并执行相应的动作，直到玩家要么找到 Nani，要么退出游戏。

`command_loop`谓词使用`repeat`控制结构重复读取来自玩家的命令，并调用`do/1`谓词来执行命令。`do/1`谓词使用一系列子句将玩家的命令与执行动作的相应谓词匹配起来。比如玩家输入命令`take(X)`，`do/1`谓词会调用`take/1`谓词来执行取物的动作。

`do/1`谓词还包括用于`nshelp`、`hint`、`inventory`、`quit`、`look`、`turn_on`、`turn_off`和`look_in`谓词的子句，这些子句为玩家提供附加信息和功能。

当玩家找到纳尼(通过拿走它)或者退出游戏时，游戏结束。

如果你难以理解谓词和对象的概念，请随意查看我们关于 prolog 基础的文章。

[](https://pipsworld.medium.com/introduction-to-prolog-a-programming-language-for-artificial-intelligence-320b75455381) [## Prolog 介绍:一种人工智能编程语言

### 了解 Prolog，这是一种用于开发人工智能应用程序的编程语言，可以处理不确定或不完整的…

pipsworld.medium.com](https://pipsworld.medium.com/introduction-to-prolog-a-programming-language-for-artificial-intelligence-320b75455381) 

# 入口点

代码的第一部分定义了游戏的主入口点，即`main`谓词:

```
main:- nani_search.       % main entry point
```

这个谓词简单地调用`nani_search`谓词，它初始化一些动态事实，然后打印游戏的简要介绍:

```
nani_search:-
  init_dynamic_facts,     % predicates which are not compiled
write('NANI SEARCH - A Sample Adventure Game'),nl,
  write('Copyright (C) Amzi! inc. 1990-2010'),nl,
  write('No rights reserved, use it as you wish'),nl,
  nl,
  ...
```

# 命令循环

介绍完后，`nani_search`谓词调用`command_loop`谓词，这是游戏的主循环:

```
command_loop:-
  repeat,
  get_command(X),
  do(X),
  (nanifound; X == quit).
```

`command_loop`谓词使用`repeat`控制结构重复读取来自使用`get_command/1`谓词的玩家的命令，然后使用`do/1`谓词执行该命令。`do/1`谓词使用一系列子句将玩家的命令与执行动作的相应谓词匹配起来。比如玩家输入命令`take(X)`，`do/1`谓词会调用`take/1`谓词来执行取物的动作。

```
do(goto(X)):-goto(X),!.
do(nshelp):-nshelp,!.
do(hint):-hint,!.
do(inventory):-inventory,!.
do(take(X)):-take(X),!.
do(drop(X)):-drop(X),!.
do(eat(X)):-eat(X),!.
do(look):-look,!.
do(turn_on(X)):-turn_on(X),!.
do(turn_off(X)):-turn_off(X),!.
do(look_in(X)):-look_in(X),!.
do(quit):-quit,!.
```

`command_loop`谓词还包括一个检查，看看玩家是否找到了纳尼或者退出了游戏。如果这些条件中的任何一个为真，`command_loop`谓词将终止，游戏将结束。

*除了* `*command_loop*` *谓词，代码还包括许多其他谓词，这些谓词定义了游戏世界的规则和事实以及玩家可以采取的行动。*

例如，`goto/1`、`take/1`、`drop/1`、`eat/1`、`look`、`turn_on/1`、`turn_off/1`和`look_in/1`谓词定义了玩家在游戏中可以采取的动作。这些谓词使用事实和规则的组合来确定对玩家动作的适当响应，并向玩家打印一条描述动作结果的消息。

```
% goto - move the player to a new room
%
%  The "is_a" structure relates rooms to their types and
%  possible exits.  The dynamic database is used to store
%  the room type and exit information for each room as it
%  is encountered.
goto(X):-
  location(here),
  can_go(here,X),
  retract(location(here)),
  asserta(location(X)),
  look.
goto(_):-
  write('You can''t go that way.'),nl.
% can_go - can the player go in that direction
%
%  The rule expresses the "is_a" relationship between rooms
%  and the directions you can go in them.
can_go(R,X):-
  room_exit(R,X),
  write('You go '),write(X),write('.'),nl.
% take - take an object
%
%  The rule expresses the relationship between the objects
%  and the rooms they are in.
take(X):-
  here(R),
  in_
```

# 解释其他谓词

在`command_loop`谓词之后，代码定义了一系列谓词，代表玩家在游戏中可以采取的不同动作。例如，`look`谓词描述了玩家的当前位置以及该位置中存在的对象:

```
look:-
  here(X),
  describe(X),
  nl,
  write('You see:'),nl,
  list_things(X),
  nl.
```

`take/1`谓词允许玩家从当前位置拿走一个物体:

```
take(X):-
  can_take(X),
  retract(here(X)),
  assert(have(X)),
  write('taken'),nl.
```

`drop/1`谓词允许玩家放下他们携带的物体:

```
drop(X):-
  have(X),
  retract(have(X)),
  assert(here(X)),
  write('done'),nl.
```

这些谓词只是玩家在游戏中可以采取的动作的几个例子。完整的代码包括更多的谓词，这些谓词定义了游戏的世界和玩家可以采取的行动。

除了定义玩家可以采取的动作的谓词之外，代码还包括一系列定义游戏世界以及其中存在的对象和位置的谓词。例如，`locations`谓词定义了游戏中可用的房间和其他位置:

```
locations:-
  livingroom,
  kitchen,
  yard,
  garden,
  bedroom,
  hall,
  attic.
```

这些位置中的每一个都由一个单独的谓词定义，该谓词描述了位置和其中存在的对象。例如，`livingroom`谓词定义了起居室的位置和那里的对象:

```
livingroom:-
  assert(here(chair)),
  assert(here(table)),
  assert(here(sofa)),
  assert(here(rug)),
  assert(here(lamp)),
  assert(here(television)),
  assert(here(remote_control)),
  assert(here(book)),
  assert(here(magazine)).
```

这些谓词用于定义游戏的世界以及其中可用的对象和位置。当玩家采取行动时，谓词用于确定该行动的后果，并相应地更新游戏的世界。

总的来说，*纳尼搜索*是一个简单的基于文本的冒险游戏的完整实现。代码定义了管理游戏世界的规则和事实以及玩家可以采取的行动，并使用这些规则和事实来允许玩家探索游戏世界并与之互动。

# 我们如何实现这一点？

*要在 Prolog 中创建一个类似“纳尼搜索”的游戏，可以使用以下步骤:*

1.  定义支配游戏世界的事实和规则，以及玩家可以采取的行动。这可以包括关于游戏中的位置、游戏中存在的对象以及这些位置和对象之间的关系的事实。它还可以包括指定玩家行为后果的规则，例如移动到一个新的位置或拿走一个对象。
2.  **实现游戏的主入口。**这可以包括任何必要的设置，例如初始化动态事实或打印欢迎消息。它还可以包括主游戏循环，该循环提示玩家输入命令，并使用您在步骤 1 中定义的谓词处理该命令。
3.  **实现处理玩家动作的谓词。这些谓词应该使用您在步骤 1 中定义的事实和规则进行逻辑推理，并确定玩家行为的后果。这些谓词可以包括诸如移动到一个新位置、拿走一个对象或查看玩家周围环境之类的动作。**
4.  **测试游戏以确保它的行为符合预期。**这可以包括手动玩游戏，并验证游戏是否正确响应玩家的动作。它还可以包括运行自动化测试来验证游戏在不同场景下的行为。

总的来说，在 Prolog 中创建一个类似于“Nani Search”的游戏需要定义控制游戏世界的事实和规则以及玩家可以采取的行动，实现主游戏循环和处理玩家行动的谓词，并测试游戏以确保其行为符合预期。

# 除了基于文本的游戏，我可以用“纳尼搜索”和 Prolog 上的这些信息做什么？

除了基于文本的游戏，Prolog 还有许多其他可能的应用，如自然语言处理、自动推理和人工智能。

Prolog 在基于文本的游戏之外的一个可能应用是自然语言处理。 **Prolog 对逻辑推理和回溯的支持使其非常适合于实现能够分析和理解自然语言输入的算法。**例如，Prolog 程序可用于处理和理解以自然语言表达的命令或问题，例如在“Nani Search”中使用的那些。

Prolog 的另一个可能的应用是自动推理。例如，这可以用于实现专家系统，该专家系统可以对知识领域进行推理，并基于该知识提供建议或推荐。

最后，它非常适合实现人工智能算法。例如，这些算法可以用来实现智能代理，这些智能代理可以对其环境进行推理，并基于该推理做出决策。Prolog 处理复杂数据结构和执行递归计算的能力使其非常适合于实现广泛的人工智能算法，包括搜索算法、规划算法和机器学习算法。

你准备好释放 Prolog 的力量了吗？Prolog 支持逻辑推理和回溯，是实现智能算法和解决复杂问题的强大工具。尝试“Nani Search”游戏或其他 Prolog 教程和示例，看看您可以用这种通用语言创建什么。现在就开始使用 Prolog，亲自发现它的强大功能！