# 库存——商店请求和响应

> 原文：<https://blog.devgenius.io/stockpile-store-request-and-response-8075a3e02515?source=collection_archive---------18----------------------->

![](img/775eafadf422866fa43783fc3357dea0.png)

一件——什么？你不用股票堆？

对于大多数应用程序来说，存储 API 的请求和响应非常重要，尤其是在与外部服务提供者集成时。
记录这些信息可以解决大多数用例，但是当请求/响应包含敏感信息时可能就不行了。

在这种情况下，一种方法是加密并存储请求和响应；这正是我们在这篇文章中要做的。

概述:
1。其请求和响应将被存储的方法/命令被标注为`@StockPile`
2。要存储的方法的参数用`@Item`标注。
3。方法拦截器来加密和存储这些方法的请求、响应/异常。

```
@Retention(RetentionPolicy.RUNTIME)
@Target({METHOD})
@BindingAnnotation
public @interface StockPile {
}
```

`@StockPile`注释仅针对方法，`@Target` —方法

```
@Retention(RetentionPolicy.RUNTIME)
@Target({FIELD, PARAMETER})
@BindingAnnotation
public @interface Item {
}
```

`@Item`表示方法的参数，`@Target` —参数

```
@Slf4j
public class StockPileModule extends AbstractModule { @Override
    protected void configure() {
        final StockPileInterceptor stockPileInterceptor = new StockPileInterceptor();
        requestInjection(stockPileInterceptor);
        bindInterceptor(Matchers.any(), Matchers.annotatedWith(StockPile.class), stockPileInterceptor);
    } @VisibleForTesting
    public static class StockPileInterceptor implements MethodInterceptor { @VisibleForTesting
        @Inject
        protected YourEncryptionService encryptionService; @VisibleForTesting
        @Inject
        protected YourStorageService storageDao; @VisibleForTesting
        @Inject
        protected ObjectMapper objectMapper; @Override
        public Object invoke(MethodInvocation invocation) throws Throwable {
            final Optional<Annotation> optionalAnnotation = Stream.of(invocation.getMethod().getDeclaredAnnotations())
                    .filter(annotation -> annotation instanceof StockPile)
                    .findFirst(); if (optionalAnnotation.isPresent()) {
                List<Object> requests = new ArrayList<>();
                final String methodName = invocation.getMethod().getName();
                final Annotation[][] parameterAnnotations = invocation.getMethod().getParameterAnnotations(); for (int index = 0; index < parameterAnnotations.length; ++index) {
                    for (final Annotation annotation : parameterAnnotations[index]) {
                        if (annotation instanceof Item) {
                            requests.add(invocation.getArguments()[index]);
                        }
                    }
                }
                final Object response;
                try {
                    response = invocation.proceed();
                    storageDao.save(requests, response, methodName);
                } catch (Exception e) {
                    storageDao.save(requests, e, methodName);
                    throw e;
                }
                return response;
            }
            return invocation.proceed();
        }
    }   
}
```

这篇文章的范围是概括请求和响应的存储。由开发人员决定存储这些 blobs 的加密和数据存储类型(NoSQL，如 AeroSpike 是一个不错的选择)。

正如在上面的方法拦截器中看到的，我们首先验证方法是否用`@StockPile`注释，然后所有用`@Item`注释的参数被添加到`List<Object>`

**storageDao.save** 带三个参数:
1。请求(`Object` )
2。响应或异常(`Object` )
3。方法名称(`String`)

```
private void save(Object request, Object response, String commandName) {
    try {
        final String userId = MDC.get(RequestContext.REQUEST_USER_ID);
        final String requestId = MDC.get(RequestContext.REQUEST_ID);

        final String workflowId = Objects.nonNull(REQUEST_USER_ID) ? userId.concat("_").concat(requestId) : userId;
        final Date currentDate = new Date(); Optional<RequestResponseBlob> requestResponseBlob = storageDao
                .get(workflowId, commandName); final byte[] encryptedRequest = objectMapper.writeValueAsBytes(encryptionService.encrypt(request));
        final byte[] encryptedResponse = objectMapper.writeValueAsBytes(encryptionService.encrypt(response)); if (storedOutboundMessage.isPresent()) {
            RequestResponseBlob presentRequestResponse = requestResponseBlob.get();
            presentRequestResponse.setRequest(encryptedRequest);
            presentRequestResponse.setResponse(encryptedResponse);
            presentRequestResponse.setUpdated(currentDate);
            storageDao.update(presentRequestResponse);
        } else {
            storageDao.save(RequestResponseBlob.builder()
                    .commandType(commandName)
                    .workflowId(workflowId)
                    .requestId(requestId)
                    .request(encryptedRequest)
                    .response(encryptedResponse)
                    .created(currentDate)
                    .updated(currentDate)
                    .build());
        }
    } catch (Exception e) {
        log.error("[STOCKPILE] Error while storing");
    }
}
```

在 **MDC** 中存放`userId`和`requestId`参见[前一岗位](https://pyblog.medium.com/mdc-improving-debugging-logging-bb421a62a195)。
为了便于查询，使用`userId`、`requestId`和**方法名**会比较理想，如上图所示。

用法:

```
@StockPile
public Response externalServiceCommandOne(@Item final SensitiveInformationRequest request, final String token) {
  Response response;
  // Stuff here
  return response
}
```

就是这样！完成的🚀