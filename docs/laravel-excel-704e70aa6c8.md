# Laravel-Excel

> 原文：<https://blog.devgenius.io/laravel-excel-704e70aa6c8?source=collection_archive---------9----------------------->

![](img/a62868be99b02489b9bb51c9ac20c379.png)

Laravel-Excel 是一个令人兴奋的软件包，在我的开发生涯中，它给了我很大的帮助。我们已经介绍了我这些年来使用的一些功能，但是还有许多您可能需要的功能我们还没有介绍。我最喜欢的包括:

*   [事件](https://docs.laravel-excel.com/3.1/imports/extending.html)。运行导入后，您可能需要进行一些最后的清理。即使在排队的块导入之后也可以调度事件，这很方便。我个人使用了所有的事件，但是我发现我几乎总是使用`afterSheet`和`afterImport`事件。一个用例包括发送一封电子邮件，告知导入成功。
*   [导出格式](https://docs.laravel-excel.com/3.1/exports/export-formats.html)。Laravel-Excel 不要求您只导出到 XLSX 文件。您有许多格式可供选择，例如:CSV、XLSX、TSV、ODS、XLS、HTML、MPDF、DOMPDF 和 TCPDF。PDF 需要一个 [PHP 渲染库](https://phpspreadsheet.readthedocs.io/en/latest/topics/reading-and-writing-to-file/#pdf)。
*   [导出到多张纸](https://docs.laravel-excel.com/3.1/exports/multiple-sheets.html)。导出到单个工作表是一回事，但有时您需要多个工作表。Laravel Excel 也可以处理这个问题。
*   [格式化列](https://docs.laravel-excel.com/3.1/exports/column-formatting.html)。是否希望将列格式化为日期？你也可以用 Laravel Excel 来做。您可以格式化单个单元格或使用`afterSheet`事件来修改一列中的所有单元格。
*   [设置](https://docs.laravel-excel.com/3.1/exports/settings.html)。指定 Excel 文档的各种不同设置，如创建者、标题和描述。

下面是我们已经讨论过的教程的完整列表。导入和导出数据是经常需要的。我很高兴 Laravel-Excel 包的存在。

*   [P1:设置和基本导入](/laravel-excel-p1-setup-and-basic-import-1b3ddf9d9b8c)
*   [P2:导入多张纸基础知识](/laravel-excel-p2-importing-multiple-sheets-basics-2d20994770b)
*   [P3:导入多张纸并处理未知定义的纸](/laravel-excel-p3-importing-multiple-sheets-and-handling-unknown-defined-sheets-749dbc1ec089)
*   [P4:导入多张有条件加载](/laravel-excel-p4-importing-multiple-sheets-conditional-loading-d70db3ae1409)
*   [P5:导入带公式的表单](/laravel-excel-p5-importing-sheets-with-formulas-4016dcc68862)
*   [P6:批量进口](/laravel-excel-p6-batch-imports-261c80e46448)
*   [P7:组块读取](/laravel-excel-p7-chunk-reading-631de427f2d)
*   [P8:排队导入](/laravel-excel-p8-queued-import-7d8e6aac2e8f)
*   P9:出口
*   [P10:从视图导出](/laravel-excel-p10-export-from-view-7fca03cc0d4)

所有的代码都可以在我的 GitHub 账户上找到。

[](https://github.com/dinocajic/package-laravel-excel) [## GitHub-dinocajic/package-Laravel-Excel:显示了 Laravel Excel 的示例功能

github.com](https://github.com/dinocajic/package-laravel-excel) ![](img/2efc6fb7afa2f5bfe83e7c8e9ff5cd48.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体刊物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。