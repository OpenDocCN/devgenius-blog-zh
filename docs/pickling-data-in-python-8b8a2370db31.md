# Python 中的酸洗数据

> 原文：<https://blog.devgenius.io/pickling-data-in-python-8b8a2370db31?source=collection_archive---------13----------------------->

使用`pickle`模块存储变量的值以便在下一个会话中重复使用，这很简单。

假设您已经处理了一个包含上百行的数据帧，并且您必须终止会话。你真的想事先损失几分钟的运行时间吗？当然不是，怎么保存数据？

# 以 Pickle 格式存储数据

以 pickle 格式或*pickle*存储数据是将 Python 对象序列化为二进制格式并保存到文件中。在这里，您可以遵循以下步骤。

```
>>> import pickle
>>> import pandas as pd# assume you have a large pandas dataframe as my_df 
# you are going to save to my_data.pickle
# extension name is optional 
# I prefer .pickle to remind me that file is pickle>>> pickle.dump( my_df, open('my_data.pickle', 'wb'))# now you have my_data.pickle in your disk
```

# 从 Pickle 格式读取数据

这个过程与上一节相反，通过*将 pickle 的二进制格式的*解序列化回 Python 对象。

```
>>> import pickle
>>> import pandas as pd# assume you already have file in pickle format
# and file name is my_data.pickle>>> data = pickle.load(open('my_data.pickle', 'rb'))# now data is come back
# variable name may be different to previous step
```

# 摘要

pickle.dump( object，file-stream) #将对象保存到文件中

object = pickle . load(file-stream)#从文件中读取并返回对象

*更多内容尽在*[*blog . devgenius . io*](http://blog.devgenius.io)*。*