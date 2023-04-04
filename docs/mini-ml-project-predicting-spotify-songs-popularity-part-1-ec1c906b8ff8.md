# 迷你 ML 项目-预测 Spotify 歌曲的受欢迎程度。第一部分

> 原文：<https://blog.devgenius.io/mini-ml-project-predicting-spotify-songs-popularity-part-1-ec1c906b8ff8?source=collection_archive---------4----------------------->

![](img/c572ac5644e3013f25e48d7c25f2cd21.png)

*照片由 Pexels 的 Stas Knop 拍摄

最近，我有机会从事一个 ML 项目，这是我对数据科学的热情和职业道路的一部分。从 Technion 毕业，学习认知心理学和人机交互使我能够利用所获得的跨学科知识。

**简介&动机**

有着扎实的音乐教育，通过学习超过 10 年的长号和与各种乐队合作，我决定**开始**一个相当有趣的目标——根据歌曲的客观特征预测[**Spotify**](https://open.spotify.com/)歌曲的流行评分。而 Spotify 为每首歌曲提供了流行指数，这是相对于平台中所有其他歌曲计算的。这个排名对创作者的成功有巨大的财务影响，因为它影响他们的歌曲被推荐给其他用户和产生收入的程度。

预测宋的受欢迎程度的能力是相当复杂的挑战，原因是显而易见的；音乐，像任何类型的艺术一样，是主观的。具有相同客观特征(例如，音调、声学、可跳舞性和响度级别)的两首歌曲可能不会共享相同的流行度分数。作为一个艺人，我不会建议任何一个音乐人受到一个算法建议的影响。原创，真实，忠于自己的音乐风格才是最重要的。

尽管如此，解释 ML 模型的学习结果仍然是有价值的，以便获得关于什么和为什么各种类型的歌曲被认为是流行的见解。

在这个项目中，我实现了 3 个架子 ML 算法，以便对歌曲的流行度评分进行建模和预测。我还添加了一个交互式的“特征混合器”，它可以操纵歌曲的特征，并检查不同的模型如何对输入的微小变化做出反应。

这篇就职文章是关于一种试图解锁 Spotify 歌曲评分预测的算法。

**剧透…虽然 Yoggi Berra 的经典名言“很难预测，尤其是对未来的预测”是正确的，但利用 ML 我们可以获得非常接近的结果，因此提供了** [**数据民主化**](https://business.adobe.com/in/resources/data-democratization-is-crucial-to-your-business.html) **。**

下面列出了实现这一目标的“方法”

**初步数据分析**

投入实践。首先，我已经导入了相关的库，顺便提一下，我选择了使用 pandas 和 sklearn，它们被认为是用于此目的的标准库。我还使用了 ipywidgets，以便为测试模型的预测提供一个漂亮的交互式 GUI。

```
*import* numpy *as* np
*import* pandas *as* pd
*from* sklearn *import* datasets, linear_model
*from* sklearn.model_selection *import* train_test_split,GridSearchCV
*from* sklearn.ensemble *import* RandomForestRegressor
*from* sklearn.tree *import* DecisionTreeRegressor
*import* matplotlib.pyplot *as* plt *# data visualization
import* ipywidgets *as* widgets *# interactive widgets
from* ipywidgets *import* Box
```

接下来，我将在 [Kaggel](https://www.kaggle.com/datasets/zaheenhamidani/ultimate-spotify-tracks-db?select=SpotifyFeatures.csv) 中找到的 excel 文件重新加载到一个 dataframe 中，并运行一些描述性统计。

```
df = pd.read_csv('SpotifyFeatures.csv')
```

![](img/de6d2cc4541341f3f1042a156928d539.png)

连续变量的描述统计

该数据库包含超过 232，000 首歌曲，这些歌曲具有不同的声音、舞蹈性、能量、响度以及最重要的流行程度。

```
df.describe()
```

![](img/d6d8e857dd1048bd5cdc9e35163af476.png)

描述统计学

描述性统计使我能够发现潜在的异常和缺失数据。我也遇到了一些我感兴趣的修改，比如将歌曲时长从毫秒标准化为秒，忽略不必要的变量。此外，我检查了一些歌曲是否有多个实例。

**数据清理和修改**

```
***# Remove duplicates + unnecessary variables***df.drop_duplicates(subset=['track_id'], keep='first',inplace=True)
df.drop(['genre','artist_name','track_name','track_id','key'],axis=1, inplace=True)***# Data cleaning and arrangement***time_signature_df=pd.get_dummies(df["time_signature"])df = pd.concat([df,time_signature_df],axis=1)
df['mode'] = np.where(df['mode']=='Major', 1, 0)***# change songs duration from milliseconds to seconds*** df['duration_ms'] = df['duration_ms'] / 1000
df.rename(columns={'duration_ms': 'duration_s'}, inplace=True) 
```

在数据清理过程之后，歌曲数量减少到 176K。这种修改有望减少当同一首歌曲同时出现在训练集和测试集中时可能导致的潜在偏差。

**数据建模**

为了对数据建模，我声明了两个变量——X 是一个包含所有歌曲特性(除了流行度)的表，y 是一个包含歌曲流行度分数的向量。

首先，我准备了用于学习的数据:

```
X= df.loc[:,df.columns !="popularity"] # all the features accept DV
y = df["popularity"] # the DV
```

![](img/56e6cf9717f0e3ce930c7a7e1456efc0.png)

左栏代表歌曲指数，而右栏代表歌曲的流行度得分

接下来，我将数据库分为训练集和测试集。80%的测试用于训练，其余用于测试。

```
**# separate the data to training and testing**
X_train, X_test, y_train,y_test=train_test_split(X,y,test_size=0.2)
# save as np.array
X_train = np.array(X_train)
X_test = np.array(X_test)
y_train = np.array(y_train) 
y_test = np.array(y_test)
The y vector contains the songs’ “popularity” score
```

我选择了 3 个书架模型来竞争预测歌曲的流行度——(1)线性回归，(2)随机森林和(3)决策树。

```
***# create a linear regression, r*andom forest & decision tree *object*** model_regression = linear_model.LinearRegression()
model_random_forest = RandomForestRegressor()
model_decision_tree = DecisionTreeRegressor()
```

下一步(也是最长的一步)是训练模型:

```
model_regression.fit(X_train,y_train)
model_random_forest.fit(X_train,y_train)
model_decision_tree.fit(X_train,y_train)
```

**模型测试**

在这一部分，我估计了模型预测歌曲的成功率。我使用了 *score()* 函数来计算决定系数。分数越接近 1，说明回归变量越准确。

```
***# estimate the R² score on training and testing data
# (1) Linear regression*** model_regression.score(X_train,y_train)
model_regression.score(X_test,y_test)***# (2)* Random Forest** model_random_forest.score(X_train,y_train)
model_random_forest.score(X_test,y_test)***# (3) D*ecision Tree** model_decision_tree.score(X_train,y_train)
model_decision_tree.score(X_test,y_test)
```

**输出:**
线性回归:训练得分 **0.202** ，测试得分 **0.199**

随机森林:训练得分 **0.906** ，测试得分 **0.341**

决策树:培训得分 **0.998** ，测试得分 **(-0.350)**

令人惊讶的是，这三个模型的性能相当低。根据模型的得分，随机森林模型提供了最准确的预测。针对决策树模型，负得分意味着无论测试什么歌曲，该模型的性能都比仅仅猜测期望值差。

**评估和可视化**

在这一部分中，我将模型的预测可视化，并使用从测试数据中提取的 10 首歌曲来比较它们的准确性。

```
test_samples = 10 # amount of songs which would be evaluated**# initialized empty lists for the models predictions**
regression = [] 
random_forest = [] 
decision_tree = []
ground_truth = []**# collecting the models' predictions** 
for i in range(test_samples): 
    regression.append(model_regression.predict([X_test[i]])) 
    random_forest.append(model_random_forest.predict([X_test[i]]))
    decision_tree.append(model_decision_tree.predict([X_test[i]]))
    ground_truth.append(y_test[i])**# Plotting the models' predictions in comparison to the ground truth**
plt.plot(range(len(regression)), regression, label='Linear Regression')
plt.plot(range(len(random_forest)), random_forest, label='Random Forest')
plt.plot(range(len(decision_tree)), decision_tree, label='Decision Tree')
plt.plot(range(len(ground_truth)), ground_truth, label='Ground Truth')
plt.xlim([0, test_samples])
plt.ylim([0, 100])
plt.xlabel('songs')
plt.ylabel('popularity')
plt.legend()
plt.show()
```

![](img/171af6468e6e75e3fcf99a6cab7d6989.png)

由 3 个模型为 10 首随机歌曲提供的预测流行度得分。红线代表歌曲的真实流行分数。

关于上面提到的图表，红线代表歌曲的流行度得分，而其他 3 条线代表模型的预测。距离红线越远=精确度越低。可以观察到，所有 3 个模型并不完全符合红线，随机森林提供了最接近地面事实的预测。

此外，还可以观察到 3 个模型有不同的波动。线性回归似乎更“保守”，预测不太容易发生剧烈变化，而决策树预测对变化更“敏感”。

**“特征混合器”用于评估模型预测**

当处理人与人工智能的交互时，有助于成功合作的一个关键方面是人工智能的无形性和可解释性。虽然与人工智能合作的外行人通常不熟悉算法背后的数学模型。我们仍然可以通过可视化 AI 在遇到不同情况时的决策来增加他或她的理解。

这种解释技术通常被定义为事后模型不可知论解释。这种类型的解释没有真正揭示模型决策背后的机制，但仍然可以为我们提供关于模型之间差异的一般理解和直觉。

![](img/474839b7956d20c099598ad507ce2b31.png)

归功于 Pixabay

为了实现这个“特性混合器”的解释，我从收集特性范围值开始。大多数特征的范围在 0 到 1 之间，而其他特征，如节奏，范围在 0 到 250 (bpm)之间。

```
***# Variables ranges***acousticness = [0,1]
danceability = [0,1]
duration_s = [0,600]
energy = [0,1]
instrumentalness = [0,1]
liveness = [0,1]
loudness= [ -60,0]
speechiness = [0,1]
tempo = [0,250]
valence = [0,1]
mode = [0,1]
features_range = {"acousticness":[0,1],"danceability" : [0,1],"duration_s":[0,600],"energy":[0,1],"instrumentalness":[0,1],"liveness":[0,1],"loudness": [-60,0],"speechiness" : [0,1],"tempo" : [0,250],"valence" : [0,1],"mode" : [0,1]}
```

接下来，我为每一个特征生成了“推子”。

```
regression = []
random_forest = []
decision_tree = []
features = X.shape[1]
widgets_box = []
headers = X.columns
temp_sample =X.iloc[0]*for* feature *in* range(features):
   temp_widget = widgets.FloatSlider(value=temp_sample[feature],
   min=features_range[headers[feature]][0],
   max=features_range[headers[feature]][1],
   step=0.1,
   description=headers[feature],
   disabled=False,
   continuous_update=False,
   orientation='vertical',
   readout=True,
   readout_format='.1f',
   )
   widgets_box.append(temp_widget)**# display the features panel**
box = Box(children=widgets_box)
```

![](img/f86c816e472b3fc318dbc645de80fb46.png)

混音器包含用于操作的歌曲功能

最后，我操纵了推子，运行了下面的代码，这些代码存储并可视化了模型的预测。

```
***# Predict the popularity score based on the selected features*** *for* feature *in* range(features):
   temp_sample[feature] = widgets_box[feature].value
   regression.append(model_regression.predict([temp_sample]))
   random_forest.append(model_random_forest.predict([temp_sample]))
   decision_tree.append(model_decision_tree.predict([temp_sample]))***# plot a simple line chart***plt.plot(range(len(regression)), regression, label='Linear Regression')
plt.plot(range(len(random_forest)), random_forest, label='Random Forest')
plt.plot(range(len(decision_tree)), deciion_tree, label='Decision Tree')
plt.xlim([0, len(regression)])
plt.ylim([0, 100])
plt.legend()
plt.show()
```

![](img/013a374fa4b7369187ab9d1ee9b726e2.png)

x=1 是我生成的第一首歌。提供了 3 种不同的预测。

正如我们在上图中看到的，3 个模型提供了不同的预测。线性回归把我的歌评为 40/100，而决策把它评为 0/100。

这种可视化也支持多重测试和比较。正如下面可以看到的，我增加了歌曲的能量，活力，舞蹈性和节奏，然后重新运行相同的脚本。这些变化导致受欢迎程度排名上升:

![](img/044ee1a08407e5c2a7f84e4356d8aefb.png)

修改后的功能混合器状态

![](img/cce9fd9d71849a290bb1fbebcc23f3ee.png)

x=2，由模型提供的流行度分数。与初始投入相比，观察到总体增加。

该图能够很容易地比较在我做出改变之前和之后的 3 个模型的预测。决策树和随机森林模型都预测我的变化反映了受欢迎程度的提高，而线性回归预测则较低。

**结论和未来方向** 该项目的第一部分使我能够为训练、测试和可视化 ML 模型预测奠定基础。我即将面临的挑战是找到一种方法来提高我预测歌曲受欢迎程度的能力。这可能通过以下方式实现:(1)实现附加模型，例如深度学习模型(2)利用我之前忽略的分类特征(例如歌曲的风格)或者(3)尝试使用预定义的超参数来优化当前模型。

**参考资料&资源** *数据科学课程，Technion。以色列。
* ka ggle:Spotify database-[https://www . ka ggle . com/datasets/zaheenhamidani/ultimate-Spotify-tracks-db？select=SpotifyFeatures.csv](https://www.kaggle.com/datasets/zaheenhamidani/ultimate-spotify-tracks-db?select=SpotifyFeatures.csv)