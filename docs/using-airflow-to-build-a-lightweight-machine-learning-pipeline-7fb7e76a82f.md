# 使用气流构建轻量级机器学习管道

> 原文：<https://blog.devgenius.io/using-airflow-to-build-a-lightweight-machine-learning-pipeline-7fb7e76a82f?source=collection_archive---------9----------------------->

有一天晚上，我在研究气流，在网上搜索看似简单的任务的建议时，我发现自己遇到了问题:

*   如何使用 Airflow 处理 CSV 文件？
*   如何使用 Airflow 将数据帧存储到数据库中？
*   如何在 Airflow 中使用 scikit-learn 创建一个简单的机器学习模型？

经过一些工作，我想出了一个简单的，工作的例子，详细如下。

# 气流设置

我不打算在这里花很多时间，因为这可能是一个帖子本身。我使用 [Astro CLI](https://docs.astronomer.io/astro/cli/overview) 来设置我所有的气流项目。它毫不费力，让我专注于我试图解决的问题，而不是花几个小时来建立气流。

# 数据

我从 Kaggle 获得了一个具有漫威超级英雄和反派属性的[数据集](https://www.kaggle.com/code/adilemrebilgic/marvel-super-heroes/data?select=charcters_stats.csv)。该文件跟踪属性，如智力，速度，力量和排列(好或坏)。我的假设是，我可以使用这些属性来预测好的或坏的对齐。

![](img/1589df1f4d637b6497d1229c89fb86ba.png)

是的，我知道“字符”拼写错误🤷‍♂️

# Python 函数

我做的第一件事是编写 Python 代码来处理 CSV 文件，并将其存储到本地 Postgres 数据库中。

我在我的气流项目**包括**文件夹中存储了我想要处理的所有 CSV 文件。这允许我在我的 Airflow 项目中拥有一个虚拟的“桌面”,我可以在其中存储和循环文件。

将 CSV 读入 pandas 数据帧后，我通过删除数据帧中的所有空值来进行基本的数据处理。

然后我建立了一个与 Airflow 提供的 Postgres 数据库的连接，我确信这将会引起一些争议。理想的设置是有一个单独的分析数据库来存储数据，而不是由 Airflow 提供的 Postgres 数据库。大多数 Airflow 专家还会告诉您通过 Airflow 中的**连接**特性建立到 Postgres 数据库的连接，并使用 Postgres 操作符来执行命令。

不过，**这个例子管用**。总会有对规模和复杂用例的考虑，但我有意保持简单。我的大部分开发工作都是在 Jupyter 笔记本上完成的，所以有一种简单的方法将这些工作转换成 Airflow DAG 对我来说很重要。这里概述的方法实际上允许我将工作从我的笔记本复制并粘贴到我的 Airflow DAG 中的 Python 操作符中，而没有太多的恐慌。

```
def upload_csv():
    #read csv file to df 
    df = pd.read_csv(AIRFLOW_HOME + '/include/charcters_stats.csv')
    #drop nulls
    df = df.dropna()
    #create engine
    engine = create_engine('postgresql://postgres:postgres@tatooine_3cc253-postgres-1:5432/postgres')
    #df to database
    df.to_sql('characters1', engine, if_exists='replace')
```

第二个函数更复杂，因为它执行用于训练和测试基本逻辑回归的代码。

我再次建立到 Airflow Postgres 数据库的连接，并使用 SQL 查询来检索我在步骤 1 中存储的数据。这很重要，因为我发现这种在任务之间传递数据的方法比使用 xcom 或其他特定于 Airflow 的技术要容易得多。

我创建了英雄和反派属性 **X** ，以及对齐描述 **Y** ，因此我可以创建一个训练和测试分割来适应逻辑回归。然后，我为每个字符生成预测，并将预测值连接回原始数据集中，这样我就可以计算模型的准确性。我将最终的数据帧存储在一个名为 **characters1scored** 的新表中，这样我就可以随时访问得分数据。

```
def retrieve_predict():
    # create engine
    engine = create_engine('postgresql://postgres:postgres@docker-postgres-1:5432/postgres')
    # retrieve data
    df = pd.read_sql("SELECT * FROM characters1",con=engine)
    # create X and Y
    X = df[['Intelligence','Strength','Speed','Durability','Power','Combat','Total']]

    y = df['Alignment']
    # create train and test split
    X_train, X_test, y_train, y_test = train_test_split(X, 
                                                        y, 
                                                        random_state=1, 
                                                        stratify=y)

    # choose classifier
    classifier = LogisticRegression(solver='lbfgs', random_state=1)
    # fit classifier 
    classifier.fit(X_train, y_train)
    # create predictions
    predictions = classifier.predict(X_test)
    # create dataframe with predicted v actual
    dfx = pd.DataFrame({"Prediction": predictions, "Actual": y_test})
    # concat with original df
    result = pd.concat([df, dfx], axis=1, join='inner')
    # send results to sql 
    result.to_sql('characters1scored', engine, if_exists='replace')
    # print accuracy score 
    print(accuracy_score(y_test, predictions))
```

# 达格

所有这些都被打包成一个 **DAG** (有向非循环图)，这是 Airflow 在代码的所有组件之间创建任务依赖和线性逻辑的方式。

我的 DAG 相当简单，因为它只包含两个由 Python 操作者运行的函数。随着这条管道的增长，我可以添加更多的功能，任务和相互依赖的逻辑。我确信这里有很多需要改进的地方，但是我有意保持简单，这样它就可以重新用于你自己的项目。

```
from airflow.models import DAG
from datetime import datetime
from airflow.operators.python import PythonOperator
import pandas as pd
import os
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score
import numpy as np 
from sqlalchemy import create_engine

AIRFLOW_HOME = os.getenv('AIRFLOW_HOME')

def upload_csv():
    #read csv file to df 
    df = pd.read_csv(AIRFLOW_HOME + '/include/charcters_stats.csv')
    #drop nulls
    df = df.dropna()
    #create engine
    engine = create_engine('postgresql://postgres:postgres@docker-postgres-1:5432/postgres')
    #df to database
    df.to_sql('characters1', engine, if_exists='replace')

def retrieve_predict():
    # create engine
    engine = create_engine('postgresql://postgres:postgres@tatooine_3cc253-postgres-1:5432/postgres')
    # retrieve data
    df = pd.read_sql("SELECT * FROM characters1",con=engine)
    # create X and Y
    X = df[['Intelligence','Strength','Speed','Durability','Power','Combat','Total']]

    y = df['Alignment']
    # create train and test split
    X_train, X_test, y_train, y_test = train_test_split(X, 
                                                        y, 
                                                        random_state=1, 
                                                        stratify=y)

    # choose classifier
    classifier = LogisticRegression(solver='lbfgs', random_state=1)
    # fit classifier 
    classifier.fit(X_train, y_train)
    # create predictions
    predictions = classifier.predict(X_test)
    # create dataframe with predicted v actual
    dfx = pd.DataFrame({"Prediction": predictions, "Actual": y_test})
    # concat with original df
    result = pd.concat([df, dfx], axis=1, join='inner')
    # send results to sql 
    result.to_sql('characters1scored', engine, if_exists='replace')
    # print accuracy score 
    print(accuracy_score(y_test, predictions))

with DAG(
    dag_id='csv_example',
    schedule_interval='@daily',
    start_date=datetime(2022, 3, 1),
    catchup=False
) as dag:

    # Upload the file
    t1 = PythonOperator(
        task_id='read_hero_villian_stats',
        python_callable=upload_csv,
    )

    t2 = PythonOperator(
        task_id='score_hero_villian',
        python_callable=retrieve_predict,
    )

    t1 >> t2 
```

# 结果呢

该模型的准确率为 71%，还有许多需要改进的地方。我认为试验不同的分类器和设计一些额外的特性可以极大地提高这个数字，但这是为了给未来打下一个简单的基础。

我也想象得分一个新的英雄(也许是 Homelander？)低智力和 max 属性剩下的路。这个虚构的新英雄(或反派)根据模型被归类为**好**。

```
new_data = np.array([[50,100,100,100,100,100,550]])
new_prediction = classifier.predict(new_data)
print(f"The new character was classified as: {new_prediction}")

###The new character was classified as: ['good'
```

# 结论

![](img/91a39fa11d8216db8e8da447429e3cf7.png)

虽然我们的模型有很大的改进空间，但我们成功地建立了一个管道来处理 CSV，将其存储到数据库中，并部署了一个简单的机器学习算法来预测二进制分类。由于有气流，它还可以每天自动运行！希望这可以作为一个有用的模板，帮助那些正在努力使用气流将数据科学项目转化为自动化管道的人。