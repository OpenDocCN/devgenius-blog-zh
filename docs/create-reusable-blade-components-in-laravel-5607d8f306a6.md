# 在 Laravel 中创建可重用的刀片组件

> 原文：<https://blog.devgenius.io/create-reusable-blade-components-in-laravel-5607d8f306a6?source=collection_archive---------1----------------------->

## Laravel 刀片组件——Laravel 从头开始创建管理面板——第 12 部分

![](img/f4fe8084c54d51d14c4ad34d0bd74b8d.png)

照片由 [micheile dot com](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 刀片模板

刀片是 PHP 模板引擎。刀片模板允许直接使用 PHP 代码。文件扩展名为`.blade.php`，存储在`resources/views`目录中

> Blade 是 Laravel 附带的简单而强大的模板引擎。

# 刀片组件

刀片组件类似于剖面、布局和包含。我们可以创建两种类型的组件。

1.  基于类的组件
2.  匿名组件

`make:component` Artisan 命令用于创建基于类的组件。

```
php artisan make:component Admin/Form/Input
```

要创建一个匿名组件，您可以在调用`make:component`命令时使用`--view`标志。

```
php artisan make:component admin.form.input --view
```

上述命令将在`resources/views/components/admin/form/input.blade.php`创建一个刀片文件

阅读更多官方文件

# 创建可重复使用的刀片组件

我们将为我们的[管理面板](https://github.com/balajidharma/basic-laravel-admin-panel)创建一些组件。在我们看来，这些组件将减少行数。目前，我们的用户索引视图文件有以下代码

`resources/views/admin/user/index.blade.php`

```
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Users') }}
        </h2>
    </x-slot><div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <div class="flex flex-col mt-8">
                    @can('user create')
                    <div class="d-print-none with-border mb-8">
                        <a href="{{ route('user.create') }}" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">{{ __('Add User') }}</a>
                    </div>
                    @endcan
                        <div class="py-2">
                        @if(session()->has('message'))
                            <div class="mb-8 text-green-400 font-bold">
                                {{ session()->get('message') }}
                            </div>
                        @endif
                            <div class="min-w-full border-b border-gray-200 shadow overflow-x-auto">
                                <form method="GET" action="{{ route('user.index') }}">
                                <div class="py-2 flex">
                                    <div class="flex pl-2">
                                        <input type="search" name="search" value="{{ request()->input('search') }}" class="rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" placeholder="Search">
                                        <button type='submit' class='ml-4 inline-flex items-center px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-700 active:bg-gray-900 focus:outline-none focus:border-gray-900 focus:ring ring-gray-300 disabled:opacity-25 transition ease-in-out duration-150'>
                                            {{ __('Search') }}
                                        </button>
                                    </div>
                                </div>
                                </form>
                                <table class="border-collapse table-auto w-full text-sm">
                                    <thead>
                                        <tr>
                                            <th class="py-4 px-4 bg-grey-lightest font-bold uppercase text-sm text-grey-dark border-b border-grey-light text-left">
                                                @include('admin.includes.sort-link', ['label' => 'Name', 'attribute' => 'name'])
                                            </th>
                                            <th class="py-4 px-4 bg-grey-lightest font-bold uppercase text-sm text-grey-dark border-b border-grey-light text-left">
                                                @include('admin.includes.sort-link', ['label' => 'Email', 'attribute' => 'email'])
                                            </th>
                                            @canany(['user edit', 'user delete'])
                                            <th class="py-4 px-4 bg-grey-lightest font-bold uppercase text-sm text-grey-dark border-b border-grey-light text-left">
                                                {{ __('Actions') }}
                                            </th>
                                            @endcanany
                                        </tr>
                                    </thead><tbody class="bg-white dark:bg-slate-800">
                                        @foreach($users as $user)
                                        <tr>
                                            <td class="border-b border-slate-100 dark:border-slate-700 p-4 text-slate-500 dark:text-slate-400">
                                                <div class="text-sm text-gray-900">
                                                    <a href="{{route('user.show', $user->id)}}" class="no-underline hover:underline text-cyan-600 dark:text-cyan-400">{{ $user->name }}</a>
                                                </div>
                                            </td>
                                            <td class="border-b border-slate-100 dark:border-slate-700 p-4 text-slate-500 dark:text-slate-400">
                                                <div class="text-sm text-gray-900">
                                                    {{ $user->email }}
                                                </div>
                                            </td>
                                            @canany(['user edit', 'user delete'])
                                            <td class="border-b border-slate-100 dark:border-slate-700 p-4 text-slate-500 dark:text-slate-400">
                                                <form action="{{ route('user.destroy', $user->id) }}" method="POST">
                                                    <div class="flex">
                                                        @can('user edit')
                                                        <a href="{{route('user.edit', $user->id)}}" class="px-4 py-2 text-white mr-4 bg-blue-600">
                                                            {{ __('Edit') }}
                                                        </a>
                                                        @endcan@can('user delete')
                                                        @csrf
                                                        @method('DELETE')
                                                        <button class="px-4 py-2 text-white bg-red-600">
                                                            {{ __('Delete') }}
                                                        </button>
                                                        @endcan
                                                    </div>
                                                </form>
                                            </td>
                                            @endcanany
                                        </tr>
                                        @endforeach
                                    </tbody>
                                </table>
                            </div>
                            <div class="py-8">
                                {{ $users->appends(request()->query())->links() }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

创建组件后

```
<x-admin.wrapper>
    <x-slot name="title">
        {{ __('Users') }}
    </x-slot>@can('user create')
    <x-admin.add-link href="{{ route('user.create') }}">
        {{ __('Add User') }}
    </x-admin.add-link>
    @endcan<div class="py-2">
        <x-admin.message />
        <div class="min-w-full border-b border-gray-200 shadow overflow-x-auto">
            <x-admin.grid.search action="{{ route('user.index') }}" />
            <x-admin.grid.table>
                <x-slot name="head">
                    <tr>
                        <x-admin.grid.th>
                            @include('admin.includes.sort-link', ['label' => 'Name', 'attribute' => 'name'])
                        </x-admin.grid.th>
                        <x-admin.grid.th>
                            @include('admin.includes.sort-link', ['label' => 'Email', 'attribute' => 'email'])
                        </x-admin.grid.th>
                        @canany(['user edit', 'user delete'])
                        <x-admin.grid.th>
                            {{ __('Actions') }}
                        </x-admin.grid.th>
                        @endcanany
                    </tr>
                </x-slot>
                <x-slot name="body">
                @foreach($users as $user)
                    <tr>
                        <x-admin.grid.td>
                            <div class="text-sm text-gray-900">
                                <a href="{{route('user.show', $user->id)}}" class="no-underline hover:underline text-cyan-600 dark:text-cyan-400">{{ $user->name }}</a>
                            </div>
                        </x-admin.grid.td>
                        <x-admin.grid.td>
                            <div class="text-sm text-gray-900">
                                {{ $user->email }}
                            </div>
                        </x-admin.grid.td>
                        @canany(['user edit', 'user delete'])
                        <x-admin.grid.td>
                            <form action="{{ route('user.destroy', $user->id) }}" method="POST">
                                <div class="flex">
                                    @can('user edit')
                                    <a href="{{route('user.edit', $user->id)}}" class="px-4 py-2 text-white mr-4 bg-blue-600">
                                        {{ __('Edit') }}
                                    </a>
                                    @endcan@can('user delete')
                                    @csrf
                                    @method('DELETE')
                                    <button class="px-4 py-2 text-white bg-red-600">
                                        {{ __('Delete') }}
                                    </button>
                                    @endcan
                                </div>
                            </form>
                        </x-admin.grid.td>
                        @endcanany
                    </tr>
                    @endforeach
                </x-slot>
            </x-admin.grid.table>
        </div>
        <div class="py-8">
            {{ $users->appends(request()->query())->links() }}
        </div>
    </div>
</x-admin.wrapper>
```

我们在`index.blade.php`中创建了以下组件

*   `admin.wrapper`
*   `admin.add-link`
*   `admin.message`
*   `admin.grid.search`
*   `admin.grid.table`
*   `admin.grid.th`
*   `admin.grid.td`

## 渲染刀片组件

为了呈现 Blade 组件，标签以字符串`x-`开始，后跟组件类的 kebab case 名称:

```
<x-alert/><x-user-profile/>
```

如果组件类在`App\View\Components`目录中嵌套更深，您可以使用`.`字符来表示目录嵌套。

```
<x-admin.wrapper></x-admin.wrapper>
```

Laravel 管理面板在[https://github.com/balajidharma/basic-laravel-admin-panel](https://github.com/balajidharma/basic-laravel-admin-panel)上可用。安装管理面板并分享您的反馈。

感谢您的阅读。

敬请关注更多内容！

*跟我来*[](https://balajidharma.medium.com/)*。*

*前一部分—第 11 部分:[使用服务&动作类](/restructuring-a-laravel-controller-using-services-action-classes-288b802122b5)重构 Laravel 控制器*

*下一部分—第 13 部分:[在 Laravel CRUD 中执行删除确认](https://balajidharma.medium.com/implement-delete-confirmation-in-laravel-crud-2180f93b5a87)*