# 2021 年:年度 Django 包 100 强榜单

> 原文：<https://blog.devgenius.io/2021-top-100-django-packages-list-during-the-year-92fef0ba79c9?source=collection_archive---------4----------------------->

![](img/b54e9b877efb05ac08bb5f30a869c5c8.png)

照片由 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/2021?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

一年前，我们调查了 Django 世界 2020 年最受欢迎的包。今天是时候和 2021 年对比了！

因此，让我们对 [google cloud](https://bigquery.cloud.google.com/dataset/the-psf:pypi) 数据集运行同样的查询，看看有什么变化

```
**SELECT** file.project, COUNT(*) as total_downloads
**FROM** `the-psf.pypi.file_downloads`
**WHERE** DATE(timestamp) 
  **BETWEEN** DATE_SUB(CURRENT_DATE(), INTERVAL 360 DAY)
      **AND** CURRENT_DATE()
**AND** file.project like '%django%'
**GROUP** **BY** file.project
**ORDER** **BY** total_downloads DESC
**LIMIT** 100
```

我们的基线是姜戈。2021 年获得超过 7800 万次下载，几乎比 2020 年多 20%!这个下载数量包括所有的手动安装和 CI 工作，所以很难说这个行业中有多少新的 Django 开发者。相比之下，`Flask`在同一年获得了超过 4 亿次下载，比 2021 年增长了 120%。`Numpy`接待超过 8.35 亿人次，增长 40%。最受欢迎的 python 包— `requests`已经被下载了超过 13 亿次，这个数字也比去年增长了 40%。

# 前 100 名中的新软件包

*   [**django-jsonview**](https://github.com/jsocol/django-jsonview)—REST API 的快捷方式
*   [**django-stubs-ext**](https://pypi.org/project/django-stubs-ext/)—django-stubs 的子包
*   [**FCM-django**](https://github.com/xtrinch/fcm-django)—Firebase 云消息
*   [**django-hosts**](https://github.com/jazzband/django-hosts)—路由特定子域的请求
*   [**姜戈-迁移-林特**r](https://github.com/3YOURMIND/django-migration-linter)-检测向后不兼容的迁移
*   [**django-colorfield**](https://github.com/fabiocaccamo/django-colorfield)**—积极为模特和管理人员开发色域**

# **休息**

**Django Rest 框架正以同样的速度增长，并且仍然在半数的 Django 安装中使用。为了让数据更加清晰，下面我将跳过下载量与 Django 使用量增长相同的软件包的 16-24%的增长率**

```
- djangorestframework            40 388 479
- django-cors-headers            22 719 141
- django-rest-framework           5 012 054    +82%
- djangorestframework-simplejwt   5 510 240    +68%
- djangorestframework-jwt         4 133 321    +13%
- django-rest-swagger             3 524 708    -8%
- django-rest-auth                2 238 789
- djangorestframework-csv         2 104 600
- djangorestframework-camel-case  1 644 871
- django-jsonview                 1 579 680    NEW
- djangorestframework-filters     1 138 336
```

**2021 年`django-tastypie`退出前 100 个包。无`djangorestframework-xml`也。另一个 REST 实现`django-ninja`仅下载了 106 588 次，远远没有登顶。**

# **GraphQL**

```
- graphene-django               2 447 735      +40%
```

**Graphene 是 Django 唯一流行的 GraphQL 实现。它的增长速度是 Rest 框架的两倍。GraphQL 会比 REST 更受欢迎吗？**

**Django 的另外两个 graphql 包下载量很低:`strawberry-graphql-django` 24187 和`ariadne-django` 7146**

**在 Django world 之外`Graphene`有惊人的 1100 万次下载，年增长率超过 87%，`Ariadne`只有 1 383 676 次，`strawberry-graphql` — 657 736 次**

# **存根**

**静态打字是我们每年比赛的赢家。仍然没有太多的 Django 用户进行静态类型检查，但以同样的增长率，这将在未来 3-4 年内发生**

```
- django-stubs                   3 450 782     +71%
- djangorestframework-stubs      1 531 140
- django-stubs-ext               1 290 978     NEW
```

# **开发实用程序**

**除了移民林挺的新助手，没有一个球员的表现超过 Django 基线。**

```
- django-extensions       20 561 833
- django-debug-toolbar    13 094 224
- pylint-django            6 609 279      
- django-silk              2 514 221
- django-waffle            2 239 980
- django-compat            1 167 273
- django-migration-linter  1 107 595    NEW
```

# **ORM 扩展和表单/序列化程序字段**

**相当稳定的结果。似乎 Django 的内部工具变得更加流行，所以`django-filters`、`import-export`和`bulk-update`包获得了更多的下载**

```
- django-filter             21 983 311     +31%
- django-model-utils         7 177 256
- django-timezone-field      6 539 299
- django-import-export       5 466 534     +34%
- django-phonenumber-field   4 574 386
- django-mptt                3 689 898     +4.5%
- django-countries           4 295 559     
- django-simple-history      3 553 915
- django-taggit              3 301 470     +32%
- django-mysql               2 223 315     -8.5%
- django-polymorphic         2 656 729     +9.5%
- django-localflavor         2 636 552     +10%        
- django-reversion           2 535 925
- django-picklefield         2 205 692     +10%
- django-treebeard           2 371 618     +21%  
- django-dirtyfields         2 383 978     +36%         
- django-jsonfield           1 382 853     -0.5%
- django-bitfield            
- django-bulk-update         1 689 512     +32%   
- django-modeltranslation    1 289 156   
- django-modelcluster        1 131 366
- django-colorfield          1 081 742     NEW
```

# **测试**

**内置的 Django 测试支持随着每个版本变得越来越好，但是`pytest`的使用仍然增长得更快**

```
- pytest-django           15 669 424     +29%
- django-nose              3 668 826     +8.6%
- django-coverage-plugin   1 618 325     +32%
```

# **形式**

**今年`django-bootstrap3`前 100 个包，而且似乎所有大部分用户都迁移到了`django-bootstrap4`**

**Django 本身得到了更好的模板表单渲染支持，`crispy`包已经陈旧。或许在 2022 年，我们会看到它的使用减少。**

```
- django-crispy-forms           5 702 525   +11%
- django-widget-tweaks          2 510 273   -3.6%      
- django-formtools              2 349 509   +0.5%
- django-ckeditor               2 185 180   +27%         
- django-autocomplete-light     1 520 720   +6.7   
- django-bootstrap4             1 570 073   +40%         
- django-tinymce                1 087 195   +13%
```

# **隐藏物**

**我们看到 Django 社区对 Redis 使用的稳定兴趣，Django 4.0 获得了开箱即用的 Redis 缓存支持。**

```
- django-redis              12 911 680   +37%
- django-redis-cache         2 244 945   +7%
- django-cacheops            1 317 225   +16%
```

# **设置**

**对于`dj-database-url`几乎没有增长，而对于`django-environ`有很大的提升，这一类别的用户比例年复一年保持稳定——占 Django 总下载的 32%。其他人对平淡的`settings.py`和`os.environ`满意吗？**

```
- django-environ        8 622 541      +54%
- dj-database-url 7 748 144      +2%
- django-appconf        7 515 049      +10%    
- django-constance      1 243 319      +7%
```

# **后台作业**

**下载量超过 5000 万次(+40%)，仍然是 Django 世界的王者。`django-crontab`走出榜首。**

```
- django-celery-beat      5 676 928    +28%
- django-celery-results   4 729 748    +42%
- django-celery           1 217 083    -18%
- django-rq               1 093 844    +12%
```

**Dramatiq 下载量增长超过 50%,但仍远未达到前 100——523 560。Huey 增长了 37%,每年有 453 811 次下载**

# **认证和授权**

**`social-auth-app-django`增长不够，失去了这一类别的第一名。`django-allauth`现在为 Django 添加社交账户认证是最流行的方式**

```
- django-allauth           4 998 753    +40%
- social-auth-app-django   4 716 219    +11.52
- django-oauth-toolkit     3 036 325    +6.5%
- django-otp               2 680 685    +38%
- django-guardian          2 376 722    +32%       
- django-auth-ldap         2 292 198    +91%
```

# **快捷方式和助手**

**对快捷键和语法糖没有太大的需求。`django-annoying` is 失去了近 10%的受欢迎度，不再名列榜首。`django-braces`也在下降**

```
- django-ipware             4 795 396    +16%
- django-braces             2 145 071    -4.7%
- django-user-agents        1 823 622
```

# **强化工具**

**另一个警告类别。Django 需要和 JS 工具集成吗？JavaScript 构建管道由自己的 JS 构建器来管理不是更好吗？**

```
- django-js-asset        4 354 255     +10%
- django-webpack-loader  3 158 250     -9%
- django-compressor      3 024 645     +2%
```

# **安全性**

**这里没有新的玩家，没有新的成就。看起来姜戈已经足够安全了。**

```
- django-axes           2 125 397      +2%
- django-ratelimit      1 552 483      +15%
- django-csp            1 780 528      +32%
```

# **邮件**

**大推进`django-anymail`移动到第一位！亚马逊不再是默认的托管地，其他云提供商已经得到了更多的关注。**

```
- django-anymail        2 766 768     +49%
- django-ses            2 353 989     +18%
```

# **监视**

**`sentry-sdk`下载量超过 9000 万次，增长 114%。肯定有一些用户是和 Django 一起来的。2021 年，针对 Django 的监测包也受到了很多关注**

```
- django-health-check       2 961 658    +53%
- django-prometheus         1 884 262    +8%
- django-log-request-id     1 487 990    +43%
```

# **管理**

**没有一个包像 Grapelli 一样成功地定制了 Django Admin，甚至今年它的受欢迎程度也下降了。但是日益增长的兴趣表明标准的管理界面仍然很受欢迎。**

**仅获得+3%的下载量，失去了榜首的位置。**

```
- django-grappelli           1 327 827    -12%
- django-admin-rangefilter   2 230 739    +61%
- django-object-actions      1 438 011    +41%
```

# **搜索**

**2021 年初，我们的下载量几乎持平，如今简单的`django-elasticsearch-dsl`比衰落的`django-haystack`增长得快得多**

```
- django-elasticsearch-dsl  1 288 081     +38%
- django-haystack           1 147 975     -8.5%
```

# **其他的**

**对于`channels`的使用没有太大的提升，它和 Django 一样成长。随着其他软件包的发展，`django-fsm`的使用越来越多，这有助于使用 Django 作为内部业务流程自动化的工具**

```
- django-storages          15 990 744    
- channels                  3 842 246
- django-fsm                1 934 169    +38%   
- django-tables2            1 525 420    +6%
- fcm-django                1 278 495    NEW
- wagtail                   1 216 937
- django-heroku             1 196 737    +0.3%
- django-hosts              1 150 593    NEW
- django-classy-tags        1 068 910
```

# **结论**

**2021 年，Django 生态系统显示出稳定的增长率。亲爱的 Django devs，这就是我们如何度过 2021 年的。你知道明年什么会登顶吗？我们漏了什么包裹？最近有没有新的工具问世？发表评论，让他们在文章更新中提出来。**

**编码快乐，圣诞快乐，新年快乐！**