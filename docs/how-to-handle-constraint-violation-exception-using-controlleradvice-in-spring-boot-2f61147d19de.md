# 如何在 Spring boot 中使用 ControllerAdvice 处理约束违反异常

> 原文：<https://blog.devgenius.io/how-to-handle-constraint-violation-exception-using-controlleradvice-in-spring-boot-2f61147d19de?source=collection_archive---------0----------------------->

Spring boot 提供了一种很好的简单方法，可以使用经验证的注释对 RequestParams 或 PathVariables 进行适当的验证。在本文中，我们将介绍如何处理 Spring boot 抛出的约束违反异常，并使用 ControllerAdvice 发送适当的 HTTP 状态代码作为响应。

假设我们有一个 Get 端点`/api/student?class={class}`，其中我们对`class` RequestParam 进行了@NotNull、@Min 和@Max 验证。(*您可以在* [*官方文档*](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/package-summary.html) *中获得支持的所有验证类型的列表。*)

```
@RestController
@RequestMapping("/api")
@Validated
public class StudentController { /**
     * GET endpoint to get list of students which belong
     * to the specified class     
     * @param class - class
     * @return List of Student
     */
    @GetMapping("/student")
    public List<Student>getStudentByClass(
        @RequestParam(value="class", required=true) 
        @NotNull 
        @Min(1) 
        @Max(12) Integer class) {
            // Code to call the service and get the student list
    }
}
```

对于上述 Get 端点，如果客户端为`class param`发送了一个不正确的值(如 0、13、null 等。)不满足验证，那么 Spring boot 将抛出违反约束的异常。

现在让我们看看如何使用 ControllerAdvice 来处理它。

用`@ControllerAdvice`标注的类是通过类路径扫描自动检测的，它允许我们在一个全局处理组件中处理整个应用程序的异常。

[引自 MDN 网络文档。](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422)

"*超文本传输协议(HTTP)* `***422 Unprocessable Entity***` *响应状态码表示服务器理解请求实体的内容类型，并且请求实体的语法是正确的，但是它无法处理所包含的指令。*

我们将发送`422` Http 状态代码，因为它封装了我们想要响应客户端的内容的本质。

```
@Slf4j
@ControllerAdvice
@RestController
public class ExceptionController { /**
     * Constraint violation exception handler   
     * 
     * @param ex ConstraintViolationException
     * @return List<ValidationError> - list of ValidationError built
     * from set of ConstraintViolation
     */
    @ResponseStatus(HttpStatus.UNPROCESSABLE_ENTITY)
    @ExceptionHandler(ConstraintViolationException.class)
    public List<ValidationError>
        handleConstraintViolation(ConstraintViolationException ex) {
            log.debug(
                "Constraint violation exception encountered: {}",
                ex.getConstraintViolations(),       
                ex
            );
        return buildValidationErrors(ex.getConstraintViolations());
    } /**
     * Build list of ValidationError from set of ConstraintViolation
     *
     * @param violations Set<ConstraintViolation<?>> - Set 
     * of parameterized ConstraintViolations
     * @return List<ValidationError> - list of validation errors
     */
    private List<ValidationError>  buildValidationErrors(
       Set<ConstraintViolation<?>> violations) {
        return violations.
               stream().
               map(violation -> 
               ValidationError.builder().
               field(
                  StreamSupport.stream(
                  violation.getPropertyPath().spliterator(), false).
                  reduce((first, second) -> second).
                  orElse(null).
                  toString()
               ).
               error(violation.getMessage()).
               build()).
               collect(toList());
      } }
```

如果你想了解更多，请浏览这个文档。

1.  [https://docs . spring . io/spring-framework/docs/current/javadoc-API/org/spring framework/web/bind/annotation/controller advice . html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)
2.  [https://docs . Oracle . com/javaee/7/API/javax/validation/constraints/package-summary . html](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/package-summary.html)
3.  [https://www . bael dung . com/exception-handling-for-rest-with-spring](https://www.baeldung.com/exception-handling-for-rest-with-spring)