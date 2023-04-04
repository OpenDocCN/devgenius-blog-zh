# 对熊猫的交易策略进行回溯测试

> 原文：<https://blog.devgenius.io/backtesting-trading-strategy-with-pandas-300ce7c1c9c1?source=collection_archive---------2----------------------->

回溯测试是根据历史数据评估交易策略表现的过程。将会有一系列的故事出版，记录回溯测试程序的发展。

![](img/f72fb013ff6fea58540d25331697849b.png)

[m.](https://unsplash.com/@m_____me?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

## 回溯测试移动平均交叉策略

我们将从测试一个简单的策略开始。也就是说，我们将使用移动平均线的简单交叉。我们将使用 pandas-ta 库来构建指标。

```
import pandas_ta as tadf["sma10"] = df.ta.sma(10)
df["sma30"] = df.ta.sma(30)
```

交叉的基本思想是，当更快的均线(SMA_10)穿过更慢的均线(SMA_30)时，你进入多头仓位，当相反的情况发生时，你平仓。因此，我们需要识别 SMA_10 在 SMA_30 之上的所有数据点，以及那些序列的开始。

```
df["over"] = df["sma10"] > df["sma30"]
df["cross_up"] = df["over"].ne(df["over"].shift()) & df["over"]
df["cross_down"] = df["over"].ne(df["over"].shift())& ~df["over"]
```

当前的 SMAs 比较与以前的不同是一个新趋势的标志。你可以在[这篇文章](/streaks-in-pandas-time-series-c63fe62aa771)中阅读更多关于识别熊猫条纹的信息。

## 运行回溯测试

我们需要遍历所有的行。一般来说，向量化操作是首选的，但是在回测时，操作依赖于序列中的前一个操作。我们将跟踪我们是否在适当的位置，因为这决定了在每个迭代中做什么。

```
for i in range(1, len(df)):
    if not in_trade[-1]:
        if df.loc[df.index[i - 1], "cross_up"]:
            in_trade.append(True)
            trades.append([df.index[i]])
        else:
            in_trade.append(False)
    else:
        if df.loc[df.index[i - 1], "cross_down"]:
            in_trade.append(False)
            trades[-1].append(df.index[i-1])
        else:
            in_trade.append(True)
```

如你所见，我们总是检查前一行的指标。我们这样做是为了模拟实时行为。澄清一下，当从交换中检索烛台数据时，我们总是得到最后一根完整的蜡烛。理论上，我们不能在这个价格开始建仓，而是在下一根蜡烛线开始时建仓。

交易清单记录我们是否已经到位。for 循环中的执行依赖于这种状态，也就是说，当我们处于适当位置时，我们正在寻找向上的交叉进入交易，反之亦然。我们也记录我们何时进入和关闭假设的位置。我们通过在交易列表中添加相应事件的指数来实现这一点。

## 计算 PnL 和假设费用

在迭代的最后，我们得到了每笔交易的开始和结束时间。我们的目标是计算他们的盈利能力。我们通过从最终价格中减去起始价格来做到这一点。为了使这个过程更加真实，我们还包括了一个假设的数量和一个假设的费用率。

```
quantity = 1
fee_rate = 0.0001

trade_df = pd.DataFrame(trades, columns=["start_time", "end_time"])
prices = pd.concat([
            df.loc[trade_df["start_time"], "Open"]
              .reset_index(drop=True),
            df.loc[trade_df["end_time"], "Open"]
              .reset_index(drop=True)
        ], axis=1).set_axis(["start_price", "end_price"], axis=1)

trade_df = pd.concat([trade_df, prices], axis=1)
```

在上面的代码中，我们通过过滤初始数据帧来选择交易时的价格。然后，我们使用定义的数量和费率来计算每笔交易的实际成本。

```
trade_df["start_filled"] = quantity * trade_df["start_price"]
trade_df["end_filled"] = quantity * trade_df["end_price"]

trade_df["buy_fee"] = fee_rate * trade_df["start_filled"]
trade_df["sell_fee"] = fee_rate * trade_df["end_filled"]
```

## 我们战略的盈利能力

每笔交易的 P&L 计算如下:

```
trade_df["P&L"] = (
        (trade_df["sell_filled"] - trade_df["sell_fee"]) -
        (trade_df["buy_filled"] + trade_df["buy_fee"])
)trade_df["P&L"].describe()
```

然后，我们运行 describe 命令来计算我们的策略的总体性能。

```
count 152.000000
mean 87.003663
std 458.396582
min -1867.960113
25% -139.295142
50% -3.247143
75% 177.310014
max 1745.465845
Name: P&L, dtype: float64
```

我们的策略做了 152 笔交易，平均 P&L 为 87 美元。我们还可以看到，交易之间有很大的差异，即我们最差的交易是亏损 1867.9 美元，我们最好的交易是盈利 1745.5 美元。

## 未来的改进

在未来的文章中，我们将增加额外的分析和策略。代码将使用面向对象的设计原则来构建，以进一步扩展其可用性。这个故事的所有代码都可以在我的 Github 上找到。

感谢您的阅读。