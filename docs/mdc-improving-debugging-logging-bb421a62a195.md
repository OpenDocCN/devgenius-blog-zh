# MDC —改进调试日志记录

> 原文：<https://blog.devgenius.io/mdc-improving-debugging-logging-bb421a62a195?source=collection_archive---------5----------------------->

![](img/56fbb1f3c3b0bfb7a31d4e79ed96fdda.png)

建筑商鲍勃同意！

将你的日志构建得和[建造者鲍勃](http://www.bobthebuilder.com/en-us/)一样好🤪

通常，在大多数后端应用程序中，线程池中的一个*独立线程处理一个客户端请求*。一旦请求完成，线程就被返回到线程池，并生成其典型的日志。

我们要解决什么？让我们以**为例**。
考虑这样一种情况:两个用户*已经登录并从数据库中检索数据。通过区分线程 ID，可以很容易地看出哪个日志语句属于哪个请求。这种基于线程的日志关联是从单个请求中识别所有日志的有用技术。然而，使用这种技术，我们无法区分哪个请求属于哪个用户。现在，当实现多线程时，事情变得更加混乱。*

**部分解决方案** :
其中一种方法是在请求进入服务时生成一个惟一的编号，并在每个日志语句中打印出来，但是这又一次很难与用户相关联

**解决方案:**
另一个解决方案是为每个日志语句打印*用户 ID* 和*请求 ID* 。*请求 ID* 将在客户端生成。对于用户的每个请求，请求 ID 都是唯一的。如果有来自多个用户的并发请求，这将有助于识别一个请求(因为我们也有*用户 ID* )。这些作为 HTTP 头与每个请求一起传递，并附加到每个日志行。

**方法**:
[MDC](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html)提供了一个简单的键/值(map)机制来捕获少量的定制诊断数据。因为它内置在日志框架中，所以从 MDC 向每个日志行添加值非常容易。log4j、log4j2 和 SL4J/logback 支持 MDC。

MDC 允许我们在一个类似 map 的结构中填充一些信息，当日志消息被写入时，appender 可以访问这些信息。MDC 结构以与 *ThreadLocal* 变量相同的方式在内部附加到执行线程。

**高层想法**是:

*   在线程开始时，用日志附加器所需的信息填充 MDC
*   记录消息

**实现** :
应用栈:[Java](https://www.java.com/en/download/)—[drop wizard](http://dropwizard.io/en/stable/)
然而，这种实现背后的思想适用于任何框架。

所有资源的方法拦截器:

```
/**
 * @author adesh.nalpet
 * created on 20th December 2019
 * A resource method interceptor for adding request context to MDC (Logger)
 */
@Slf4j
public class RequestContextModule extends AbstractModule { @Override
    protected void configure() {
        final RequestContextInterceptor requestContextInterceptor = new RequestContextInterceptor();
        requestInjection(requestContextInterceptor);
        bindInterceptor(Matchers.any(), Matchers.annotatedWith(Path.class), requestContextInterceptor);
    } @VisibleForTesting
    public static class RequestContextInterceptor implements MethodInterceptor { @Override
        public Object invoke(MethodInvocation invocation) throws Throwable { /* Applicable for methods annotated with @Path (Resources) */
            final Optional<Annotation> optionalAnnotation = Stream.of(invocation.getMethod().getDeclaredAnnotations())
                    .filter(annotation -> annotation instanceof Path)
                    .findFirst(); if (optionalAnnotation.isPresent()) {
                final Annotation[][] parameterAnnotations = invocation.getMethod().getParameterAnnotations(); for (int index = 0; index < parameterAnnotations.length; ++index) {
                    for (final Annotation annotation : parameterAnnotations[index]) {
                        /* Check for method arguments annotated with @Auth */
                        if (annotation instanceof Auth) {
                            try {
                                final Auth auth = (Auth) invocation.getArguments()[index];
                                /* Add request and user ID to MDC
                                Note :
                                * Any information in Auth can be logged.
                                * Consider compliance restrictions before logging any sensitive information.
                                */
                                MDC.put(RequestContext.REQUEST_ID, auth.getRequestId());
                                MDC.put(RequestContext.REQUEST_USER_ID, auth.getUserId());
                            } catch (Exception e) {
                                /* Deliberately catching all exceptions,
                                to ensure application is not affected from logger
                                TODO : Set-up alerts for the below log */
                                log.error("[Auth to MDC] Error while fetching Auth");
                            }
                        }
                    }
                }
            }
            return invocation.proceed();
        }
    }
}
```

清除上下文的响应过滤器:

```
/**
 * @author adesh.nalpet
 * created on 20th December 2019
 * A Servlet response filter to clear MDC.
 */
public class ClearContextResponseFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        /* deliberately nothing */
    } @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        try {
            chain.doFilter(request, response);
        } finally {
            RequestContext.clearContext();
        }
    } @Override
    public void destroy() {
        /* deliberately nothing */
    }
}
```

请求上下文:

```
/**
 * @author adesh.nalpet
 * created on 20th December 2019
 * TODO : Validate storing RequestContext in thread local.
 */
public class RequestContext { public static final String REQUEST_ID = "request_id";
    public static final String REQUEST_USER_ID = "user_id"; public static void clearContext() {
        MDC.remove(REQUEST_ID);
        MDC.remove(REQUEST_USER_ID);
    }
}
```

日志格式:

```
logFormat: "%(%-5level) [%date] %X{request_id} %X{user_id} [%thread] [%logger{0}]: %message%n"
```

走向 MDC🚀