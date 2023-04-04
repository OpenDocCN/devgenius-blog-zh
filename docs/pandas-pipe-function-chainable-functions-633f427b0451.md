# 熊猫管道函数—可链接的函数

> 原文：<https://blog.devgenius.io/pandas-pipe-function-chainable-functions-633f427b0451?source=collection_archive---------11----------------------->

![](img/4d5adcf28a8a794ec7b21021d3dce21e.png)

图片来自 Pixabay 的 Manfred Richter

在数据处理过程中，我们应用了多种功能，如添加额外的列、更改现有列的详细信息、统计计算、拆分或替换数据帧上的值，以获得所需的结果。

为了简单起见，我们将使用来自**熊猫的 **pipe()** 方法。**

熊猫从 0.16.2 版本开始引入`pipe()`。`pipe()`在方法链中启用用户定义的方法。

现在开发人员可以编写可链接的代码，可读性更好。

> 语法: **DataFrame.pipe(func，*args，**kwargs)**

公文链接— [pipe()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pipe.html)

创建 3 个函数，我们将使用这些函数作为管道中的可链接函数。

```
# Adding Age column to Dataframe
def add_age_column(df):
    df["age"] = [23,24,22,22,21,22,23,24,24]
    return df

# Adding date column and passing list of values
def add_date_column(df, date: list):
    df["date"] = date
    return df

# adding multiple columns with values using dict
def add_multi_columns(df, data: dict):
    for key, value in data.items():
        df[key] = value

    return df
```

让我们创建一个熊猫数据框架并应用这些可链接的函数

```
import pandas as pd
import numpy as np

data = {'name': ['Ted'] * 3 + ['Lisa'] * 3 + ['Sam'] * 3,
        'subject': ['math', 'physics', 'history'] * 3,
        'score': np.random.randint(60, 100, 9)}

df = pd.DataFrame(data)

# o/p

#    name  subject  score  
# 0   Ted     math     60   
# 1   Ted  physics     67   
# 2   Ted  history     97   
# 3  Lisa     math     65   
# 4  Lisa  physics     66
```

首先，我们将使用 **add_age_column** ，它会将代码中定义的常量列表值添加到 dataframe 的 **age** 列中。

```
df.pipe(add_constant_column)

# o/p

#    name  subject  score  age
# 0   Ted     math     60   23
# 1   Ted  physics     67   24
# 2   Ted  history     97   22
# 3  Lisa     math     65   22
# 4  Lisa  physics     66   21
```

同样，我们将使用其余两个函数 **add_date_column** 和 **add_multi_columns。**

在 **add_date_column** 函数中，我们给出列表参数，在 **add_multi_columns** 函数中，我们给出字典参数。

```
df.pipe(add_date_column, ["10/12/23", "1/4/22", "11/1/20"]*3)

# o/p

#    name  subject  score  age      date
# 0   Ted     math     60   23  10/12/23
# 1   Ted  physics     67   24    1/4/22
# 2   Ted  history     97   22   11/1/20
# 3  Lisa     math     65   22  10/12/23
# 4  Lisa  physics     66   21    1/4/22

df.pipe(add_multi_columns, {'status': ['Fail', 'Pass', 'Pass'] * 3,
        'intelligent_score': np.random.randint(60, 100, 9)})

# o/p

#    name  subject  score  age      date status  intelligent_score
# 0   Ted     math     60   23  10/12/23   Fail                 72
# 1   Ted  physics     67   24    1/4/22   Pass                 81
# 2   Ted  history     97   22   11/1/20   Pass                 85
# 3  Lisa     math     65   22  10/12/23   Fail                 96
# 4  Lisa  physics     66   21    1/4/22   Pass                 60
```

我们也可以在一个链中使用所有的函数。

```
 (df.pipe(add_age_column)
  .pipe(add_date_column, ["10/12/23", "1/4/22", "11/1/20"]*3)
  .pipe(add_multi_columns, {'status': ['Fail', 'Pass', 'Pass'] * 3,
        'intelligent_score': np.random.randint(60, 100, 9)}))

# o/p

#    name  subject  score  age      date status  intelligent_score
# 0   Ted     math     60   23  10/12/23   Fail                 72
# 1   Ted  physics     67   24    1/4/22   Pass                 81
# 2   Ted  history     97   22   11/1/20   Pass                 85
# 3  Lisa     math     65   22  10/12/23   Fail                 96
# 4  Lisa  physics     66   21    1/4/22   Pass                 60
```

完整代码。

```
# Pandas pipe 

import pandas as pd
import numpy as np

data = {'name': ['Ted'] * 3 + ['Lisa'] * 3 + ['Sam'] * 3,
        'subject': ['math', 'physics', 'history'] * 3,
        'score': np.random.randint(60, 100, 9)}

# Adding Age column to Dataframe
def add_age_column(df):
    df["age"] = [23,24,22,22,21,22,23,24,24]
    return df

# Adding date column and passing list of values
def add_date_column(df, date: list):
    df["date"] = date
    return df

# adding multiple columns with values using dict
def add_multi_columns(df, data: dict):
    for key, value in data.items():
        df[key] = value

    return df

# Dataframe
df = pd.DataFrame(data)

# Pipe
df.pipe(add_age_column)

# o/p

#    name  subject  score  age
# 0   Ted     math     60   23
# 1   Ted  physics     67   24
# 2   Ted  history     97   22
# 3  Lisa     math     65   22
# 4  Lisa  physics     66   21

df.pipe(add_date_column, ["10/12/23", "1/4/22", "11/1/20"]*3)

# o/p

#    name  subject  score  age      date
# 0   Ted     math     60   23  10/12/23
# 1   Ted  physics     67   24    1/4/22
# 2   Ted  history     97   22   11/1/20
# 3  Lisa     math     65   22  10/12/23
# 4  Lisa  physics     66   21    1/4/22

df.pipe(add_multi_columns, {'status': ['Fail', 'Pass', 'Pass'] * 3,
        'intelligent_score': np.random.randint(60, 100, 9)})

# o/p

#    name  subject  score  age      date status  intelligent_score
# 0   Ted     math     60   23  10/12/23   Fail                 72
# 1   Ted  physics     67   24    1/4/22   Pass                 81
# 2   Ted  history     97   22   11/1/20   Pass                 85
# 3  Lisa     math     65   22  10/12/23   Fail                 96
# 4  Lisa  physics     66   21    1/4/22   Pass                 60

# complete pipeline in single flow

(df.pipe(add_age_column)
  .pipe(add_date_column, ["10/12/23", "1/4/22", "11/1/20"]*3)
  .pipe(add_multi_columns, {'status': ['Fail', 'Pass', 'Pass'] * 3,
        'intelligent_score': np.random.randint(60, 100, 9)}))

# o/p

#    name  subject  score  age      date status  intelligent_score
# 0   Ted     math     60   23  10/12/23   Fail                 72
# 1   Ted  physics     67   24    1/4/22   Pass                 81
# 2   Ted  history     97   22   11/1/20   Pass                 85
# 3  Lisa     math     65   22  10/12/23   Fail                 96
# 4  Lisa  physics     66   21    1/4/22   Pass                 60 
```

下一篇文章再见😊！

编码快乐！！！