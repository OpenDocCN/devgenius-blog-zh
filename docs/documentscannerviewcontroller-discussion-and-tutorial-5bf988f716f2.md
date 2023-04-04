# DocumentScannerViewController:讨论和教程

> 原文：<https://blog.devgenius.io/documentscannerviewcontroller-discussion-and-tutorial-5bf988f716f2?source=collection_archive---------3----------------------->

在 WWDC 22 上，苹果的视觉团队推出了一项新功能，使扫描实时数据变得轻而易举。该特征采用`DocumentScannerViewController`的形式。在本教程/演练中，我们将讨论这个新控制器试图解决的问题、它的特性和它的局限性。本文引用了苹果在 2022 年 WWDC 大会上[用 VisionKit](https://developer.apple.com/wwdc22/10025) 捕捉机器可读代码和文本的会议代码。

# 什么是数据扫描？

简而言之，数据扫描是摄像头等传感器从现实世界读取数据的一种方式。这些数据可能是文本、条形码、QR 码等形式。iOS 15 中的实时文本功能就是一个数据扫描仪的例子，它可以从图像中检测文本和其他有用的信息。

# 问题是

在 iOS 16 之前，扫描摄像头馈送中的数据需要多步方法，包括编写与多个框架交互的代码。我们来看一个例子。

# AVFoundation

`AVFoundation`框架让我们与设备上可用的视听传感器进行交互。在使用这个框架的典型方法中，我们将首先使用一个`AVCaptureDevice`来创建一个`AVCaptureDeviceInput`。然后，我们将把这个输入传递给一个`AVCaptureSession`。该会话将连接到一个预览层，以使用`AVCaptureVideoPreviewLayer`显示实时摄像机画面。与此同时，这次会议也将为我们提供一个`AVCaptureMetadataOutput`，导致一个`AVMetadataObject`。这将为我们提供一种捕获机器可读代码的方法，如条形码、QR 码等。

# 将 AVFoundation 与 Vision 结合使用

`AVFoundation`方法只能用于检测默认形式的机器可读代码。如果您还想检测来自摄像头的文本，您可以将`AVCaptureSession`的输出连接到`AVCaptureVideoDataOutput`。这给了我们一个`CMSampleBufferRef`流，它可以被输入到`Vision`框架的`VNImageRequestHandler`来执行观察。这些观察采取视觉观察对象的形式。

# 如何缓解这个多步骤的过程？

在 iOS 16 中，苹果引入了一种新的数据扫描方法，将上述所有步骤封装到一个视图控制器中:引入`DataScannerViewController`作为`VisionKit`框架的一部分。`DataScannerViewController`是`UIViewController`的子类。它结合了`AVFoundation`和`Vision`的特性，专门用于数据扫描。

# 面向用户的功能

*   实时摄像机预览:这个视图控制器显示一个实时摄像机画面预览，就像你手动配置了一个`AVCaptureVideoLayer`一样。
*   指导:在顶部提供小提示，如在数据扫描期间“减速”。
*   项目高亮显示:为检测到的数据和边界框提供项目高亮显示。
*   点击聚焦:用户可以在这个控制器中点击聚焦到一个不同的数据对象上，而不需要任何额外的代码。
*   缩小至放大:用户可以缩小至放大画面，以便更近距离地查看要扫描的项目。

# 开发者功能

*   已识别项目的坐标在视图坐标中。这样就不需要从 AVKit 的坐标空间转换到 Vision 的坐标空间，然后再转换回视图的坐标空间。
*   可以设置感兴趣区域，使得控制器仅扫描所提供区域中的项目。该区域也由视图坐标标记。
*   可以指定文本内容类型和机器可读代码符号，以便扫描仪只扫描您感兴趣的项目。

# 入门指南

# 隐私使用描述

因为我们正在访问摄像机，所以我们必须在 Xcode 项目中为`Privacy-Camera Usage Description`键提供一个`plist`条目。该键的值将是当系统提示请求允许使用摄像机时显示在提示中的字符串。重要的是尽可能的描述性，同时尽可能的简洁，让你的用户知道为什么他们设备的摄像头会被使用。

# 密码

新的`DocumentScannerViewController`是`VisionKit`框架的一部分。从导入开始。

```
import VisionKit
```

# 注意:并非所有设备都支持数据扫描。

除了要求 iOS 16，苹果还将数据扫描限制在 2018 年或以后推出的内置苹果神经引擎的设备上。要检查设备是否与`VisionKit`的新数据扫描控制器兼容，使用`DataScannerViewController`上的`isSupported`类属性。

```
DataScannerViewController.isSupported
```

我们还需要检查可用性。

```
DataScannerViewController.isAvailable
```

如果在系统中设置了某些限制，此布尔值可能为假。例如，用户可能已经拒绝了相机许可，或者他们可能已经在屏幕时间内通过内容和隐私限制设施关闭了所有应用程序的相机使用。

# 返回代码

首先定义一个名为`recognizedDataTypes`的变量。这个变量是一组`DataScannerViewController.RecognizedDataType`项。可识别的数据类型包括两大类:文本和机器可读代码。

# 文本类型

这些类型在`DataScannerViewController.TextContentType`中定义。它们由以下内容类型组成:

*   统一资源定位器
*   日期时间持续时间
*   电子邮件地址
*   航班号
*   完整街道地址
*   shipmentTrackingNumber
*   电话号码

```
.text(textContentType: .URL)
```

除了配置可以识别的文本类型之外，我们还可以传入我们希望识别这些文本的语言列表。如果你知道什么语言，你可以把它们列出来。如果没有显式地传入任何语言，扫描程序将使用用户的首选语言列表。要指定语言:

```
.text(languages: ["en"])
```

可用语言的列表可以通过类的另一个属性来访问:`supportedTextRecognitionLanguages`

```
DataScannerViewController.supportedTextRecognitionLanguages
```

使用此列表可访问可用于数据扫描仪的最新可用语言列表。

# 机器可读代码类型

这些类型被定义为`VNBarcodeSymbology`的一部分。这些是他们包含的一些项目。完整列表可在[这里](https://developer.apple.com/documentation/vision/vnbarcodesymbology?changes=late_1_8)找到。

*   季度
*   微 QR
*   代码 128
*   ean13

要检测这些，只需将另一个可识别的数据类型传递给变量。

```
.barcode(symbologies: [.qr, .ean13])
```

您定义已识别数据类型的代码现在应该是这样的。

```
let recognizedDataTypes: Set<DataScannerViewController.RecognizedDataType> = [
	.barcode(symbologies: [.qr, .ean13]),
	.text(languages: ["en"])
]
```

我们现在可以创建一个`DataScannerViewController`的实例，并传递这组我们希望扫描仪识别的数据项。

```
let controller = DataScannerViewController(recognizedDataTypes: recognizedDataTypes)
```

现在，您可以像展示任何其他视图控制器一样展示它。演示完成后，呼叫`controller.startScanning()`。

```
present(controller, animated: true) {
	try? controller.startScanning()
}
```

# 可用的初始化参数

*   `recognizedDataTypes`:你想要识别的数据类型。
*   `qualityLevel`:这可以有三种类型:平衡、快速或精确。如果您希望以牺牲准确性为代价快速执行检测，请选择快速。如果您想检测微小的细节，如微型二维码，请选择“精确”。对于大多数任务，平衡级别(也是默认级别)就足够了。
*   `recognizesMultipleItems`:决定扫描仪是聚焦于一个项目还是在一帧中检测几个项目的标志。
*   `isHighFrameRateTrackingEnabled`:当相机的预览框不断移动，您的自定义高光需要精确跟踪时，请启用此选项。
*   `isPinchToZoomEnabled`:允许用户在预览窗口上执行捏合手势，以放大并聚焦于特定对象。
*   `isGuidanceEnabled`:允许在视图顶部显示标签，以帮助指导用户执行成功的数据扫描。
*   `isHighlightingEnabled`:如果您正在绘制自定义高光，您可以选择禁用此选项。

# 代表

为了利用被扫描的数据并提供定制的高亮显示，我们需要使用委托方法。首先向`DataScannerViewController`的实例提供一个委托。

```
controller.delegate = self
```

使您的演示者类别符合`DataScannerViewControllerDelegate`。

# 处理点击交互

我们要讨论的第一种方法是

```
dataScanner(_:didTapOn:)
```

正如函数签名所暗示的，当用户点击一个包含可扫描数据的项目时，就会调用这个方法。当用户点击聚焦于具有数据项的对象时会发生这种情况，该数据项属于在该控制器的初始化期间传递的已识别数据项的集合。来自苹果公司 WWDC 22 日的会议:

```
func dataScanner(_ dataScanner: DataScannerViewController, didTapOn item: RecognizedItem) {
    switch item {
    case .text(let text):
        print("text: \(text.transcript)")
    case .barcode(let barcode):
        print("barcode: \(barcode.payloadStringValue ?? "unknown")")
    default:
        print("unexpected item")
    }
}
```

# 使用自定义高光

**添加自定义高亮显示**
如前所述，您可以提供覆盖`DataScannerViewController`默认高亮显示视图的自定义高亮显示。传回给我们的每个已识别的项目都有一个 UUID，在该特定数据项的生命周期内，它仍然与该项目相关联。这意味着您可以跟踪一个特定的数据项，从它进入帧的时间到它最后被控制器看到的时间。使用这个 UUID，我们可以为特定的项目添加亮点。使用字典来记录这些项目。

```
*// Dictionary to store our custom highlights keyed by their associated item ID.*
var itemHighlightViews: [RecognizedItem.ID: HighlightView] = [:]
```

现在，您可以为扫描仪唯一识别的每个项目添加一个新的`UIView`。为此，请使用 didAdd 方法。

```
*// For each new item, create a new highlight view and add it to the view hierarchy.*
func dataScanner(_ dataScanner: DataScannerViewController, didAdd addItems: [RecognizedItem], allItems: [RecognizedItem]) {
    for item in addedItems {
        let newView = newHighlightView(forItem: item)
        itemHighlightViews[item.id] = newView
        dataScanner.overlayContainerView.addSubview(newView)
    }
}
```

**更新自定义高亮显示**
正如我之前提到的，已识别的物品会被赋予唯一的标识符，可用于在其生命周期内跟踪该物品。使用另一个委托方法，我们可以在项目在预览窗口中移动时实时更新自定义高亮视图的框架。

```
*// Animate highlight views to their new bounds*
func dataScanner(_ dataScanner: DataScannerViewController, didUpdate updatedItems: [RecognizedItem], allItems: [RecognizedItem]) {
    for item in updatedItems {
        if let view = itemHighlightViews[item.id] {
            animate(view: view, toNewBounds: item.bounds)
        }
    }
}
```

**移除自定义高亮显示**
最后，一旦识别出的项目不再在视图中，我们需要从视图层次结构中移除自定义高亮显示视图。为此，请访问此委托方法。

```
*// Remove highlights when their associated items are removed.*
func dataScanner(_ dataScanner: DataScannerViewController, didRemove removedItems: [RecognizedItem], allItems: [RecognizedItem]) {
    for item in removedItems {
        if let view = itemHighlightViews[item.id] {
            itemHighlightViews.removeValue(forKey: item.id)
            view.removeFromSuperview()
        }
    }
}
```

# 变焦

为了在控制器的`zoomFactor`属性改变时得到通知，实现这个委托方法。

```
func dataScannerDidZoom(_ dataScanner: DataScannerViewController) {
	print(controller.zoomFactor)
}
```

# 错误处理

如果由于某种原因，数据扫描器在视图控制器的生命周期中变得不可用，这个委托方法将被调用。

```
func dataScanner(
    _ dataScanner: DataScannerViewController,
    becameUnavailableWithError error: DataScannerViewController.ScanningUnavailable
) {
	print(error.localizedDescription)
}
```

# 已识别项目的连续异步流。

```
func updateViaAsyncStream() async {
    guard let scanner = dataScannerViewController else { return } let stream = scanner.recognizedItems
    for await newItems: [RecognizedItem] in stream {
        let diff = newItems.difference(from: currentItems) { a, b in
            return a.id == b.id
        } if !diff.isEmpty {
            currentItems = newItems
            sendDidChangeNotification()
        }
    }
}
```

# 附加功能

**拍照**T11`DataScannerViewController`可以用来拍摄当前预览窗口的照片。要拍摄照片并将其保存到用户的照片库中，请使用以下代码:

```
if let image = try? await dataScanner.capturePhoto() {
    UIImageWriteToSavedPhotosAlbum(image, nil, nil, nil)
}
```

**缩放**
您可以编程设置控制器及其预览窗口的缩放级别。要设置`zoomFactor`:

```
controller.zoomFactor = 0.4
```

您可以使用`minZoomFactor`和`maxZoomFactor`属性来决定这个值。这使您可以分别访问相机的最小和最大可用变焦值。

**扫描过程相关的**
停止扫描调用`stopScanning()`方法。使用控制器上的`isScanning`属性检查扫描仪是否处于活动状态。

# 结论

`DocumentScannerViewController`是一种功能强大的新方法，用于扫描实时摄像机视频馈送中的数据。这个过程以前需要大量代码，并且需要手动集成`VisionKit`和`AVKit`。尽管局限性非常明显，如 iOS 16 最低版本要求、与没有苹果神经引擎的旧设备不兼容等，但我仍然相信这个 API 在未来几年将非常有用。

如果你喜欢这篇文章，可以考虑请我喝杯咖啡，奖励我的努力。你也可以选择[在 Github 上赞助我](https://github.com/sponsors/SwapnanilDhol)。通过 Github 赞助商，你可以请求各种服务，比如结对编程、代码审查、咨询等等！