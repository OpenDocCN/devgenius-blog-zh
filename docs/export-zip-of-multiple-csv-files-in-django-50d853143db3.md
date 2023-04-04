# 在 Django 导出多个 CSV 文件的 zip 文件

> 原文：<https://blog.devgenius.io/export-zip-of-multiple-csv-files-in-django-50d853143db3?source=collection_archive---------1----------------------->

![](img/baabdade5f2f9e4cf44739798a74c01c.png)

导出文件是一种经常发生的功能，用户可以在其中取出他们的数据。

今天，我将尝试将数据导出到多个 CSV 文件中的 zip 文件中。

(来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3037639) 的 [Tayeb MEZAHDIA](https://pixabay.com/users/tayebmezahdia-4194100/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3037639) 拍摄的图像)

# 应用模型

假设我们有最简单的图书馆系统，一本书可以属于许多图书馆。

像这样的模型:

```
**class** Library(models.Model):
    name = models.TextField()

**class** Book(models.Model):
    title = models.TextField()
    libraries = models.ManyToManyField(
        null=True,
        blank=True,
        to='Library',
        related_name='books'
    )
```

我们在这里尝试做的是导出一个 zip 文件，其中包括许多 CSV 文件，其中一个库的每个演示文稿都显示该库中的图书列表。

# 制作视图以获取导出 zip 文件

像往常一样，为了创建一个用于下载的 API，我们编写了一个只允许 GET 方法的视图。

```
**import** csv
**import** io
**import** zipfile
**from** wsgiref.util **import** FileWrapper
**from** django.http **import** StreamingHttpResponse
**from** rest_framework.views **import** APIView

**class** ExportZip(APIView):
    **def** get(self):
        csv_datas = self.build_multiple_csv_files()

        temp_file = io.BytesIO()
        **with** zipfile.ZipFile(
             temp_file, "w", zipfile.ZIP_DEFLATED
        ) **as** temp_file_opened:
            # add csv files each library
            **for** data **in** csv_datas:
                data["csv_file"].seek(0)
                temp_file_opened.writestr(
                    f"library_{data['library_name']}.csv",
                    data["csv_file"].getvalue()
                )

        temp_file.seek(0)

        # put them to streaming content response 
        # within zip content_type
        response = StreamingHttpResponse(
            FileWrapper(temp_file),
            content_type="application/zip",
        )

        response['Content-Disposition'] = 'attachment;filename=Libraries.zip'
        **return** response

    **def** build_multiple_csv_files(self):
        csv_files = []
        **return** csv_files
```

在上图中，我们使用了 [*zipfile 模块*](https://docs.python.org/3/library/zipfile.html) ，这是一个用于数据压缩和归档的 Python 标准库。

[zip file。ZipFile](https://docs.python.org/3/library/zipfile.html#zipfile.ZipFile)允许我们打开一个 ZIPFIle 文件进行写入，其中 ***文件*** 可以是一个 [*文件样的对象*](https://docs.python.org/3/library/io.html#module-io) ，具体在这种情况下是 **temp_file** 是一个二进制 I/O as**temp _ FIle _ open**。

```
**class** zipfile.ZipFile(
    file, mode='r', compression=ZIP_STORED, 
    allowZip64=True, compresslevel=None, 
    *, strict_timestamps=True
)
```

我们在上下文管理器中通过“ **with** ”语句打开它，然后可以确保我们的 zip 在 with 语句的套件完成后关闭——即使发生了异常。

```
temp_file = io.BytesIO()
**with** zipfile.ZipFile(
    temp_file, "w", zipfile.ZIP_DEFLATED
) **as** temp_file_opened:
    # write to zip file
```

在上下文管理器中，我们通过方法[writer str](https://docs.python.org/3/library/zipfile.html#zipfile.ZipFile.writestr)将 CSV 内容文件写入 zip**temp _ file _ open、**。

```
ZipFile.writestr(
    zinfo_or_arcname, data, 
    compress_type=None, compresslevel=None
)
```

在这里我们放入两个需要的参数是 *zinfo_or_arcname* 和*数据*

```
temp_file_opened.writestr(
    f"File_library_{file['lib']}.csv",
    file["csv_file"].getvalue()
)
```

写完多个 CSV 文件后，我们通过 [*查找*](https://docs.python.org/3/library/io.html#io.IOBase.seek) 来查找 zip 文件，然后使用 [*文件包装器*](http://filewrapper/) 将类似文件的对象转换为迭代器，然后将它们返回到 *StreamingHttpResponse* 。

此时，我们可以下载一个空的“Libraries.zip”文件。

# 构建 CSV 文件列表

如你所见，我们准备了一个名为“***build _ multiple _ CSV _ files***”的方法，它返回上面的空列表。在这一步，我们将把代码放到这个函数中来构建一个 CSV 文件列表。

```
**class** ExportLibraries(APIView):
    header_data = {
        "name": "Name",
        "library": "Library Name"
    }

    **def** get(self):
        ...
        **return** response

    **def** build_multiple_csv_files(self, libraries, books):
        csv_files = []

        **for** library **in** libraries.iterator():
            mem_file = io.StringIO()
            writer = csv.DictWriter(
                mem_file, fieldnames=self.header_data.keys()
            )
            writer.writerow(self.header_data)

            books_in_library = books.filter(libraries__in=[library.id])
            **for** book **in** books_in_library:
                book_row = self.build_book_row(book, library)
                writer.writerow(book_row)

            mem_file.seek(0)

            csv_files.append({
                "library_name": library.name,
                "csv_file": mem_file
            })

        **return** csv_files

    **def** build_book_row(self, book, library):
        row = self.header_data.copy()

        row["name"] = book.name
        row["library"] = library.name

        **return** row
```

看看上面的代码，我们访问所有的库，然后通过 init 从 *csv 编写器构建每个 CSV 文件。dict writer*er()使用 header_data 的键， *header_data* 可能是这样的:

```
header_data = {
    "name": "Book Name",
    "library": "Library Name"
}
```

之后，将标题添加到 writer 中，然后使用一个循环通过 call 方法将所有书籍逐行添加到 writer 中。write_row()

完成写入后，在库的名称中添加一个对象，以帮助设置 CSV 文件名和 CSV 文件内容。

您可以通过将该视图添加到 URLs 文件来进行导出。

```
urlpatterns = [
    path(
        'export_libraries/',
        ExportLibraries.as_view(),
        name="export_libraries"
    )
]
```

# 单元测试

下一个问题是，如何测试结果？

这一部分应该在处理逻辑之前完成，就像通过 TDD(测试驱动开发)一样，但是我想先展示一下逻辑是如何工作的，这样可能会更容易理解哪些应该被测试。

我计划为此进行两次单元测试:

–**一个 API:** 调用 API 应该导出一个 zip 文件

–**一个用于 CSV 文件和内容:**视图上调用*build _ multiple _ CSV _ files*应该会返回每个库数据的列表。在这里，还可以逐行检查每个 CSV 文件上的内容。

**注意:**请注意，我下面的 UTs 只是对它看起来像什么的一个提示，你应该根据你的特点来做。

## 单元测试:调用 API 导出 zip 文件

```
**def** test_export_libraries(self):
    response = self.client.get(reverse('export_libraries'))
    **assert** response.status_code **is** status.HTTP_200_OK
    **assert** response.get('Content-Disposition') == "Libraries.zip"
```

这个 UT 非常简单，我们只需要检查以确保 API 调用 200 和导出的文件是名称中的 zip 文件。

## 单元测试:调用视图函数获取 CSV 文件

```
**def** test_build_csvs_files(self):
    # assume we mock 2 libraries
    # library_1, library_2
    # queryset is books and libraries
    view = ExportRecipesCost()
    view.request = drf_request_for_context(self.user)
    csv_files = view.build_multiple_csv_files(
        libraries, books
    )
    # check number of csv files
    **assert** len(csv_files) == 2
    # first csv file
    **assert** csv_files[0]["library_name"] == library_1.name
    **assert** csv_files[0]["csv_file"]
    # go check csv content in first file here

    # second csv file
    **assert** csv_files[0]["library_name"] == library_2.name
    **assert** csv_files[0]["csv_file"]
    # go check csv content in first file here
```

我在代码上留下了一些注释，让我们知道有一些测试数据，我们计划做的是在模拟 DRF 请求中调用视图函数(由 util 函数 *drf_request_for_context 创建)。*

除了返回的 CSV 文件的数量之外，我们还可以根据文件头检查 CSV 内容文件。

# 提高性能

在我的解决方案中，我们使用了两个循环，一个用于图书馆，嵌套在其中的是每个图书馆的图书。更好的解决方案是准备所有的图书数据，不管它属于哪个图书馆，在图书馆的循环中使用这组数据。这可能会降低性能。

另一个可以改进的地方是出口流程。我们可以将逻辑导出作为后台任务(例如 celery 任务)运行，而不是将响应数据发送到 *StreamingHttpResponse* 以从浏览器下载。完成后，将 zip 文件上传到 s3，然后给用户一个下载的方法(URL 或连接到第三方)。这个流程可以让用户体验更好，并避免数据太多时出现超时错误。

让我们尝试使用提示，如果你有更好的解决方案，请与我分享。

总之，今天，我们来看看在 Django 应用程序中导出多个 CSV 文件中的 zip 文件的方法(我称之为使用 Django 查询)。

有一个用熊猫从 Django 应用程序导出数据的迷人方法，我希望将来有机会和你分享。

[本内容的原始版本](https://beautyoncode.com/export-zip-of-multiple-csv-files-in-django/)属于我的个人博客【beautyoncode.com。

感谢您的阅读。

BeautyOnCode。