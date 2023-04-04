# 消费来自 Laravel 的任何 HTTP 服务:好方法

> 原文：<https://blog.devgenius.io/consume-any-http-service-from-laravel-the-good-way-e523034e9ad0?source=collection_archive---------0----------------------->

![](img/4bd4dfa263d80275d12b6758220f3dc0.png)

肖恩·林在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

网上有大量的文章解释了如何使用 GuzzleHTTP 来消费来自任何 PHP 项目的 HTTP 服务，当然也包括来自 Laravel 项目的服务。

([在 Twitter 上关注我](https://twitter.com/JuanDMeGon)获取关于 Laravel 和 Web 开发的技巧和想法)

现在，这些文章的主要部分向您展示了如何创建 Guzzle 的客户端类的实例，发送请求并获得响应，如下所示:

```
use GuzzleHttp\Client;$client = new Client([
    'base_uri' => $this->baseUri,
]);$response = $client->request($method, $requestUrl, [
    'query' => $queryParams,
    'form_params' => $formParams,
    'headers' => $headers,
]);$response = $response->getBody()->getContents();
```

# 问题是…

![](img/5cf5127506c8ca416e02eb39bda17b64.png)

不幸的是，这些文章中有几篇只是关注如何从任何 Laravel 组件(通常是控制器)直接使用 GuzzleHTTP，然后返回响应，但是…如果您需要使用几个服务，或者甚至在 Laravel 项目的不同部分使用相同的服务，会发生什么呢？假设您需要从 BarController 的 action foo()中消费一个确定的 HTTP 服务，然后从您的 FooController 的 bar()中消费，也可能从任何特定的模型、服务等中消费。

因此，在此之后，您可能会在整个项目中重复调用 GuzzleHTTP 的客户端类，这听起来可能不错，因为 GuzzleHTTP 为 HTTP 请求提供了一个很好的抽象级别，但是有一种更好的方法可以让您的调用适应客户端类，并将其集中在您的项目中。

归根结底，您需要的是提供向您的项目发送 HTTP 请求的能力，通过任何 HTTP 方法(POST、PUT、PATCH、DELETE 等)向任何 URL 发送请求。)，当然也包括任何正文内容甚至查询参数。

因此，在为客户开发了几个 HTTP 客户端、我的辅助项目和[我的课程](https://www.juandmegon.com/#courses)之后，我发现 PHP 中的特性是一个非常灵活的解决方案，可以添加消费任何外部服务的能力。这样，项目中使用这一特性的任何组件都将能够发送请求。

最后，在这个“介绍”之后，让我向您展示一个可能**使用任何类型的 HTTP 服务**的特征提案，当然是使用 Guzzle:

```
namespace App\Traits;use GuzzleHttp\Client;trait ConsumesExternalServices
{
    /**
     * Send a request to any service
     * [@return](http://twitter.com/return) string
     */
    public function makeRequest($method, $requestUrl, $queryParams = [], $formParams = [], $headers = [], $hasFile = false)
    {
        $client = new Client([
            'base_uri' => $this->baseUri,
        ]);$bodyType = 'form_params';if ($hasFile) {
            $bodyType = 'multipart';$multipart = [];foreach ($formParams as $name => $contents) {
                $multipart[] = [
                    'name' => $name,
                    'contents' => $contents
                ];
            }
        }$response = $client->request($method, $requestUrl, [
            'query' => $queryParams,
            $bodyType => $hasFile ? $multipart : $formParams,
            'headers' => $headers,
        ]);$response = $response->getBody()->getContents();return $response;
    }
}
```

基本上，您是在从 Guzzle 中概括对客户端类的 request()方法的调用，允许任何 HTTP 方法、任何 URL，以及可选的任何查询参数、表单参数头和非常重要的发送文件。

注意到了吗？是的，没人告诉过你用 GuzzleHTTP 发送文件与发送常规请求略有不同。您需要指定一个 multipart，而不是 form_params，此外，您需要对主体的每个字段进行不同的格式化，因为它们都需要“name”和“content”值(这就是为什么有 foreach()的原因)。

现在，一旦你有了这个特性，你就可以使用它并从任何组件调用它，让我们看一个简短的例子:

```
namespace App\Services;use App\Traits\ConsumesExternalServices;class MyExternalService
{
    $baseUri = 'someUri';
    //Or better take that from the config() in the __construct()use ConsumesExternalServices;public function getUsers()
    {
        $this->makeRequest('GET', '/users');
    }public function createUser($data)
    {
        $this->makeRequest('POST', '/users', [], $data);
    }
    ...
}
```

那么…我们已经完成了吗？没那么快，你可以做一些其他的事情来使用这个特性提高你的项目的可伸缩性，并且能够消费你需要的任何服务。例如，您可能希望定制您使用的每个服务解析授权流的方式(发送带有令牌的查询参数？或者发送带有访问令牌的授权头？还是别的？).另外，获取响应后解析响应的方式(是 JSON 吗？XML？明文？HTML？).通过这个[课程](https://www.udemy.com/course/http-client-laravel-guzzle-requests-consume-apis-services/?couponCode=MEDIUM)，我可以帮助你正确解决这个问题。

顺便说一下，**我创建了一个小的** [**包**](https://packagist.org/packages/juandmegon/http-client-tools) 来在我所有的项目中重用这个特性(和一些其他组件)，所以你可能也可以这么做。

最后，您可能希望将您使用的每个服务分离到一个服务类(或一组服务)中，您可以在其中指定如何执行特定的操作(获取用户、获取帖子、发布内容等)。)

现在，你还需要解决其他几件事情。所以，让我来帮你。我已经向你展示了一条很好的路要走，但我当然可以向你展示更多。我来教你如何集成 Laravel 和 Guzzle 来消费任何 HTTP 服务。让我向您展示如何用 Laravel 创建您的 [HTTP 客户端。](https://www.programarya.com/http-client-laravel-guzzle-requests-consume-apis-services)

我们到此为止。我希望这能对你的项目和事业有所帮助。如果你对我的任何课程感兴趣，这里是我的[作品集](https://www.juandmegon.com/#courses)，或者你可以直接选修 [HTTP Clients](https://www.programarya.com/http-client-laravel-guzzle-requests-consume-apis-services) 课程。

最美好的祝愿:)