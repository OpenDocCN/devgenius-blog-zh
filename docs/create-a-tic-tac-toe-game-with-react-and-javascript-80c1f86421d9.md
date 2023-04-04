# 用 React 和 JavaScript 创建一个井字游戏

> 原文：<https://blog.devgenius.io/create-a-tic-tac-toe-game-with-react-and-javascript-80c1f86421d9?source=collection_archive---------7----------------------->

![](img/94c5703fb372cb3c565a76fa467e0d08.png)

照片由[米歇尔·亨德森](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个易于使用的 JavaScript 框架，让我们可以创建前端应用程序。

在本文中，我们将看看如何用 React 和 JavaScript 创建一个井字游戏。

# 创建项目

我们可以用 Create React App 创建 React 项目。

要安装它，我们运行:

```
npx create-react-app tic-tac-toe
```

和 NPM 一起创建我们的 React 项目。

# 创建井字游戏

为了创建井字游戏，我们编写:

```
import React, { useMemo, useState } from "react";export default function App() {
  const [board, setBoard] = useState([[], [], []]); const restart = () => {
    setBoard([[], [], []]);
  }; const isWinner = (player) => {
    return (
      (board[0][0] === player &&
        board[0][1] === player &&
        board[0][2] === player) ||
      (board[1][0] === player &&
        board[1][1] === player &&
        board[1][2] === player) ||
      (board[2][0] === player &&
        board[2][1] === player &&
        board[2][2] === player) ||
      (board[0][0] === player &&
        board[1][0] === player &&
        board[2][0] === player) ||
      (board[0][1] === player &&
        board[1][1] === player &&
        board[2][1] === player) ||
      (board[0][2] === player &&
        board[1][2] === player &&
        board[2][2] === player) ||
      (board[0][0] === player &&
        board[1][1] === player &&
        board[2][2] === player) ||
      (board[0][2] === player &&
        board[1][1] === player &&
        board[2][0] === player)
    );
  }; const xWins = useMemo(() => {
    return isWinner("x");
  }, [board, isWinner]); const oWins = useMemo(() => {
    return isWinner("o");
  }, [board, isWinner]); if (xWins || oWins) {
    if (xWins) {
      return (
        <>
          <div>x wins</div>
          <button onClick={restart}>restart</button>
        </>
      );
    } if (oWins) {
      return (
        <>
          <div>o wins</div>
          <button onClick={restart}>restart</button>
        </>
      );
    }
  } else {
    return (
      <div className="App">
        <form>
          {Array(3)
            .fill()
            .map((_, i) => {
              return (
                <div key={i}>
                  {Array(3)
                    .fill()
                    .map((_, j) => {
                      return (
                        <span key={j}>
                          <input
                            value={board[i][j] || ""}
                            onChange={(e) => {
                              const b = [...board];
                              b[i][j] = e.target.value;
                              setBoard(b);
                            }}
                            style={{ width: 20 }}
                          />
                        </span>
                      );
                    })}
                </div>
              );
            })}
        </form>
      </div>
    );
  }
}
```

我们有保存井字游戏值的`board`状态。

函数`restart`调用`setBoard`来清除棋盘。

然后我们有了`isWinner`函数，它检查`player`是否以玩家获胜的方式填充了棋盘。

我们检查每个可能的组合，看看玩家是否赢了，包括水平、垂直和对角线组合。

然后我们用带有回调的`useMemo`定义`xWins`和`oWins`，回调用玩家字符串调用`isWinner`以查看玩家是否获胜。

我们观察`board`和`isWinner`变量的更新。

然后我们检查`xWins`和`oWins`。

在这两种情况下，我们都显示谁赢了和重新启动按钮。

否则，我们将嵌套数组呈现到输入网格中。

每个输入都将`value`设置为`board[i][j]`值。

然后，为了在用户输入时更新该值，我们将`onChange`属性设置为一个函数，该函数复制`board`并将其设置为`b`。

然后我们将`b[i][j]`设置为`e.target.value`。

然后我们用`b`调用`setBoard`来更新公告板。

我们将输入样式设置为宽度 20px。

# 结论

我们可以用 React 和 JavaScript 创建一个井字游戏。