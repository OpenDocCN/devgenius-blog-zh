# Laravel 9 Foreach 循环变量示例

> 原文：<https://blog.devgenius.io/laravel-9-foreach-loop-variable-example-560a8d2b03d5?source=collection_archive---------7----------------------->

在本文中，我们将看到 laravel 9 foreach 循环变量的例子。Laravel 提供了一个简单的刀片模板，该模板提供了许多直接在 HTML 代码中使用的指令。Blade 还为常见的 PHP 控制结构提供了便捷的快捷方式，比如条件语句和循环。Laravel 刀片有一个 foreach 指令。

@foreach 指令与 foreach 循环相同。在迭代一个`foreach`循环时，您可以使用[循环变量](https://laravel.com/docs/9.x/blade#the-loop-variable)来获得关于该循环的有价值的信息。$loop 变量用于获取关于循环中第一次或最后一次迭代的信息。

所以，我们来看看 foreach 循环变量 laravel 9，laravel 9 刀片中的 foreach 循环，控制器中的 laravel 9 foreach 循环。

```
@foreach ($users as $user)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif

    <p>This is user {{ $user->id }}</p>
@endforeach
```

如果你在一个嵌套循环中，你可以通过`parent`属性访问父循环的`$loop`变量。

```
@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is the first iteration of the parent loop.
        @endif
    @endforeach
@endforeach
```

**读也:** [**拉韦勒 9 拔毛法举例**](https://websolutionstuff.com/post/laravel-9-pluck-method-example)

当前循环迭代的索引(从 0 开始)。

```
@php
$numbers = [1, 3, 5, 7, 9, 11];
@endphp@foreach ($numbers as $number)
    @if ($loop->index == 2)
        {{ $number }} // 5
    @endif
@endforeach
```

当前循环迭代(从 1 开始)。

```
@php
$numbers = [1, 3, 5, 7, 9, 11];
@endphp@foreach ($numbers as $number)
    @if ($loop->iteration == 3)
        {{ $number }} // 5
    @endif
@endforeach
```

`$loop`变量还包含许多其他有用的属性:

PropertyDescription `$loop->index`当前循环迭代的索引(从 0 开始)。`$loop->iteration`当前循环迭代(从 1 开始)。`$loop->remaining`循环中剩余的迭代次数。`$loop->count`被迭代的数组中的总项数。`$loop->first`这是否是循环的第一次迭代。`$loop->last`这是否是循环的最后一次迭代。`$loop->even`这是否是循环中的一次均匀迭代。`$loop->odd`这是否是循环中的一次奇数迭代。`$loop->depth`当前循环的嵌套层次。`$loop->parent`在嵌套循环中时，父循环变量。

**你可能也会喜欢:**

*   **读也:** [**Laravel 9 有许多通过关系的例子**](https://websolutionstuff.com/post/laravel-9-has-many-through-relationship-example)
*   **阅读还:** [**Laravel 9 图片上传于 Summernote 编辑**](https://websolutionstuff.com/post/laravel-9-image-upload-in-summernote-editor)
*   **读也:** [**Laravel 9 一对一关系示例**](https://websolutionstuff.com/post/laravel-9-one-to-one-relationship-example)
*   **阅读另:** [**如何在 Laravel 9**](https://websolutionstuff.com/post/how-to-upload-multiple-image-in-laravel-9) 中上传多个图像