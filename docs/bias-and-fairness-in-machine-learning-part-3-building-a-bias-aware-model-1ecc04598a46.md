# 机器学习中的偏见和公平，第 3 部分:建立偏见感知模型

> 原文：<https://blog.devgenius.io/bias-and-fairness-in-machine-learning-part-3-building-a-bias-aware-model-1ecc04598a46?source=collection_archive---------12----------------------->

## 文章

## *来自* [*特色工程图书营*](https://www.manning.com/books/feature-engineering-bookcamp?utm_source=medium&utm_medium=organic&utm_campaign=book_ozdemir_feature_11_11_21) *作者:思南·奥兹德米尔*

*本系列文章涵盖了*

*●识别并减少我们的数据和模型中的偏差*

*●通过各种指标量化公平性*

*●应用特征工程技术，在不牺牲模型性能的情况下消除模型偏差*

在[manning.com](https://www.manning.com/?utm_source=medium&utm_medium=organic&utm_campaign=book_ozdemir_feature_11_11_21)结账时，将 **fccozdemir** 输入折扣代码框，即可享受 35%的折扣。

## **建立偏见感知模型**

有关数据集、模型偏差和公平性的机制和重要性以及构建基本模型的信息，请查看[第 1 部分](https://manningbooks.medium.com/bias-and-fairness-in-machine-learning-part-1-introducing-our-dataset-and-the-problem-24f5f15c4f23)和[第 2 部分](https://manningbooks.medium.com/bias-and-fairness-in-machine-learning-part-2-building-a-baseline-model-and-features-358d13b39f1a)。

让我们开始使用两种特征工程技术来构建一个更具偏见意识的模型。我们将首先应用一个熟悉的转换来构造一个新的偏差较小的列，然后继续使用本书的特征提取方法。我们的目标是在不牺牲大量模型性能的情况下，最小化模型的偏差。

## **功能构建—使用 Yeo-Johnson 变压器处理不同的影响**

我们将做一些类似于 box-cox 变换的事情来变换我们的一些特征，以便使它们看起来更正常。为了说明为什么我们必须调查我们的模型低估了非裔美国人的累犯率的原因。一种方法是从我们的数据集中完全去除种族，并期望 ML 模型去除所有偏见。不幸的是，这很少是答案。

无特权人群和特权人群经历不同的机会，这可能通过相关特征呈现在数据中。我们的模型的偏见的最可能的原因是，至少我们的一个特征与种族高度相关，我们的模型能够通过这个特征重建某人的种族身份。要找到这个特征，我们先从找到我们数值特征的相关系数和身为非裔开始。

```
compas_df.corrwith(compas_df['race'] == 'African-American').sort_values()
 age              **-0.179095**
 juv_count         0.111835
 priors_count      **0.202897**
```

`age`和`priors_count`都与我们简单的非裔美国人的布尔标签高度相关，所以让我们仔细看看每一个。先看年龄。我们可以绘制一个直方图，并打印出一些基本的统计数据，我们会看到，在我们的四个种族类别中，年龄似乎相对相似，具有相似的均值、标准差和中位数。这向我们发出信号，即使年龄与非裔美国人负相关，这种关系也不是我们模型偏差的一个巨大贡献因素。

```
# Age is not very skewed
 compas_df.groupby('race')['age'].plot(
     figsize=(20,5),
     kind='hist', xlabel='Age', title='Histogram of Age'
 )
 compas_df.groupby('race')['age'].describe()
```

![](img/b59a205afc8df657cbd5ebda19bc0e61.png)

图一。按组别划分的年龄分布。上表表明年龄分布在不同组间没有显著差异，从而表明不同治疗和影响的影响较小。值得注意的是，非裔美国人的平均年龄和中位年龄比其他类别中确定的人年轻约 10-15 %,这可能是我们在年龄栏和非裔美国人身份栏之间看到强相关性的原因

让我们把注意力转向 priors_count 并做同样的打印输出。当我们这样做的时候，我们会看到一些年龄上的鲜明对比。

```
# Priors is extremely skewed by looking at the differences in mean/median/std across the racial categories
 compas_df.groupby('race')['priors_count'].plot(
     figsize=(20,5),
     kind='hist', xlabel='Count of Priors', title='Histogram of Priors'
 )
 compas_df.groupby('race')['priors_count'].describe()
```

![](img/1e0edf52bf7395b56b96c7237d557a40.png)

图二。乍一看，似乎所有种族的前科模式都遵循相似的模式。先验计数的分布显示了种族群体之间相似的右偏。然而，由于许多人无法控制的原因，非裔美国人的中位数和平均先验数几乎是其他群体的两倍。

值得注意的两件事:

1.  非裔美国人的前科严重右倾，其平均值是中值的两倍
2.  由于长期的系统性刑事司法问题，非裔美国人的前科几乎是其他种族总和的两倍

priors_count 与种族如此相关，并且对于不同的种族类别有不同的偏斜，这是一个巨大的问题，主要是因为 ML 模型可能会发现这一事实，并且仅仅通过查看 priors_count 列就对某些种族产生偏见。为了解决这个问题，我们将创建一个定制的转换器，通过将 **yeo-johnson 转换**应用到每个种族类别的值的子集来修改一个列。这将有助于消除本专栏对我们小组公平性的不同影响。

作为伪代码，它看起来会像

```
For each group label:
         Get the subset of priors_count values for that group
         Apply the yeo-johnson transformation to the subset
         Modify the column in place for that group label with the new values
```

通过对每个值子集应用转换，而不是将转换作为一个整体应用到列，我们强制每个组的值集为正态，平均值为 0，标准偏差为 1，这使得模型更难从给定的 priors_count 值重建特定的组标签。

让我们构建一个定制的 scikit-learn 转换器来执行这个操作。

**清单 1。通过 Yeo-Johnson 缓解不同的治疗**

```
from sklearn.preprocessing import PowerTransformer  # A
 from sklearn.base import BaseEstimator, TransformerMixin  # A

 class NormalizeColumnByLabel(BaseEstimator, TransformerMixin):
     def __init__(self, col, label):
         self.col = col
         self.label = label
         self.transformers = {}

     def fit(self, X, y=None):  # B
         for group in X[self.label].unique():
             self.transformers[group] = PowerTransformer(
                 method='yeo-johnson', standardize=True
             )
             self.transformers[group].fit(
                 X.loc[X[self.label]==group][self.col].values.reshape(-1, 1)
             )
         return self

     def transform(self, X, y=None):  # C
         C = X.copy()
         for group in X[self.label].unique():
             C.loc[X[self.label]==group, self.col] = self.transformers[group].transform(
                 X.loc[X[self.label]==group][self.col].values.reshape(-1, 1)
             )
         return C
```

**# A 进口**

**# B 为每组标签安装一个电源变压器**

**# C 当转换一个新的 D \数据帧时，我们使用我们已经安装的转换器的转换方法，并在适当的位置修改数据帧**

使用我们的新 transformer，让我们将它应用于我们的训练数据，以查看我们的先前计数已被修改，以便每个组标签的平均先前计数为 0，标准偏差为 1。

```
n = NormalizeColumnByLabel(col='priors_count', label='race')

 X_train_normalized = n.fit_transform(X_train, y_train)

 X_train_normalized.groupby('race')['priors_count'].hist(figsize=(20,5))
 X_train_normalized.groupby('race')['priors_count'].describe()
```

![](img/b40e644e9b5e88e7c661f5c110d87de8.png)

图 3。在对先前计数的每个子组子集应用 yeo-johnson 变换之后，分布开始看起来不那么偏斜并且彼此不同。这将使 ML 模型很难从这个特征中重建 race

**清单 2。我们的第一个偏见感知模型**

```
clf_tree_aware = Pipeline(steps=[
     ('normalize_priors', NormalizeColumnByLabel(col='priors_count', label='race')),  # A
     ('preprocessor', preprocessor),
     ('classifier', classifier)
 ])

 clf_tree_aware.fit(X_train, y_train)
 aware_y_preds = clf_tree_aware.predict(X_test)

 exp_tree_aware = dx.Explainer(clf_tree_aware, X_test, y_test, label='Random Forest DIR', verbose=False)  # B
 mf_tree_aware = exp_tree_aware.model_fairness(protected=race_test, privileged = "Caucasian")

 # performance is virtually unchanged overall
 pd.concat([exp.model_performance().result for exp in [exp_tree, exp_tree_aware]])

 # We can see a small drop in parity loss
 mf_tree.plot(objects=[mf_tree_aware], type='stacked')  # C
```

**# A 在我们的预处理器之前添加我们的新转换器，以在做任何其他事情之前修复 priors _ count
# B 检查我们的模型性能
# C 调查奇偶损失的变化**

我们新的偏差感知模型消除了不同的影响，效果非常好！我们实际上可以看到模型性能的小幅提升和累积奇偶校验损失的小幅下降。

![](img/ebacc379a77625423f98873ebc64b46f.png)![](img/498fbc8fb5adbc7e4fd57231ee387ed2.png)

图 4。顶栏代表我们的偏差感知模型的偏差指标总和，该模型在所有指标中的模型性能(在指标表中注明)都有小幅提升，除了 recall 没有变化。底部的条形图显示了我们之前看到的原始无偏差叠加图。总的来说，我们新的无偏倚模型在一些 ML 指标上表现得更好，并且根据我们的奇偶丢失条形图显示偏倚有所下降。我们在正确的轨道上！

## **特征提取—使用 AIF360 学习公平表示实现**

到目前为止，我们还没有做任何事情来解决我们的模型不知道敏感特性的问题。我们将使用 *AI Fairness 360* (aif360)，这是一个由 IBM 开发的开源工具包，用于帮助数据科学家获得预处理、处理中和后处理偏差缓解技术，以应用我们的第一个特征提取技术，称为*学习公平表示(LFR)* 。LFR 的想法是绘制我们的数据 x

对于我们的用例，我们将尝试将我们的分类变量(其中 4 / 6 代表种族)映射到一个新的“更公平”的向量空间，该向量空间保持统计奇偶性，并尽可能多地保留来自原始 x 的信息。

Aif360 使用起来可能有点棘手，因为他们强迫你使用他们自己版本的数据帧`BinaryLabelDataset`。下面是一个定制的 scikit-learn 转换器，它将:

1.  接受 X，一个由我们的分类预处理器创建的二进制值的数据帧
2.  将数据帧转换为二进制 LabelDataset
3.  安装 aif360 包装中的 LFR 模块
4.  使用现在拟合的 LFR 变换任何新数据集，将其映射到我们新的公平表示中

**清单 3。定制 LFR 变压器**

```
from aif360.algorithms.preprocessing.lfr import LFR
 from aif360.datasets import BinaryLabelDataset

 class LFRCustom(BaseEstimator, TransformerMixin):
     def __init__(self, col, protected_col, unprivileged_groups, privileged_groups):
         self.col = col
         self.protected_col = protected_col
         self.TR = None
         self.unprivileged_groups = unprivileged_groups
         self.privileged_groups = privileged_groups

     def fit(self, X, y=None):
         d = pd.DataFrame(X, columns=self.col)
         d['response'] = list(y)

         binary_df = BinaryLabelDataset(  # A
             df=d,
             protected_attribute_names=self.protected_col,
             label_names=['response']
         )

         # Input reconstruction quality - Ax
         # Output prediction error - Ay
         # Fairness constraint - Az

         self.TR = LFR(unprivileged_groups=self.unprivileged_groups,
                  privileged_groups=self.privileged_groups, seed=0,
                  k=2, Ax=0.5, Ay=0.2, Az=0.2,  # B
                  verbose=1
                 )
         self.TR.fit(binary_df, maxiter=5000, maxfun=5000)
         return self

     def transform(self, X, y=None):
         d = pd.DataFrame(X, columns=self.col)
         if y:
             d['response'] = list(y)
         else:
             d['response'] = False

         binary_df = BinaryLabelDataset(
             df=d,
             protected_attribute_names=self.protected_col,
             label_names=['response']
         )
         return self.TR.transform(binary_df).convert_to_dataframe()[0].drop(['response'], axis=1)  # B
```

**# A aif360 binary label dataset 对象的相互转换
# B 这些参数可以在 AIF 360 网站上找到，并且是通过离线网格搜索发现的**

为了使用我们的新转换器，我们需要稍微修改一下管道，并使用 FeatureUnion 对象。

**清单 4。具有不同冲击消除和 LFR 的型号**

```
categorical_preprocessor = ColumnTransformer(transformers=[
     ('cat', categorical_transformer, categorical_features)
 ])  # A

 # Right now the aif360 package can only support one privileged and one unprivileged group
 privileged_groups = [{'Caucasian': 1}]  # B
 unprivileged_groups = [{'Caucasian': 0}]  # B

 lfr = LFRCustom(
     col=['African-American', 'Caucasian', 'Hispanic', 'Other', 'Male', 'M'],
     protected_col=sorted(X_train['race'].unique()) ,
     privileged_groups=privileged_groups,
     unprivileged_groups=unprivileged_groups
 )

 categorical_pipeline = Pipeline([
     ('transform', categorical_preprocessor),
     ('LFR', lfr),
 ])

 numerical_features = ["age", "priors_count"]
 numerical_transformer = Pipeline(steps=[
     ('scale', StandardScaler())
 ])

 numerical_preprocessor = ColumnTransformer(transformers=[
         ('num', numerical_transformer, numerical_features)
 ])  # A

 preprocessor = FeatureUnion([  # C
     ('numerical_preprocessor', numerical_preprocessor),
     ('categorical_pipeline', categorical_pipeline)
 ])

 clf_tree_more_aware = Pipeline(steps=[  # D
     ('normalize_priors', NormalizeColumnByLabel(col='priors_count', label='race')),
     ('preprocessor', preprocessor),
     ('classifier', classifier)
 ])

 clf_tree_more_aware.fit(X_train, y_train)

 more_aware_y_preds = clf_tree_more_aware.predict(X_test)
```

**#A 隔离数值和分类预处理器，以便我们可以分别将 LFR 拟合到分类数据中
#B 告诉 aif360 高加索人标签为 1 的行具有特权，高加索人标签为 0 的行没有特权
#C 使用 FeatureUnion 合并我们的分类数据和数值数据
#D 我们的新管道将通过 yeo-johnson 移除不同的影响/处理，并将 LFR 应用到我们的分类数据中，以解决模型不可知问题**

简单地将 LFR 模块应用到我们的数据框架中需要很多代码。真正的唯一原因是需要将我们的 pandas 数据帧转换成 aif360 的自定义数据对象，然后再转换回来。现在我们已经拟合了我们的模型，让我们最后看看我们的模型的公平性。

```
exp_tree_more_aware = dx.Explainer(clf_tree_more_aware, X_test, y_test, label='Random Forest DIR + LFR', verbose=False)

 mf_tree_more_aware = exp_tree_more_aware.model_fairness(protected=race_test, privileged="Caucasian")

 pd.concat([exp.model_performance().result for exp in [exp_tree, exp_tree_aware, exp_tree_more_aware]])
```

我们可以看到，我们的最终模型与不同的影响消除和 LFR 应用有争议的比我们的原始基线模型更好的模型性能。

![](img/4ba22bfddb89bbbfc6d4c3e94cc0b962.png)

图 5。我们最终的偏倚感知模型已经提高了准确性、f1 和精确度，并且在召回率和 AUC 上只有微小的下降。这很棒，因为它表明，通过减少偏差，我们已经让我们的 ML 模型在更“经典”的指标如准确性方面同时表现得更好。双赢！

我们还希望检查我们的累积奇偶校验损失，以确保我们朝着正确的方向前进。

```
mf_tree.plot(objects=[mf_tree_aware, mf_tree_more_aware], type='stacked')
```

当我们检查我们的图时，我们可以看到我们的公平性指标也在下降！这是全方位的好消息。我们的模型在性能方面没有受到基线模型的影响，我们的模型也表现得更加公平。

![](img/690f24b3f082c4cc8f598677416f5696.png)

图 6。我们的最终偏差感知模型消除了不同的影响，LFR 是迄今为止最公平的模型。请再次记住，规模越小意味着偏见越少，这通常对我们更有利。在对我们的数据做了一些非常简单的转换后，我们在这里肯定会采取一些正确的措施来看到偏差的下降和模型性能的提高！

让我们最后一次看看我们的 dalex 模型公平性检查。回想一下我们的无意识模型，我们有 7 个数字超出了我们的范围(0.8，1.25)，我们在 4 / 5 的指标中检测到了偏差。

```
mf_tree_more_aware.fairness_check()  # 4 / 15 numbers out of the range of (08, 1.25)
 Bias detected in **3** metrics: TPR, FPR, STP

 Conclusion: your model is not fair because 2 or more criteria exceeded acceptable limits set by epsilon.

 Ratios of metrics, based on 'Caucasian'. Parameter 'epsilon' was set to 0.8 and therefore metrics should be within (0.8, 1.25)
                        TPR       ACC       PPV       FPR       STP
 African-American  **1.626829**  1.058268  1.198953  **1.538095  1.712329**
 Hispanic          1.075610  1.102362  0.965096  0.828571  0.893836
 Other             0.914634  0.996850  0.806283  1.100000  0.962329
```

我们现在只有 3 个指标超出了我们的范围，而以前是 7 个，现在只在 3 个指标中检测到偏差，而不是 4 个。总之，我们的工作似乎稍微改善了我们的模型性能，同时减少了我们的累积宇称损失。

我们已经在这些数据上做了很多工作，但是我们会放心地提交这个被认为是准确和公平的累犯预测模型吗？**绝对不行！**我们在本系列文章中的工作仅仅触及了偏见和公平意识的表面，并且只关注了一些预处理技术。我们甚至还没有开始深入讨论我们可用的其他形式的偏差缓解。

## **总结**

*   模型的公平性同样重要，有时甚至比模型本身的性能更重要
*   在我们的模型中，有多种定义公平的方式，每种都有利弊
*   考虑到我们数据中的相关因素，仅仅不知道一个模型通常是不够的
*   统计均等和均等几率是公平的两种常见定义，但有时会相互矛盾
*   我们可以在训练模型之前、之中和之后减轻偏差
*   完全不同的影响消除和公平表示的学习帮助我们的模型变得更加公平，也导致了模型性能的小幅提升
*   仅仅预处理是**不足以减轻偏差。我们还必须研究加工中和后加工方法，以进一步减少我们的偏差**

本文章系列到此为止。

如果你想了解这本书的更多信息，可以在曼宁的 liveBook 平台上查看[这里](https://livebook.manning.com/book/feature-engineering-bookcamp?origin=product-look-inside&utm_source=medium&utm_medium=organic&utm_campaign=book_ozdemir_feature_11_11_21)。