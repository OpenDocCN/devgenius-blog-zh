# 更改 rest_framework 的请求参数验证失败响应

> 原文：<https://blog.devgenius.io/change-rest-frameworks-request-parameter-validation-failure-response-e1c7030945ca?source=collection_archive---------5----------------------->

# Django 和 rest_framework

![](img/2ce9e5f2287cf4e9f4a07ec81af84c6e.png)

[Faisal](https://unsplash.com/@faisaldada?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Django 是 web 应用程序开发的流行框架之一。

rest_framework 包允许开发人员更容易、更快速地构建 REST API 服务。因为它为我们提供了各种内置特性，比如序列化器、过滤器、验证等等。

# django-过滤包

Django rest-framework 的一个流行包是 django-filter。我们可以用 django-filter 包轻松开发搜索特性。

假设我们需要从数据库中的`note`表中进行搜索。可搜索的字段有`title`、`rank`和`content`。模型定义如下所示:

```
class Note(models.Model):
    title = models.CharField(max_length=20, blank=False, null=False)
    rank = models.IntegerField(blank=False, null=False, default=10)
    content = models.CharField(max_length=100, blank=False, null=False)
    created_datetime = models.DateTimeField(auto_now_add=True)
    modified_datetime = models.DateTimeField(auto_now=True)
```

在没有任何软件包帮助的情况下，我们需要实现以下部分来达到目的。

1.  从请求中检索`title`、`rank`和`created_datetime`参数值。类似`request.query_params.get("title")`的东西
2.  验证 get 请求参数。
3.  使用过滤器参数从数据库中提取记录。这可以通过使用模型对象查询或直接 SQL 查询构建来实现。

```
# search by title could be like
notes: [Notes] = Note.objects.filter(title__in=title)
```

## 使用 django 过滤器

另一方面，如果我们使用 django-filter 包，这将变得非常容易实现。首先，我们需要像这样定义过滤器类:

```
class NoteFilters(rest_framework.FilterSet):
    title = rest_framework.CharFilter(
        field_name="title",
        lookup_expr="contains"
    )

    rank = rest_framework.NumberFilter(
        field_name="rank",
        lookup_expr="gte"
    )

    date_from = rest_framework.IsoDateTimeFilter(
        field_name="created_datetime",
        input_formats=DATE_INPUT_FORMATS,
        lookup_expr="gte",
    )

    class Meta:
        model = Note
        fields = ["title", "modified_datetime"]
```

这些表达式在 filter 类中相当简单。例如，返回标题包含参数`title`值的注释的第一个表达式。

然后，您需要做的就是创建如下所示的视图:

```
class NoteList(ListCreateAPIView):
    serializer_class = NoteSerializer
    queryset = Note.objects.all()
    filter_backends = (rest_framework.DjangoFilterBackend,)
    filterset_class = NoteFilters
```

这里我们可以将我们已经创建的`NoteFilters`定义为`filterset_class`。
现在可以进行测试了。

样本请求:
搜索任何在`title`中带有`3`且在`2022/03/13`之后创建的笔记。

```
GET http://localhost:8000/api/note/?date_from=2022/03/13 19:51:00&title=3
```

如果数据库中存在这样的注释，这将返回 JSON 结果。

完整的源代码在我的 GitHub 库[这里](https://github.com/huchka/djangorestframeworkexception)。
[https://github.com/huchka/djangorestframeworkexception](https://github.com/huchka/djangorestframeworkexception)

# 更改 django 的响应-过滤器验证错误

django-filter 还验证请求参数。在示例中，我们将`rank`定义为一个整数变量。让我们看看当我们将`rank`参数作为字符串发送时会发生什么。

```
GET http://localhost:8000/api/note/?rank=asdf
```

它返回核心代码中预定义的错误消息:

```
{
    "rank": [
        "Enter a number."
    ]
}
```

然而，一些项目/作品需要它们自己特定的错误结构。例如，我们希望参数验证错误如下:

```
[
    {
        "error_field_name": "rank",
        "error_validation_message": [
            "Enter a number."
        ]
    }
]
```

# 解决办法

错误结果来自属于 django rest 框架的 ValidationError 异常类。要更改 ValidationError 类的响应，我们需要覆盖这个类。为此，我创建了扩展`ValidationError`的`CustomValidationError`类。

```
class CustomValidationError(ValidationError):
    def __init__(self, detail=None, code=None):
        logger.error(f"{detail=}")
        self.detail = [{"error_field_name": k, "error_validation_message": v} for k, v in detail.items()]
```

对于错误异常，返回这个`self.detail`字段。我们可以把它变成我们需要的任何东西。

之后，当`ValidationError`异常发生时，我们需要引发我们的`CustomVvalidationError`异常类。

```
class CustomListCreateAPIView(ListCreateAPIView):
    def get(self, request, *args, **kwargs):
        try:
            return super().get(request)
        except ValidationError as e:
            raise CustomValidationError(e.detail, e.status_code)
```

# 摘要

我在这里展示了一种可能的方法来为搜索特性更改 REST API 的参数验证。通常，一个项目需要以相同的格式改变所有的 API 错误响应。所以，我们需要改变所有的 API 视图类。但是，这是一种非常低效的方式。相反，我建议创建一个像上面这样的自定义视图类，然后我们可以使用该类作为视图的父类。