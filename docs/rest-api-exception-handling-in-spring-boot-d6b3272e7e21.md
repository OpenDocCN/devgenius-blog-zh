# Spring Boot 的 REST API 异常处理

> 原文：<https://blog.devgenius.io/rest-api-exception-handling-in-spring-boot-d6b3272e7e21?source=collection_archive---------0----------------------->

![](img/4b93da1acd11a20dff6e7fdd1f2d6e50.png)

## 什么是例外？

异常是一种不希望的或不可预见的情况，它发生在程序执行期间，即运行时，会中断程序。

在 Spring Boot 如何处理异常？

在 Spring Boot 中有许多处理异常的方法，但是我们将关注“如何为每个 API 全局处理异常？”。

我们将使用@ **ControllerAdvice 和@ExceptionHandler** 在一个地方处理所有异常，因此如果抛出异常，它将通过我们的处理方法。

**@ControllerAdvice**

```
@ControllerAdvice is a specialization of the @Component annotation which allows to handle exceptions across the whole application in one global handling component. It can be viewed as an interceptor of exceptions thrown by methods annotated with @RequestMapping and similar.
```

**@异常处理程序**

```
Annotation for handling exceptions in specific handler classes and/or handler methods.
```

为了处理异常，我们需要为所有异常的响应创建一个通用类。

```
**errorMessage**: The message that we want to display to the end user.**errorCode**: Http Status Code**timestamp**: Time when an exception is thrown
```

现在，对于每种类型的异常，我们必须创建单独的类。

常见使用案例:

*   *未授权异常 401*
*   *未找到资源异常 404*
*   *资源已经存在/冲突 409*
*   *错误请求/自定义异常 400*

**创建类别**

*   UnauthorizedException.java

```
/**
 * @author anuragdhunna
 */
public class UnauthorizedException extends RuntimeException {

    public UnauthorizedException(String message) {
        super(message);
    }

}
```

*   ResourceNotFoundException.java

```
/**
 * @author anuragdhunna 
 */

public class ResourceNotFoundException extends RuntimeException {

    public ResourceNotFoundException(String message) {
        super(message);
    }

}
```

*   ResourceAlreadyExists.java

```
/**
 * @author anuragdhunna 
 */
public class ResourceAlreadyExists extends RuntimeException {

    public ResourceAlreadyExists(String message) {
        super(message);
    }
}
```

*   CustomException.java

```
/**
 * @author anuragdhunna 
 */
public class CustomException extends RuntimeException {

    public CustomException(String message) { super(message); }

}
```

现在让我们使用@ControllerAdvice 创建全局异常处理类

现在所有的异常都通过**GlobalExceptionHandle**r 来处理。

让我们来测试一下:

在您选择的任何控制器中创建方法:

```
@RequestMapping(value = "/testExceptionHandling", 
method = RequestMethod.***GET***)
public String testExceptionHandling(@RequestParam int code) {
    switch (code) {
        case 401:
            throw new UnauthorizedException("You are not authorized");
        case 404:
            throw new ResourceNotFoundException("Requested resource is not found.");
        case 400:
            throw new CustomException("Please provide resource id.");
        case 409:
            throw new ResourceAlreadyExists("Resource already exists in DB.");

        default:
            return "Yeah...No Exception.";

    }
}
```

API:

```
http://localhost:5000/api/testExceptionHandling?code=401
```

回应:

```
{
 "errorMessage": "You are not authorized",
 "errorCode": "UNAUTHORIZED",
 "timestamp": "14-06-2020 04:51:13"
}
```

# 在你走之前…

# 如果你喜欢❤的这篇文章，请点击👏鼓掌。

如果你知道有人可能会提取价值，请**分享**。我会在**评论**、**反馈**中寻找你的**意见**和**建议**。