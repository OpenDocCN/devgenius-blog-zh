# PHP 7.2 会是 WP 6.2 的最低要求版本吗？

> 原文：<https://blog.devgenius.io/will-php-7-2-be-the-minimum-required-version-for-wp-6-2-67e424e85936?source=collection_archive---------19----------------------->

是的，这是真的！好像从 WordPress 6.2 开始**WP 要求的最低 PHP 版本会是 7.2** 。软件在这方面的最后一次更改是在 2019 年 5 月，针对 WordPress 5.2。

![](img/0b274c92a9248ceee68089f1b234a0e8.png)

之前没有更新是因为团队设置了 5%作为版本使用的最大阈值。据[这张票](https://core.trac.wordpress.org/ticket/57345)，PHP 5.6 的使用率终于降到了那个以下。由于 PHP 没有版本 6，版本 7.0 和 7.1 的使用次数更低，最低版本将被提升到 PHP 7.2。这些数字也可以在 WP.org 的[统计页面上找到。](https://wordpress.org/about/stats/)

> 截至 2022 年 12 月 17 日[…]:
> 
> PHP 5.6 占所有 WordPress 网站的 4.99%
> PHP 7.0 占 2.68%
> PHP 7.1 占 1.82%
> 
> [https://core.trac.wordpress.org/ticket/57345](https://core.trac.wordpress.org/ticket/57345)

# WordPress 和 PHP 的时间轴

为了增加向后兼容性，WordPress 对 PHP 最低版本采取了严格的政策。例如，PHP 5.2 的最终版本于 2011 年 1 月发布，但 WP 一直支持到 2019 年。

PHP 5.6 在 2018 年 12 月到达了它的[生命终点](https://www.php.net/eol.php)，4 年后的今天，它仍然是 WordPress 要求的最低版本。如果改变真的发生，我们仍然会落后: **PHP 7.2 的最后一个版本发布于 2020 年 11 月**，两年前。

出于好奇，在我写这篇文章时，仍在接收更新的最老版本[是 PHP 8.0，计划于 2023 年 11 月停止使用。最新版本 8.2 于本月发布，将在 2025 年 12 月前接收安全更新。](https://www.php.net/supported-versions.php)

# WordPress 的 PHP 版本有什么变化

除了在安全性和速度上的改进，PHP 版本的改变也带来了一系列新的特性，使代码库现代化，减少了出错的机会。

你可以在这三个链接中看到可用特性的完整列表: [PHP 7.0](https://www.php.net/manual/en/migration70.new-features.php) 、 [PHP 7.1](https://www.php.net/manual/en/migration71.new-features.php) 和 [PHP 7.2](https://www.php.net/manual/en/migration72.new-features.php) 。下面你可以看到一些例子，我们将 PHP 7.0 作为最低设置:

# 返回类型声明

从 PHP 7.0 开始，可以声明函数或方法返回的类型:

```
function example( array ...$arrays ): array {}
```

虽然在其他一些语言中很常见，但类型声明(用于参数和返回)对一些人来说可能是新事物。

这个特性的最大优点是代码的静态分析:即使不执行代码，也有可能识别出`example()`是否被调用，而不需要期待一个*数组*。如果是这种情况，可以在执行任何其他测试之前检测并修复错误。

如今，大多数 ide 都已经内置了这种功能。

# 零合并运算符(？？)

而不是写作

```
$username = isset( $_GET['user'] ) ? $_GET['user'] : 'nobody';
```

有可能写出它的简化版本:

```
$username = $_GET['user'] ?? 'nobody';
```

# 飞船操作员(<=>

该运算符通常用于比较函数，根据比较结果返回-1、0 或 1。

```
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1
```

# PHP 8 兼容性和 WordPress 版本

与如此广泛的版本保持兼容让事情变得更加困难。与 PHP 8 (8.0、8.1 和 8.2)的兼容性[仍处于测试阶段](https://make.wordpress.org/core/handbook/references/php-compatibility-and-wordpress-versions/)。放弃对 PHP 5.6 的支持会给 PHP 8.x 足够的空间吗？希望如此吧！

你呢，你已经更新了你的 PHP 站点版本了吗？