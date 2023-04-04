# 单一责任原则

> 原文：<https://blog.devgenius.io/solid-single-responsibility-principle-61f2c4b3d982?source=collection_archive---------5----------------------->

![](img/5a2c0c16802c62820681c1842e1418c7.png)

> 一个类应该有且只有一个改变的理由

这个原理在汤姆·狄马克和梅里尔·佩奇-琼斯的著作中有所描述。他们称之为内聚，他们将其定义为模块的元素的**功能相关性。**

例如，考虑下面的矩形类。这个类有一个在屏幕上绘制矩形的方法和另一个计算矩形面积的方法。

```
public class Rectangle
{
  public double Area()
  {
    ...
  } public void Draw()
  {
    ...	
  }
}
```

现在假设两个不同的应用程序使用 rectangle 类。

一个应用程序是计算几何(使用 Rectangle 来执行一些几何操作，但从不在屏幕上绘制矩形)，另一个应用程序是纯图形的，其主要目的是在屏幕上绘制矩形。

这个设计违反了单责任原则，因为矩形类有**两个责任**。第一个职责是提供矩形几何的数学模型，第二个职责是在 GUI 上呈现矩形。

这种违反的后果将是:

*   我们必须在计算几何应用中包含 GUI。英寸 NET 中，GUI 组件必须用计算几何应用程序来构建和部署。
*   如果对 GraphicalApplication 的更改由于某种原因导致矩形发生变化，这种变化可能会迫使我们重新构建、重新测试和重新部署 ComputationalGeometryApplication。如果我们忘记这样做，应用程序可能会以不可预知的方式崩溃。

为了解决这个问题并遵守这个原则，我们应该将 Rectangle 的计算部分移到 GeometricRectangle 类中。现在，对矩形渲染方式的更改不会影响 ComputationalGeometry 应用程序。

```
public class GeometricRectangle
{
  public double Area()
  {
    ...
  }}public class Rectangle
{
  public void Draw()
  {
    ...	
  }}
```

# 为什么有必要将这两种责任分成不同的类？

原因是每个责任都是变化的轴心。当需求发生变化时，这种变化将通过类之间职责的变化来体现。如果一个类承担了不止一个责任，那么这个类就有不止一个理由去改变。

如果一个类有不止一个责任，那么这些责任就成了一对。改变一项职责可能会损害或抑制班级完成其他职责的能力。这种耦合导致脆弱的设计，当改变时会以意想不到的方式破裂。