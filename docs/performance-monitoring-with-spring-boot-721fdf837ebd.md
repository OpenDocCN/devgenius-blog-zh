# 使用 Spring Boot 进行性能监控

> 原文：<https://blog.devgenius.io/performance-monitoring-with-spring-boot-721fdf837ebd?source=collection_archive---------6----------------------->

![](img/030600fb56f80a9598e4b86ad2dd7335.png)

里卡多·罗查在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇文章中，我想展示 Spring Boot 为性能监控提供的一些有趣的特性。

一旦应用程序开始扩展，性能就成了重中之重。我们过度优化应用程序，失去了简单性。这就是软件开发的工作方式。在生产场景中，我们监控应用程序的性能。随着大多数应用程序向云迁移，监控应用程序并不断提高性能至关重要。

如果您使用了弹簧执行器，它会提供大量的统计数据进行监控。以前，我讨论过这个主题[弹簧执行器。](https://betterjavacode.com/2018/09/14/spring-boot-actuator/)

随后，我们将涵盖 Spring Boot 的一些不同的功能。我们将讨论**customizabletraceconnector**、**performance monitor interceptor**和**CommonsRequestLoggingFilter**。

# 使用 CustomizableTraceInterceptor

您可以添加 CustomizableTraceInterceptor 作为一个`Bean`并使用该 Bean 作为您想要拦截的表达式的顾问。这个拦截器允许我们拦截方法调用并添加定制的日志消息。

为了在工作示例中展示这一点，我们将跟踪存储库计时。首先，创建一个扩展`CustomizableTraceInterceptor`的类，如下所示:

```
package com.abccompany.home.performance;import org.aopalliance.intercept.MethodInvocation;
import org.apache.commons.logging.Log;
import org.springframework.aop.framework.AopProxyUtils;
import org.springframework.aop.interceptor.CustomizableTraceInterceptor;
import org.springframework.data.jpa.repository.support.SimpleJpaRepository;public class RepositoryMethodInterceptor extends CustomizableTraceInterceptor
{
    @Override
    protected Class<?> getClassForLogging(Object target)
    {
        Class<?> classForLogging = super.getClassForLogging(target);
        if (SimpleJpaRepository.class.equals(classForLogging))
        {
            Class<?>[] interfaces = AopProxyUtils.proxiedUserInterfaces(target);
            if (interfaces.length > 0)
            {
                return interfaces[0];
            }
        }
        return classForLogging;
    }
 protected void writeToLog(Log logger, String message, Throwable ex)
    {
        if (ex != null)
        {
            logger.info(message, ex);
        }
        else
        {
            logger.info(message);
        }
    } protected boolean isInterceptorEnabled(MethodInvocation invocation, Log logger)
    {
        return true;
    } }
```

我一会儿会解释这个类在做什么。我们需要一个`@Bean`，它将使用这个拦截器来拦截存储库方法。这方面的代码如下所示:

```
package com.abccompany.home.performance;import org.springframework.aop.Advisor;
import org.springframework.aop.aspectj.AspectJExpressionPointcut;
import org.springframework.aop.support.DefaultPointcutAdvisor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class TraceLoggerConfig
{
    @Bean
    public RepositoryMethodInterceptor repositoryMethodInterceptor()
    {
        RepositoryMethodInterceptor repositoryMethodInterceptor = new RepositoryMethodInterceptor();
        repositoryMethodInterceptor.setHideProxyClassNames(true);
        repositoryMethodInterceptor.setUseDynamicLogger(false);
        repositoryMethodInterceptor.setExitMessage("Executed $[methodName] in $[invocationTime] " +
                "ms");
        return repositoryMethodInterceptor;
    } @Bean
    public Advisor advisor()
    {
        AspectJExpressionPointcut aspectJExpressionPointcut = new AspectJExpressionPointcut();
        aspectJExpressionPointcut.setExpression("execution(public * com.abccompany.home" +
                ".repositories.*Repository+.*(..))");
        return new DefaultPointcutAdvisor(aspectJExpressionPointcut, repositoryMethodInterceptor());
    }
}
```

现在，如果我们看看这个配置，这是创建一个使用`RepositoryMethodInterceptor`的 bean，它是`CustomizableTraceInterceptor`的子类。您可以看到，我们使用了一个退出消息来记录存储库方法在这个 bean 中使用的时间。

```
repositoryMethodInterceptor.setExitMessage("Executed $[methodName] in $[invocationTime] " +
        "ms");
```

AspectJExpression 创建一个表达式，应该对其进行包拦截。这个类`RepositoryMethodInterceptor`做了一些有用的事情。首先，它帮助我们**跟踪`Repository`类的类信息**。其次，它**将消息**记录在我们的日志文件中。运行该应用程序后，您将看到如下日志消息:

```
2020-05-24 19:08:04.870  INFO 14724 --- [nio-8443-exec-9] c.r.h.p.RepositoryMethodInterceptor      : Entering method 'findUserByEmail' of class [com.abccompany.home.repositories.UserdataRepository]
Hibernate: select userdata0_.id as id1_4_, userdata0_.email as email2_4_, userdata0_.firstname as firstnam3_4_, userdata0_.guid as guid4_4_, userdata0_.lastname as lastname5_4_, userdata0_.middlename as middlena6_4_, userdata0_.confirmpassword as confirmp7_4_, userdata0_.passwordtxt as password8_4_, userdata0_.phonenumber as phonenum9_4_, userdata0_.role as role10_4_ from userdata userdata0_ where userdata0_.email=?
2020-05-24 19:08:04.872  INFO 14724 --- [nio-8443-exec-9] c.r.h.p.RepositoryMethodInterceptor      : Executed findUserByEmail in 2 ms
2020-05-24 19:08:04.872  INFO 14724 --- [nio-8443-exec-9] c.r.h.p.RepositoryMethodInterceptor      : Entering method 'findAll' of class [com.abccompany.home.repositories.FeedbackRepository]
Hibernate: select feedback0_.id as id1_1_, feedback0_.createdon as createdo2_1_, feedback0_.fromdate as fromdate3_1_, feedback0_.guid as guid4_1_, feedback0_.rating as rating5_1_, feedback0_.rentalpropertyid as rentalpr8_1_, feedback0_.review as review6_1_, feedback0_.todate as todate7_1_, feedback0_.userid as userid9_1_ from feedback feedback0_
2020-05-24 19:08:04.876  INFO 14724 --- [nio-8443-exec-9] c.r.h.p.RepositoryMethodInterceptor      : Executed findAll in 4 ms
```

# 使用 PerformanceMonitorInterceptor

为了使用`PerformanceMonitorInterceptor`，我们将创建一个配置类，并添加一个将创建`PerformanceMonitorInterceptor`的 bean。`AspectJExpressionPointcut`将指向评估我们的控制器类的表达式。

像`CustomizableTraceInterceptor`一样，我们将**有一个子类，它将扩展** `PerformanceMonitoringInterceptor`，这样我们可以在 Spring Boot 日志中记录我们的消息。这将如下所示:

```
package com.abccompany.home.performance;import org.aopalliance.intercept.MethodInvocation;
import org.apache.commons.logging.Log;
import org.springframework.aop.interceptor.PerformanceMonitorInterceptor; public class ControllerMonitoringInterceptor extends PerformanceMonitorInterceptor
{
    protected void writeToLog(Log logger, String message, Throwable ex)
    {
        if (ex != null)
        {
            logger.info(message, ex);
        }
        else
        {
            logger.info(message);
        }
    } protected boolean isInterceptorEnabled(MethodInvocation invocation, Log logger)
    {
        return true;
    }
}
```

我们将为`ControllerMonitoringInterceptor`创建一个 bean。这个 bean 将是我们的日志配置的一部分，它也将评估控制器类的`AspectJExpression`。因此，它将如下所示:

```
package com.abccompany.home.performance;import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.aop.Advisor;
import org.springframework.aop.aspectj.AspectJExpressionPointcut;
import org.springframework.aop.interceptor.PerformanceMonitorInterceptor;
import org.springframework.aop.support.DefaultPointcutAdvisor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;@Configuration
@EnableAspectJAutoProxy
@Aspect
public class ControllerLoggerConfig
{
    @Pointcut("execution(* com.abccompany.home.controllers.*Controller+.*(..))")
    public void monitor()
    { } @Bean
    public ControllerMonitoringInterceptor controllerMonitoringInterceptor()
    {
        return new ControllerMonitoringInterceptor();
    } @Bean
    public Advisor advisorPerformance()
    {
        AspectJExpressionPointcut aspectJExpressionPointcut = new AspectJExpressionPointcut();
        aspectJExpressionPointcut.setExpression("com.abccompany.home.performance" +
                ".ControllerLoggerConfig.monitor()");
        return new DefaultPointcutAdvisor(aspectJExpressionPointcut,
                controllerMonitoringInterceptor());
    }
}
```

现在，如果我们执行应用程序，日志消息将显示控制器类的延迟。

```
2020-05-24 20:12:09.237  INFO 9280 --- [nio-8443-exec-6] c.r.h.p.ControllerMonitoringInterceptor  : StopWatch 'com.abccompany.home.controllers.LoginController.signin': running time (millis) = 0
2020-05-24 20:12:18.263  INFO 9280 --- [nio-8443-exec-2] c.r.h.p.ControllerMonitoringInterceptor  : StopWatch 'com.abccompany.home.controllers.MainController.home': running time (millis) = 43
2020-05-24 20:12:20.025  INFO 9280 --- [nio-8443-exec-9] c.r.h.p.ControllerMonitoringInterceptor  : StopWatch 'com.abccompany.home.controllers.MainController.logout': running time (millis) = 12
2020-05-24 20:12:20.042  INFO 9280 --- [nio-8443-exec-5] c.r.h.p.ControllerMonitoringInterceptor  : StopWatch 'com.abccompany.home.controllers.LoginController.login': running time (millis) = 0
```

# 如何使用 CommonsRequestLoggingFilter

此外，Spring Boot 还提供了一个记录传入请求的有用特性。这有助于监控应用程序并了解请求是如何发出的。为了使用这个特性，我们将创建一个`@Configuration`类 RequestLoggingFilter，如下所示:

```
package com.abccompany.home.performance; import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.filter.AbstractRequestLoggingFilter;
import org.springframework.web.filter.CommonsRequestLoggingFilter;import javax.servlet.http.HttpServletRequest;@Configuration
public class RequestLoggingFilter extends AbstractRequestLoggingFilter
{
    @Bean
    public CommonsRequestLoggingFilter requestLoggingFilterConfig()
    {
        CommonsRequestLoggingFilter commonsRequestLoggingFilter = new CommonsRequestLoggingFilter();
        commonsRequestLoggingFilter.setIncludeClientInfo(true);
        commonsRequestLoggingFilter.setIncludeQueryString(true);
        commonsRequestLoggingFilter.setIncludePayload(true);
        return commonsRequestLoggingFilter;
    } @Override
    protected void beforeRequest (HttpServletRequest request, String message)
    {
        logger.info(message);
    } @Override
    protected void afterRequest (HttpServletRequest request, String message)
    {
        logger.info(message);
    }
}
```

一旦我们添加了这个，我们将在日志中看到如下的`beforeRequest`和`afterRequest`消息:

```
2020-05-24 21:07:15.161  INFO 11984 --- [nio-8443-exec-1] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : Before request [uri=/css/bootstrap.min.css]
2020-05-24 21:07:15.171  INFO 11984 --- [nio-8443-exec-2] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : Before request [uri=/js/jquery.min.js]
2020-05-24 21:07:15.203  INFO 11984 --- [nio-8443-exec-7] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : Before request [uri=/js/bootstrap.min.js]
2020-05-24 21:07:15.290  INFO 11984 --- [nio-8443-exec-7] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : After request [uri=/js/bootstrap.min.js]
2020-05-24 21:07:15.306  INFO 11984 --- [nio-8443-exec-2] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : After request [uri=/js/jquery.min.js]
2020-05-24 21:07:15.318  INFO 11984 --- [nio-8443-exec-1] gFilter$$EnhancerBySpringCGLIB$$cb4fdaab : After request [uri=/css/bootstrap.min.css]
```

# 结论

总之，我展示了三个特性**customizabletrace interceptor**、**performance monitor interceptor**和**CommonsRequestLoggingFilter**来记录有用的性能指标。

如果你喜欢这篇文章，请在你的博客上分享。如果您有任何问题，也可以通过 twitter 上的@betterjavacode 联系我。原文章发表在我的博客[https://betterjavacode . com/2020/05/25/performance-monitoring-with-spring-boot/](https://betterjavacode.com/2020/05/25/performance-monitoring-with-spring-boot/)

# 参考

1.  [弹簧框架特征](https://mdeinum.wordpress.com/2015/07/01/spring-framework-hidden-gems/)
2.  [CommonsRequestLoggingFilter](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/filter/CommonsRequestLoggingFilter.html)