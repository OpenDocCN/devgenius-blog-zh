# 拉勒维尔:你能抑制回调吗？你当然可以！

> 原文：<https://blog.devgenius.io/laravel-can-you-throttle-a-callback-sure-you-can-2ab64c1a99a3?source=collection_archive---------10----------------------->

## 我想要一些薄煎饼！

![](img/74ad303e28578e752270a430256b1ab0.png)

纳蕾塔·马丁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我非常喜欢节流。有时您需要控制调用逻辑的次数，特别是在联系外部 API 时，以避免请求中的硬错误和停止整个应用程序。

在拉勒维尔，我们可以很容易地做到。[有一个限速器](https://medium.com/p/eb443b1bedc)，你可以用它来抑制回调，或者其他任何事情。但是如果我们可以进一步创建一个助手来节省我们更多的代码行，为什么要止步于此呢？

# 节流就像没有明天一样

让我们做一个全球帮手。我们称之为“节流”，因为它非常简单。它将接受速率限制器所需的密钥和我们想要执行的回调本身。我们可以为尝试次数和衰减窗口设置一些默认值，以分钟为单位。

> 速率限制器使用衰减接近逻辑工作:允许在一个时间窗口内进行给定次数的尝试。一旦第一次尝试超出了时间窗口，就可以进行新的尝试。

过程很简单:我们将询问速率限制器给定的键是否尝试了太多次。如果不是，我们将执行回调并返回 true，否则我们将返回 false，表明回调没有执行。

```
function throttle($key, $callback, $tries = 60, $decay = 1)
{
    $limiter = app(RateLimiter::class); if (! $limiter->tooManyAttempts($key, $tries)) {
        $callback();
        $limiter->hit($key, $decay * 60);
        return true;
    } return false;
}
```

这是一个相对简单的函数，你可以在代码的任何地方使用。只要您不依赖于回调结果本身或者不知道节流何时再次可用，它将在不中断您的应用程序流的情况下节流任何东西。

例如，我们需要向另一个用户发送一条消息，但是要避免在应用程序中向用户发送多条消息。对于这一点，节流功能可以帮助我们。

```
public function send(Request $request)
{
    $request->validate([
        'user'    => 'required|exists:users,id',
        'message' => 'required|string'
    ]); $function = function () use ($request) {
        User::find($request->user)->notify(
            new Message($request->message)
        );
    }; $id = Auth::user()->id; $executed = throttle("send_by:{$id}", $function, 2, 1); if (! $executed) {
        session()->flash('Slow down, you can't spam users!');
    } return back();
}
```

这只是一个例子，不要当真。显然，有更好的方法来抑制发送消息之类的事情，但是节流器可以减轻调用速率限制器的痛苦，并且每次都手动进行抑制，例如，通过用 MD5 散列来识别消息，反复发送相同的消息。

```
public function send(Request $request)
{
    // ... $hash = md5($request->message); $executed = throttle("send_by:{$id}:{$hash}, $function, 2, 1); // ...
}
```

你可以在我的 [Larahelp 包](https://github.com/DarkGhostHunter/Larahelp)里找到这个特质和其他帮手。如果你发现在你的项目中有用的东西，给它一个机会。

[](https://github.com/DarkGhostHunter/Larahelp) [## darghosthunter/Lara help

### 使用超过 35 个有用的全球助手来增强您的 Laravel 项目。您可以通过 composer 安装软件包…

github.com](https://github.com/DarkGhostHunter/Larahelp)