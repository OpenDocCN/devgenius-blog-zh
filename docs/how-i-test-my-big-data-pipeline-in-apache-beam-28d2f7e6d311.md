# 如何在 Apache Beam 中测试我的大数据管道？

> 原文：<https://blog.devgenius.io/how-i-test-my-big-data-pipeline-in-apache-beam-28d2f7e6d311?source=collection_archive---------7----------------------->

## 大数据工程之旅

## 如何在 Apache Beam 中测试我的大数据管道？

![](img/f927304cd4cf8a0073a8845e7b289ace.png)

[un splash 上 Ferenc Almasi 的照片](https://unsplash.com/@flowforfrank)

# 概观

在软件开发中，单元测试是一个重要的步骤，它使您的项目提高质量，并确保您的项目在部署产品之前没有任何错误，因此我们将其视为编程阶段的一部分。如果我们在编码中执行一个功能，我们还会通过对该功能使用单元测试来创建测试用例。在本文中，我将分享我如何在 Apache Beam 中为我的大数据工作编写单元测试。

## 针对单个转换的#1 测试

要测试一个单独的转换，您将一步一步地进行:

*   创建测试管道
*   创建一些静态的测试输入数据
*   创建转换
*   使用 PAssert 验证输出 PCollection 包含您期望的元素

**创建测试管道**

TestPipeline 是一个专门用于测试转换的 Apache Beam Java SDK 类。

在创建管道对象时，我们使用 TestPipeline 来代替管道。与 Pipeline.create 不同，TestPipeline.create 在内部处理 PipelineOptions 的设置。

参见测试管道示例:

**创建测试输入数据**

请参见下面的代码:

**创建变换**

我们通过从一个标准的内存集合类(比如 List)创建一个 PCollection 来创建输入转换。见下文:

之后，我们将转换应用于输入 PCollection，并保存结果输出 PCollection

**路人**

PAssert 是 Apache Beam Java SDK 中包含的一个类，它是对 PCollection 内容的断言。我们使用 PAssert 来验证包含一组特定预期元素的 PCollection。

以下 Java 代码示例使用 PAssert 类来检查正确的输出形式:

以下代码显示了测试转换的完整测试:

## #2 复合转换测试

我们使用测试复合转换，以防我们代码中的转换可以具有嵌套结构(就嵌套类而言)，其中一个复杂的转换执行多个较简单的转换(例如多个 ParDo、Combine 或者甚至其他复合转换)。这些转换称为复合转换。在单个复合转换中嵌套多个转换可以使代码更加模块化，更容易理解。

以下代码显示了复合转换的完整测试:

正如您在上面看到的，代码适用于“ **SumNumbers** ”转换子类，它由一个名为“ **SumInts** ”子类的嵌套转换组成。在嵌套 transform " **SumInts** "子类**，**中，我们覆盖了" **apply** 方法来实现实际的处理逻辑。

就通过上面的例子创建复合转换而言，我们可以认为" **Combine** "本身就是一个复合转换；" **SumNumbers** 和" **SumInts** "也是嵌套的复合转换。

## #3 整个管道的测试

测试整个管道被称为端到端测试。在上面的例子中，我们从静态输入数据创建了两个 p collection，然后使用 PAssert 来验证管道生成的两个 p collection 的内容是否与静态输出数据中的预期值相匹配。

下面的代码显示了一个测试整个转换的完整测试:

# 摘要

本文向您介绍了如何在 Apache Beam 中运行单元测试的方法，并帮助您在使用 Apache Beam 时快速实现这一实践。

# 参考

*   [测试您的管道(apache.org)](https://beam.apache.org/documentation/pipelines/test-your-pipeline/#testing-transforms)
*   [使用数据流构建生产就绪数据管道:开发和测试数据管道|云架构中心|谷歌云](https://cloud.google.com/architecture/building-production-ready-data-pipelines-using-dataflow-developing-and-testing#unit_tests)