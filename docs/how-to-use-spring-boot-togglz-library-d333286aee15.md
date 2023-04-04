# 如何使用 spring boot togglz 库

> 原文：<https://blog.devgenius.io/how-to-use-spring-boot-togglz-library-d333286aee15?source=collection_archive---------2----------------------->

![](img/0d3b139fd1bf6143dd99afd38654ec1e.png)

> **什么是 Togglz 库**

Togglz 是 Java 的[特性切换](http://martinfowler.com/bliki/FeatureToggle.html)模式的实现。在持续部署和交付的环境中，特性切换是一种非常常见的敏捷开发实践。基本思想是将切换与您正在处理的每个新功能相关联。这允许您在应用程序运行时启用或禁用这些功能，甚至是针对单个用户。

参考:【https://www.togglz.org/ 

> 使用弹簧靴的简单示例

1.  让我们从添加所需的库开始

```
implementation 'org.togglz:togglz-spring-boot-starter:3.0.0'
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```

我们将需要执行器库来检查和修改切换值

2.创建实现功能的枚举

![](img/fa790f4774009180e19729a833e9406e.png)

3.现在用 bean 创建一个配置类来获取我们的特性值

![](img/9218634aa65d8d94a5c3f1a1a55d046a.png)

4.现在，让我们在 application.properties 文件中添加我们的配置(让我们将标志的默认值设为 false)

![](img/0b6cd1861d0a265d8d0c854bbc7439bf.png)

5.是时候在我们的实现中使用我们的特性标志了，下面是示例代码

![](img/7f4242ccf9c0a845e6c275ffdf3351fa.png)

搞定了。！！

是的，我们已经完成了我们的实现，是时候测试我们的特性了

**卷曲以检查开关**的值

```
curl --location --request GET '[http://localhost:8080/actuator/togglz/'](http://localhost:9092/actuator/togglz/') \
--header 'Content-Type: application/json'
```

![](img/41f13fd692f1034a1ce935a021ebaec7.png)

注意:请记住，我们将特性的默认值设置为 false

现在让我们检查这个特性是否在我们的代码中工作

![](img/b045e9d93f0f1fa30f1d0bd07db4740f.png)

**卷曲来改变你的特征值**

```
curl --location --request POST '[http://localhost:8080/actuator/togglz/FLAG'](http://localhost:9092/actuator/togglz/FLAG') \
--header 'Content-Type: application/json' \
--data-raw '{
   "enabled": "true"
}'
```

![](img/70bbed2039048dbd9f525b0b4c0087dc.png)

既然我们已经更改了标志的值，让我们测试一下我们的逻辑

![](img/bad67f1e0ef7bb8b09074afbdb486314.png)

Yayyy！！按预期工作

在 spring boot 中实现特性 togglz 就是这么简单