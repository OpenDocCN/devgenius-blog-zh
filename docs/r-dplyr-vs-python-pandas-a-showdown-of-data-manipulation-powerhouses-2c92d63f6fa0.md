# R dplyr vs Python pandas:数据操作巨头的摊牌

> 原文：<https://blog.devgenius.io/r-dplyr-vs-python-pandas-a-showdown-of-data-manipulation-powerhouses-2c92d63f6fa0?source=collection_archive---------7----------------------->

![](img/1d93a2b39b960871890d130cc4424412.png)

数据操作和分析在许多领域都是至关重要的任务，能够快速方便地操作和分析大型数据集非常重要。两个流行的数据操作和分析工具是 Python pandas 和 dplyr。

虽然这两个工具有一些相似之处，但它们的设计和使用是不同的。在本文中，我们将比较 pandas 和 dplyr，突出它们的主要区别，并展示如何使用它们来解决常见的数据操作问题，使用常见的和高级的功能，如过滤、排列、分组、选择、连接和窗口功能。

我们将使用虚构的住房数据来展示每种工具的能力。

**滤镜**

一个常见的数据操作任务是根据特定标准过滤数据。pandas 和 dplyr 都提供了过滤数据的工具。

以下是如何使用 pandas 过滤住房数据的示例:

```
import pandas as pd

# load the data from a CSV file
df = pd.read_csv('housing_data.csv')

# filter the data frame to only include houses with a price greater than $500,000, at least 3 bedrooms, and a square footage greater than 1,500
df = df[(df['price'] > 500000) & (df['bedrooms'] >= 3) & (df['sq_ft'] > 1500)]

# view the first few rows of the filtered data
df.head()
#   house_id     date     price  bedrooms  sq_ft  bathrooms  zipcode
#        3   1/3/2020  1020000         3   1600          2    90210
#        4   1/4/2020  1030000         3   1700          2    90210
#        5   1/5/2020  1040000         3   1800          2    90210
#        6   1/6/2020  1050000         3   1900          2    90210
```

以下是如何使用 dplyr 过滤住房数据的示例:

```
# install and load the dplyr package
install.packages("dplyr")
library(dplyr)

# load the data from a CSV file
df <- read.csv('housing_data.csv')

# filter the data frame to only include houses with a price greater than $500,000, at least 3 bedrooms, and a square footage greater than 1,500
df <- df %>%
  filter(price > 500000, bedrooms >= 3, sq_ft > 1500)

# view the first few rows of the filtered data
head(df)
#   house_id     date     price  bedrooms  sq_ft  bathrooms  zipcode
#        3   1/3/2020  1020000         3   1600          2    90210
#        4   1/4/2020  1030000         3   1700          2    90210
#        5   1/5/2020  1040000         3   1800          2    90210
#        6   1/6/2020  1050000         3   1900          2    90210
```

**安排**

另一个常见的数据操作任务是根据某些标准对数据进行排序。pandas 和 dplyr 都提供了对数据进行排序的工具。

以下是如何使用 pandas 对住房数据进行排序的示例:

```
# sort the data frame by the 'price' column in descending order, and then by the 'bedrooms' column in ascending order
df = df.sort_values(['price', 'bedrooms'], ascending=[False, True])

# view the first few rows of the sorted data frame
df.head()
#   house_id     date     price  bedrooms  sq_ft  bathrooms  zipcode
#        6   1/6/2020  1050000         3   1900          2    90210
#        5   1/5/2020  1040000         3   1800          2    90210
#        4   1/4/2020  1030000         3   1700          2    90210
#        3   1/3/2020  1020000         3   1600          2    90210
```

以下是如何使用 dplyr 对住房数据进行排序的示例:

```
# sort the data frame by the 'price' column in descending order, and then by the 'bedrooms' column in ascending order
df <- df %>%
  arrange(desc(price), bedrooms)

# view the first few rows of the sorted data frame
head(df)
#   house_id     date     price  bedrooms  sq_ft  bathrooms  zipcode
#        6   1/6/2020  1050000         3   1900          2    90210
#        5   1/5/2020  1040000         3   1800          2    90210
#        4   1/4/2020  1030000         3   1700          2    90210
#        3   1/3/2020  1020000         3   1600          2    90210 
```

**分组依据**

另一个常见的数据操作任务是根据特定标准对数据进行分组，然后对这些组执行操作。pandas 和 dplyr 都提供了数据分组工具。

以下是如何使用 pandas 对住房数据进行分组的示例:

```
# group the data frame by the 'zipcode' column and calculate the mean price for each group
df = df.groupby('zipcode')['price'].mean()

# view the resulting data frame
df
# zipcode
# 90210    1040000
# 90211    1050000
# 90212    1020000
```

以下是如何使用 dplyr 对住房数据进行分组的示例:

```
# group the data frame by the 'zipcode' column and calculate the mean price for each group
df <- df %>%
  group_by(zipcode) %>%
  summarise(mean_price = mean(price))

# view the resulting data frame
df
# zipcode  mean_price
# 90210    1040000
# 90211    1050000
# 90212    1020000
```

**选择**

另一个常见的数据操作任务是从数据框中选择特定的列或行。pandas 和 dplyr 都提供了选择数据的工具。

以下是如何使用 pandas 从住房数据框中选择列的示例:

```
# select the 'date', 'price', and 'zipcode' columns from the data frame
df = df[['date', 'price', 'zipcode']]

# view the first few rows of the resulting data frame
df.head()
#      date   price    zipcode
#   1/3/2020  1020000    90210
#   1/4/2020  1030000    90210
#   1/5/2020  1040000    90210
#   1/6/2020  1050000    90210
```

以下是如何使用 dplyr 从住房数据框中选择列的示例:

```
# select the 'date', 'price', and 'zipcode' columns from the data frame
df <- df %>%
  select(date, price, zipcode)

# view the first few rows of the resulting data frame
head(df)
#    date    price    zipcode
#  1/3/2020  1020000    90210
#  1/4/2020  1030000    90210
#  1/5/2020  1040000    90210
#  1/6/2020  1050000    90210
```

**加入**

另一个常见的数据操作任务是连接来自多个来源的数据。pandas 和 dplyr 都提供了连接数据的工具。

以下是如何使用 pandas 连接`house_id`列上的两个住房数据框的示例:

```
# load the second data frame from a CSV file
df2 = pd.read_csv('housing_data_2.csv')

# join the two data frames on the 'house_id' column, using an inner join to only include rows with matching values in both data frames
df = df.join(df2, on='house_id', how='inner')

# view the first few rows of the resulting data frame
df.head()
#   house_id     date     price  zipcode       city     state  year_built
#        3   1/3/2020  1020000    90210  Beverly Hills    CA        1950
#        4   1/4/2020  1030000    90210  Beverly Hills    CA        1960
#        5   1/5/2020  1040000    90210  Beverly Hills    CA        1970
#        6   1/6/2020  1050000    90210  Beverly Hills    CA        1980
```

以下是如何使用 dplyr 连接`house_id`列上的两个房屋数据框的示例:

```
# load the second data frame from a CSV file
df2 <- read.csv('housing_data_2.csv')

# join the two data frames on the 'house_id' column, using an inner join to only include rows with matching values in both data frames
df <- df %>%
  inner_join(df2, by='house_id')

# view the first few rows of the resulting data frame
head(df)
#   house_id     date     price  zipcode       city     state  year_built
#        3   1/3/2020  1020000    90210  Beverly Hills    CA        1950
#        4   1/4/2020  1030000    90210  Beverly Hills    CA        1960
#        5   1/5/2020  1040000    90210  Beverly Hills    CA        1970
#        6   1/6/2020  1050000    90210  Beverly Hills    CA        1980
```

我们可以使用 dplyr 和 pandas 处理更多的连接操作，例如 left_join、right_join 和 full_join。

**窗口功能**

另一个高级数据操作功能是使用窗口函数，它允许您跨与当前行相关的一组行执行计算。pandas 和 dplyr 都提供了使用窗口函数的工具。

下面是一个如何使用 pandas 计算当前行`price`和前一行`price`之间的差异的例子:

```
# define the window function that calculates the difference between the current rows 'price' and the previous rows 'price'
def price_diff(df):
  return df['price'] - df['price'].shift(1)

# apply the window function to the data frame, grouping by the 'zipcode' column
df = df.groupby('zipcode').apply(price_diff)

# view the first few rows of the resulting data frame
df.head()
#   house_id     date  price  zipcode       city        state  year_built price_diff
#        3  1/3/2020  1020000    90210  Beverly Hills    CA        1950       NaN
#        4  1/4/2020  1030000    90210  Beverly Hills    CA        1960     10000
#        5  1/5/2020  1040000    90210  Beverly Hills    CA        1970     10000
#        6  1/6/2020  1050000    90210  Beverly Hills    CA        1980     10000
```

下面是一个如何使用 pandas 来计算当前行`price`和前一行`price`之间的差异的例子:

```
# define the window function that calculates the difference between the current rows 'price' and the previous rows 'price'
price_diff <- function(df) {
  df %>%
    mutate(price_diff = price - lag(price))
}

# apply the window function to the data frame, grouping by the 'zipcode' column
df <- df %>%
  group_by(zipcode) %>%
  price_diff()

# view the first few rows of the resulting data frame
head(df)
#   house_id     date     price  zipcode       city     state  year_built price_diff
#        3   1/3/2020  1020000    90210  Beverly Hills    CA        1950        NA
#        4   1/4/2020  1030000    90210  Beverly Hills    CA        1960     10000
#        5   1/5/2020  1040000    90210  Beverly Hills    CA        1970     10000
#        6   1/6/2020  1050000    90210  Beverly Hills    CA        1980     10000
```

除了在窗口函数中使用 lag 之外，还有更常用的函数，如 row_number、rank 和 dense_rank。lag 的用法只是为了展示我们如何使用 pandas 和 dplyr 来处理窗口函数。

**结论**

总之，pandas 和 dplyr 都是数据操作和分析的强大工具。虽然它们有一些相似之处，但它们的设计和使用方式不同，而且各有优缺点。通过了解 pandas 和 dplyr 之间的主要区别，您可以为您的数据操作和分析任务选择正确的工具。