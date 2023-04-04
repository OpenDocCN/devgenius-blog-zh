# 数据分析项目的全面演练

> 原文：<https://blog.devgenius.io/full-walk-through-of-a-data-analytics-project-d38604cfa06f?source=collection_archive---------6----------------------->

一个完整的数据分析项目是什么样子的(至少是一个简短的版本)

![](img/449bf0efdb30bc6d0bd47d343732a82d.png)

按年份划分的奥斯汀建筑许可

# 那么这些项目通常是如何开始的呢？

通常，如果你从事数据分析、数据工程、数据科学或 BI 工作，你的任务就是回答问题。但是，这些问题不仅仅是简单的问与答。它们有点深。如果说我在数据工程师和其他历史中学到了什么，那就是你很可能还不知道答案，即使你知道，你也需要证明它，并提供一个非常容易理解的视觉效果(只需要 5 秒钟或更少的时间来消化)。

由于我在一家不能分享工作的公司工作，这是我几周前想到的一个问题:

# 他们打算什么时候在我家附近建更多的商店？！

# 第一步:

## 提出正确的问题

并决定如何回答这个问题。大多数情况下，回答任何问题都有几种方法，通常你不得不从头开始，因为你最初的问题既愚蠢又不重要。我们将每一种回答问题的尝试称为途径、路线、方法或路径。由于我不从事建筑业，所以我的快速拨号上没有任何精力充沛、无所不知的奥斯汀商业人士，所以我选择了**路线**，根据我的邮政编码中建筑许可逐年增加的情况来预测商店可能何时建成。

这很可能不是很多人会有的问题，但我更倾向于“如何”解决基于数据的问题。

# 第二步:

## 我必须找到数据

经过一些快速搜索，我能够找到 austin.gov.data 的网站，你知道吗，这些数据是公开的；他们有一个 API——尽管功能有限。这意味着用它制作一个应用程序/网站可能是不可能的。但是因为这样做的目的不是为了真正赚钱，只是为了我的知识和与邻居的后院聊天，API 的限制不是太大的问题。

```
#the sites with the data
csv1 = '[https://data.austintexas.gov/resource/3syk-w9eu.csv'](https://data.austintexas.gov/resource/3syk-w9eu.csv')
all_data_exports = '[https://data.austintexas.gov/Building-and-Development/Issued-Construction-Permits/3syk-w9eu'](https://data.austintexas.gov/Building-and-Development/Issued-Construction-Permits/3syk-w9eu')
austin_data = '[https://data.austintexas.gov'](https://data.austintexas.gov')
```

# 第三步:

## 旋转你的 env/程序并加载一些数据(brahh brahh 摩托车声音*)

让我们假设您的环境已经设置好，我将在这个项目中使用 jupyter notebook。

加载到库中。有很多方法可以从其他文件中导入数据并加载到你的程序中，等等等等，我真的很懒。我只是将用户名、密码、api 密钥/令牌放入同一个程序的一个变量中。但是，是的，如果你打算分享你的程序，希望其他人也能下载并运行它，你也许应该把这些东西藏在一个文件里，让用户加载他们自己的凭证。

```
#load libraries
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
from pandas_profiling import ProfileReport
from sodapy import Socrata
# import qgrid #didn't work - not sure why
from pandas import option_context
import chart_studio
import chart_studio.plotly as py 
import chart_studio.tools as tls
import requests#one options for viewing in jupyter
pd.set_option('display.max_columns', None)username = 'your_username'
api_key = 'your_api_key'px.set_mapbox_access_token('you_app_token')
```

加载数据。我们要把所有这些东西放进熊猫的数据框里。熊猫是一个图书馆——但是我听到越来越多的科技招聘人员说“你需要知道熊猫的语言！”它不是一种语言，而是 python，它有函数和其他东西。但他们只是招聘人员——他们不应该对熊猫了解太多。

```
#it's a big file so i used the usecols param to limit the size
df = pd.read_csv('Issued_Construction_Permits-2.csv', usecols = ['Permit Type', 'Permit Type Desc', 'Permit Num', 'Permit Class Mapped','Permit Class','Work Class', 'Project Name', 'Description', 'Property Legal Description', 'Issued Date', 'Calendar Year Issued' ,'Project ID','Latitude','Longitude','Original Zip', 'Contractor Company Name'])print(df.memory_usage())
print(df.memory_usage().sum())
df.head()
```

![](img/8b00b6df7c32a2e2d14a0ea964f450cc.png)

上面的代码说明了这一点——但是这是基于 csv 的(所以下载并加载它)

# 第四步:

## 数据清理、数据清洗、过滤、数据剖析、数据质量——任何其他删除无用数据的短语，请告诉我。我们都知道我们需要更多的数据短语…

由于数据很快变大，我们应该过滤掉一些垃圾。比如基于某种条件的异常行。让我们过滤许可类型为商业，年份至少为 1980 年或更久，没有邮政编码的垃圾数据。另外，我想用 0 填充 NaN(表示空值或空白)单元格。我想将其中一列的数据类型从字符串/对象类型改为浮点型。绘制图形时需要这样做——字符串数据类型的数字不会正确绘制。记住这一点。

我们还将创建一个基于项目名称计算许可数量的列。必须有 y 轴的值。

```
#filters
min_year = 1980
df = df[df['Permit Class Mapped'] != 'Residential']
df = df[df['Calendar Year Issued'] >= min_year]
df = df[df['Original Zip'] != 0]
print(df.shape)#create count permits column
df = df.reset_index()
df['count_permits'] = df.groupby('Project Name')["index"].transform("count")#fill blank cells
df['Calendar Year Issued'] = df['Calendar Year Issued'].fillna(0)
df['Original Zip'] = df['Original Zip'].fillna(0)
df['Description'] = df['Description'].fillna(0)
df['Original Zip'] = df['Original Zip'].astype(float)#This gets us down to a 140k row dataframe. Manageable
```

# **第五步:**

## 制作可以绘图的数据框架

接下来，我们需要做一个分组数据框架(这就是我所说的)。但是你可以称它为聚合表或数据框架。原因是您不想绘制 140，000 行，您想要绘制几百或一千行——这很容易，不会占用很多内存。图形对象比文本对象大得多，所以分组数据帧是一个不错的选择。

我们将按邮政编码和发放年份分组，并按列统计每个组的许可数量。

```
grouped_df = pd.DataFrame(df.groupby(['Original Zip', 'Calendar Year Issued'])['count_permits'].count().reset_index())
print(grouped_df.shape)
grouped_df.head(
```

![](img/5cca2b3fb3c8ff286119224229b79475.png)

# 第六步:

## 一些看起来像什么的图:

```
fig = px.line(grouped_df
              , x="Calendar Year Issued"
              , y="count_permits"
              , color="Original Zip"
#               , line_group=""
              , height=500
             )
fig.show()
```

![](img/eb31af6310f75631c45ce4bab15a0e27.png)

```
#for this plot, we're just rearranging the grouped dataframe and #plotting it
grouped_df = grouped_df.sort_values(by=['Calendar Year Issued','count_permits'],ascending=True)
grouped_df['Original Zip'] = grouped_df['Original Zip'].astype(str)fig = px.bar(grouped_df
              , x="Calendar Year Issued"
              , y="count_permits"
              , color="Original Zip"
              , template='seaborn'
             )fig.update_layout(barmode='stack')fig.show()
```

![](img/2dd448bb1c5440cf32021211b568bfef.png)

好了，现在我们有了一个概念，哪一个邮政编码每年有最多的许可。不错的开始，推迟一些你老板想看到的事情。但是，我所有的老板通常都想要更多。这些都是表面水平，他们大多很无聊。他们告诉我一句名言，只有两个字:那又怎样？从没想过这些话会回来困扰我。

# 第七步:

## 整个项目的面包和黄油——讲述您的故事的可靠数据可视化

更大更好的视觉效果。使用 Mapbox 从 plotly 获得的散点图(look it 一个开源地图解决方案，您需要一个免费帐户才能使用)。下面的代码没有什么特别之处——我们只是调用一个 plotly 图形，并添加一些参数来制作地图。

基本上，我们所寻找的，就试图预测德克萨斯奥斯汀将在哪里建造下一个的**而言，是确定深绿色泡沫的趋势。深绿色气泡代表较新的许可证，紫色气泡代表较旧的许可证。因此，深绿色许可证的数量表明了建筑面积的增长。**

```
#enter in the zipcode you want to search 
#and the earliest year for which you want to see data
min_year = 2013df = df[df['Calendar Year Issued'] >= min_year]
df = df[df['Permit Class Mapped'] != 'Residential']
print(df.shape)# df = df.reset_index()df['count_permits'] = df.groupby('Project Name')["index"].transform("count")
df['count_permits'] = df['count_permits'].fillna(0)#display map
fig = px.scatter_mapbox(df
                        , lat="Latitude"
                        , lon="Longitude"
                        , color="Calendar Year Issued"
                        , text='Description'
                        , size="count_permits"
                        , color_continuous_scale=px.colors.diverging.PRGn
                        , zoom=10
                        , height=1000
                        , opacity=.15
                        , title='<b>Number of Permits per Project in Austin</b>'
                        , template='seaborn'
                       )fig.update_layout(
    mapbox_style="open-street-map"
)fig.show()
```

![](img/784dc907f7f346feb49858b9cdd5b8a4.png)

`open-street-map`

## 让我们再抛出一些方法来看这个，好吗？

这些地图的列表 mapbox.syles 为:

```
"open-street-map", "carto-positron", "carto-darkmatter", "stamen-terrain", "stamen-toner" or "stamen-watercolor"
```

![](img/501f44687c915d4e472617257d86f0f6.png)

`stamen-watercolor`

![](img/449bf0efdb30bc6d0bd47d343732a82d.png)

`"carto-positron"`、`"carto-darkmatter"`、`"stamen-terrain"`、`"stamen-toner"`

决定哪张地图最能说明你的故事，然后选择那张。用太多图表淹没大众，尤其是高层人士，不是个好主意。他们会问:“如果除了颜色之外，其他图表没有区别，那你为什么要把所有这些图表都给我看？”。

# **第 8 步:**

## 尽可能实现自动化

如果你不想让整个过程自动化，那么额外的一步也是不必要的，那就是使用 API 方法来收集数据。但是，这个站点有一些 API 限制，不允许我一次下载太多数据，所以，虽然我编写了这段代码，但我并没有实际使用它。这也是这些凭据变量发挥作用的地方。

```
#!/usr/bin/env python
#api version# Unauthenticated client only works with public data sets. Note 'None'
# in place of application token, and no username or password:
client = Socrata("data.austintexas.gov"
                 ,'your api key'
                 ,username="your email"
                 ,password="your pw"
                )# First 2000 results, returned as JSON from API / converted to Python list of
# dictionaries by sodapy.
results = client.get("3syk-w9eu"
                     , order='calendar_year_issued desc'
                     , where="calendar_year_issued in ('2013','2014','2015','2016','2017','2018','2019','2020','2021')"
                     , limit=20000 #something weird here - can't read data based on query parameters bc limit function seems to precede other functions
                    )# Convert to pandas DataFrame
results_df = pd.DataFrame.from_records(results)del(results)min_year = '2013-01-01'
zip_code = 78747results_df['original_zip'] = results_df['original_zip'].fillna(0)
results_df['calendar_year_issued'] = results_df['calendar_year_issued'].astype(int)
results_df['original_zip'] = results_df['original_zip'].astype(int)
results_df['issue_date'] = results_df['issue_date'].astype('datetime64[ns]')# results_df = results_df[results_df['issue_date'] >= min_year]
results_df = results_df[results_df['original_zip'] == zip_code]results_df = results_df.reset_index()
results_df['count_permits'] = results_df.groupby('project_id')["index"].transform("count")#Show df
print(results_df.shape)
results_df.head()
```

![](img/f9da72f97c9404dc464103d6ccc21929.png)

Api 代码抛出了这个

# 步骤 9:

## 深入了解关键洞察，因为他们 100%都想要更多。给一英寸，他们会要求一英里

现在，让我们把所有的代码，并运行它大部分在一个细胞，而我们在它，让我们添加一个更酷的图表，只是为了打动任何人谁读到这个项目。给人们留下深刻印象一直都是一件好事——你甚至不需要和陌生人说话，就能赢得他们的尊重。赢赢。

它们基本上都是相同的代码(我没有使用邮政编码列表，只是指定了一个邮政编码)，并添加了一个不同的地理地图数字。它们都还在情节性地使用——一致性是关键。

可能有更多的地图，但我还是很懒。这些就够了。如果人们不知道其他的选择，不展示他们不会伤害你。伟大的哲学工作由笑。

```
#enter in the zipcode you want to search 
#and the earliest year for which you want to see data
min_year = 2013
zip_code = 78747
zip_codes = ['78747','78744','78617','78612','78616','78719']df = df[df['Calendar Year Issued'] >= min_year]# df = df[df['Permit Class Mapped'] == 'Commercial']df1 = df[df['Original Zip'] == zip_code]
print(df1.shape)df1 = df1.reset_index()df1['count_permits'] = df1.groupby('Project Name')["index"].transform("count")#display map
fig = px.scatter_mapbox(df1
                        , lat="Latitude"
                        , lon="Longitude"
                        , color="Calendar Year Issued"
                        , text='Description'
                        , size="count_permits"
                        , color_continuous_scale=px.colors.diverging.PRGn
                        , zoom=12
                        , height=900
                        , opacity=.15
                        , title='<b>Number of Permits by Zipcode (78747)</b>'
                        , template='seaborn'
                       )fig1 = px.scatter_mapbox(df1
                        , lat="Latitude"
                        , lon="Longitude"
                        , color="Calendar Year Issued"
                        , text='Description'
                        , size="count_permits"
                        , color_continuous_scale=px.colors.diverging.PRGn
                        , zoom=12
                        , height=900
                        , opacity=.15
                        , title='<b>Number of Permits by Zipcode (78747)</b>'
                        , template='seaborn'
                       )fig.update_layout(
    mapbox_style="open-street-map"
)fig1.update_layout(
    mapbox_layers=[
        {
            "below": 'traces',
            "sourcetype": "raster",
            "sourceattribution": "United States Geological Survey",
            "source": [
                "[https://basemap.nationalmap.gov/arcgis/rest/services/USGSImageryOnly/MapServer/tile/{z}/{y}/{x](https://basemap.nationalmap.gov/arcgis/rest/services/USGSImageryOnly/MapServer/tile/{z}/{y}/{x)}"
            ]
        },
        {
            "sourcetype": "raster",
            "sourceattribution": "Government of Canada",
            "source": ["[https://geo.weather.gc.ca/geomet/](https://geo.weather.gc.ca/geomet/)?"
                       "SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&BBOX={bbox-epsg-3857}&CRS=EPSG:3857"
                       "&WIDTH=1000&HEIGHT=1000&LAYERS=RADAR_1KM_RDBR&TILED=true&FORMAT=image/png"],
        }
      ]
)fig.show()
fig1.show()
```

## 图 1:

![](img/0f6d9e10312b87ef48954b6c613f0c16.png)

## 图2:

![](img/c8e4c011ebd25e51a172f66135181a7e.png)

# 步骤 10:

## 我们还能补充什么呢？此外，他们还想看到什么呢？

我对这些许可证的描述有点好奇，所以我写了一些东西来展示更多的许可证。我原以为他们会更有趣——我错了。非常乏味的阅读

```
#show wider columns
with option_context('display.max_colwidth', 100):
    display(df1.Description.sample(20))
```

![](img/8f5397a5baee0c9a2717effad7f1803a.png)

# **第 11 步:**

## Data Prep(更多的是第 0 步，但无论如何)

我之前应该提到过这一点，但我不想在这篇文章中从头到尾增加一个新的步骤，但是要想知道你的数据是什么样的，一个简单的方法就是对它进行概要分析。熊猫侧写库很棒。运行此代码，您将非常清楚地了解数据集是什么—重复项、空值、指标、统计信息等。

```
profile = ProfileReport(df1
                        , title='Pandas Profiling Report'
                        , explorative=True
                       )
profile
```

![](img/b471a5a7d40efb2af77ab2e4286be679.png)![](img/f1ffad4722137c226f6e8714fc371320.png)![](img/5d33a688025d013ef3735466533117b6.png)

# 第 12 步:

## 统计学和机器学习？！

*好吧，我没有在这个项目中添加任何 ML——我可以添加。某种程度的回归也不会太难；计算每个 zipcode 每年的许可数量，并预测出还有多少年。我们本可以更进一步，通过聚类来预测新许可证的类型——我可能永远也不会这么说，但这是可能的。*

这将是您可以计算逐年百分比变化的部分，或不同属性的增长变化(78701 在过去 N 年中有 X%的变化)等。这不是我的目标——我的意思是我是提问者，所以我一定是老板！而我需要看到的只是趋势/势头。

许多组织喜欢通过数据驱动和提供实际数字来掩盖他们的踪迹。我理解这种担心，但最终，一个以 X 步调移动 40%或 60%的趋势不会改变我的想法。我相信亚马逊有一个价值主张，那就是“我们承担可计算的风险”——这一点一直困扰着我，因为你可能真的永远都在获取精确的数字，但精确的数字是一个人做出决策真正需要的。顺便说一句，如果你走进一个与杰夫·贝索斯的会议，你说“XYZ 的指标是 31.25%”，他可能会当场解雇你，因为你没有说 31%，甚至 30%，浪费了他的时间。

# 这应该可以了:

现在你只需要展示它。我猜是发邮件然后展示吧。可能想把它放入一个 powerpoint 演示文稿中——你必须弄清楚你的老板想要如何表达。

不管怎样，希望你们都喜欢这个——欢迎随时关注和评论任何问题。这都在我的 [github](https://github.com/maxwellbade/Austin-Building-Permits) btw 里。

最好，
Max