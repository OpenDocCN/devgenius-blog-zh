# 面试官:弹簧容器注入豆子的方法有哪些？

> 原文：<https://blog.devgenius.io/interviewer-what-are-the-ways-to-inject-beans-into-spring-container-5c0984b2493d?source=collection_archive---------2----------------------->

![](img/323c71c7802fad6dcc08a88dceba4ed0.png)

你是否一直在使用@Service、@Component、@ Contoller 或@ Bean 来添加你的 Spring beans？

你见过其他人的一些代码在注入 beans 时看起来很花哨，但不知道它是如何达到同样的目的的吗？

下面我们来看看一些常见的将春豆注入容器的方法。

# 1.@Configuration + @Bean

这可能是最常见的一种。

@ Configuration 声明了一个配置类，我们可以使用@ Bean 将 Bean 添加到容器中。

```
@Configuration
public class MyConfiguration {
    @Bean
    public Person person() {
        Person person = new Person();
        person.setName("spring");
        return person;
    }
}
```

# 2.@Component + @ComponentScan

对于同一个示例，我们可以将 Person 对象声明为一个组件。然后我们将使用@ components can“base packages”来声明组件的路径。

```
@Component
public class Person {
    private String name;

    public String getName() {

        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

@ComponentScan(basePackages = "com.springboot.initbean.*")
public class Demo1 {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(Demo1.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}
```

结果:

> 人员{name='null'}

我们已经成功地将 Person bean 注入到 Spring 容器中。

# 3.@导入

有各种灵活的方式来使用这个注释。

## @导入

直接进口豆。一旦我们完成了 Person 类的构造，我们就可以将该类导入到 Spring 容器中。

```
public class Person {
    private String name;

    public String getName() {

        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

@Import(Person.class)
public class Demo1 {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(Demo1.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}
```

## @导入+导入选择器

我们还可以定义一个 *MyImportSelector* 来实现 *ImportSelector* 接口，并覆盖 *selectImports* 方法，在其中我们将指定我们需要的 beans。

现在我们只需要导入 *MyImportSelector* 类*。*

```
@Import(MyImportSelector.class)
public class Demo1 {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(Demo1.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}

class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{"com.springboot.pojo.Person"};
    }
}
```

## @ Import+importbean definitionregistrar

对于这种方法，我们还需要实现一个接口*importbeanditionregistrar*。

```
@Import(MyImportBeanDefinitionRegistrar.class)
public class Demo1 {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(Demo1.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}

class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        // construct beanDefinition
        AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(Person.class).getBeanDefinition();
        // register the bean into the  container
        registry.registerBeanDefinition("person", beanDefinition);
    }
}
```

这种方法与前一种方法的区别在于额外声明了“bean definition”——bean 的元数据。

# 4.@FactoryBean

*FactoryBean* 本身就是一个 Bean，顺便说一下，不要与管理 bean 的 *BeanFactory* 混淆。

```
@Configuration
public class Demo1 {
    @Bean
    public PersonFactoryBean personFactoryBean() {
        return new PersonFactoryBean();
    }

    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(Demo1.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}

class PersonFactoryBean implements FactoryBean<Person> {

    @Override
    public Person getObject() throws Exception {
        return new Person();
    }

    @Override
    public Class<?> getObjectType() {
        return Person.class;
    }
}
```

这里我们使用方法 1 — @ Configuration + @ Bean 将 *PersonFactoryBean* 添加到 Spring 容器中。但是，请注意，我没有在@ Bean 中直接注入 Person bean。相反， *PersonFactoryBean* 被注入。然后，Spring 容器从工厂 bean 中获取 Person bean。

# 5.bean definitionregistrypostprocessor

当 Spring 容器执行*beanDefinitionregistrypostprocesso，*中的*postprocessbeanDefinition registry*方法时，它将单独处理 bean definition，我们可以用它来调整 bean definition。

```
public class Demo1 {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
        MyBeanDefinitionRegistryPostProcessor beanDefinitionRegistryPostProcessor = new MyBeanDefinitionRegistryPostProcessor();
        applicationContext.addBeanFactoryPostProcessor(beanDefinitionRegistryPostProcessor);
        applicationContext.refresh();
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}

class MyBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor {

    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(Person.class).getBeanDefinition();
        registry.registerBeanDefinition("person", beanDefinition);
    }
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {

    }
}
```

我希望这篇文章对你有所帮助。

我是后端软件工程师。如果你渴望了解技术，请关注我的频道，了解我在日常工作和生活中获得的灵感。

> **获取连接:**
> [我的 LinkedIn](https://www.linkedin.com/in/daini-wang-5127b2182)
> 
> **阅读更多:**

[](https://medium.com/@wdn0612/10-best-software-engineering-practices-for-java-f438906c0211) [## Java 的 10 个最佳软件工程实践

### 1.编写有意义的 Java-doc/注释

medium.com](https://medium.com/@wdn0612/10-best-software-engineering-practices-for-java-f438906c0211) [](/my-favorite-5-intellij-plugins-that-can-boost-your-productivity-b30ac73389ce) [## 我最喜欢的 5 个 IntelliJ 插件可以提高你的工作效率

### IntelliJ 已经成为 2022 年 Java 开发中使用最多的 IDE。

blog.devgenius.io](/my-favorite-5-intellij-plugins-that-can-boost-your-productivity-b30ac73389ce) [](/10-java-shortcuts-in-intellij-idea-5ee3a8caa02d) [## IntelliJ IDEA 中的 10 个 Java 快捷方式

### 我上一篇关于我最喜欢的 5 个 IntelliJ 插件的文章得到了很好的反馈。

blog.devgenius.io](/10-java-shortcuts-in-intellij-idea-5ee3a8caa02d)