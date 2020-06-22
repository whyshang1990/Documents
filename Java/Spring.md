# Spring

## Spring Boot

`https://blog.csdn.net/scj1014/article/details/90614104`
在SpringApplication run之前会先进行初始化

```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
        // 初始化 ResourceLoader 默认为null
        this.resourceLoader = resourceLoader;

        Assert.notNull(primarySources, "PrimarySources must not be null");
        this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
        this.webApplicationType = WebApplicationType.deduceFromClasspath();
        // 设置初始化器和监听器
        setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
        setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
        // 获取启动类
        this.mainApplicationClass = deduceMainApplicationClass();
    }
```

### SpringApplication.run()

```java
public ConfigurableApplicationContext run(String... args) {
        // 创建一个StopWatch对象, 开始记录启动时间
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();

        // 如函数名所示配置java headless 属性
        configureHeadlessProperty();

        // 从类路径下的 META-INF/spring.factories文件中找
        // 对应SpringApplicationRunListener的全路径数组，
        // 并通过createSpringFactoriesInstances()方法实例化成对象返回；
        SpringApplicationRunListeners listeners = getRunListeners(args);
        listeners.starting();

        try {
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            // environment 准备阶段
            ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
            configureIgnoreBeanInfo(environment);

            // 功能为打印Banner，也可以自定义启动logo，
            Banner printedBanner = printBanner(environment);
            // 创建ApplicationContext容器，根据类型决定是创建普通WEB容器还是REACTIVE容器还是普通Annotation的ioc容器
            context = createApplicationContext();

            exceptionReporters = getSpringFactoriesInstances(SpringBootExceptionReporter.class,
                    new Class[] { ConfigurableApplicationContext.class }, context);
            prepareContext(context, environment, listeners, applicationArguments, printedBanner);
            refreshContext(context);
            afterRefresh(context, applicationArguments);
            stopWatch.stop();
            if (this.logStartupInfo) {
                new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
            }
            listeners.started(context);
            callRunners(context, applicationArguments);
        }
        catch (Throwable ex) {
            handleRunFailure(context, ex, exceptionReporters, listeners);
            throw new IllegalStateException(ex);
        }

        try {
            listeners.running(context);
        }
        catch (Throwable ex) {
            handleRunFailure(context, ex, exceptionReporters, null);
            throw new IllegalStateException(ex);
        }
        return context;
    }
```

## Spring MVC

- DispatcherServlet
前端控制器用于接收请求

HandlerMapping 和 HandlerAdapter 是@Contoller和@RequestMapping注解的处理器

- HandlerMapping 是处理请求映射的处理器
- HandlerAdapter 是适配器处理器(动态调用方法和处理参数)

> http://www.360doc.com/content/17/1227/11/16915_716684292.shtml
> http://www.360doc.com/content/17/1227/11/16915_716683836.shtml
> http://www.360doc.com/content/17/1227/11/16915_716684230.shtml
> http://www.360doc.com/content/17/1227/11/16915_716684133.shtml
> http://www.360doc.com/content/17/0801/09/16915_675765902.shtml

- DispatcherServlet
前端控制器用于接收请求

### HandlerMapping

### Interface InitializingBean

### HandlerMapping 映射处理器

@RequestMapping和@Controller 封装成RequestMappingInfo，