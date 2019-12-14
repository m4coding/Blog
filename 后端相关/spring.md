### Spring

#### 概念

    面向切面编程
    
    控制反转
    

### SpringBoot

* @SpringBootApplication

        注解：开启Spring的自动配置和组件扫描功能

* SpringApplication.run

        可以在命令行中将应用当做一个jar包来运行，便于调试开发


* Maven或Gradle中的SpringBoot插件

        构建插件的主要功能是把项目打包成一个可执行的超级JAR（uber-JAR），包括把应用程序的所有依赖打入JAR文件内，
        并为JAR添加一个描述文件，其中的内容能让你用java -jar来运行应用程序。


#### Spring Web Security

Spring安全性配置

    通过重载WebSecurityConfigurerAdapter来实现配置安全性
    有三个configure方法：
        configure(WebSecurity)  通过重载，配置Spring Security的Filter链
        configure(HttpSecurity) 通过重载，配置如何通过拦截器保护请求
        configure(AuthenticationManagerBuilder)  通过重载，配置user-detail服务


* 使用转码后的密码

    如果直接明文保存密码在数据库中，那么黑客窃取之后，将会导致密码暴露，所以可以通过转码存储

    spring security模块内置三个加密器：
        1、BCryptPasswordEncoder  基于BCrypt的加密器
        2、NoOpPasswordEncoder   一个什么都不做的密码编码器
        3、StandardPasswordEncoder  标准加密器，基于SHA-256加密

    当内置的加密器满足不了需求时，可以通过实现PasswordEncoder接口来实现自定义加密器

    不管你使用哪一个密码转码器，都需要理解的一点是，“数据库中的密码是永远不会解码的”（正确的策略是不能反向解码）
    所采取的策略与之相反，用户在登录时输入的密码会按照相同的算法进行转码，然后再与数据库中已经转码过的密码进行对比
    这个对比是在PasswordEncoder的matches()方法中进行的。

* Authentication鉴权

    1、username和password被获得后封装到一个UsernamePasswordAuthenticationToken（Authentication接口的实例）的实例中
    2、这个token被传递给AuthenticationManager进行验证
    3、成功认证后AuthenticationManager将返回一个得到完整填充的Authentication实例
    4、通过调用SecurityContextHolder.getContext().setAuthentication(...)，参数传递authentication对象，来建立安全上下文（security context）

* 添加了spring security的简单配置后，如果不重写configure(HttpSecurity http)方法，运行起应用后默认会进入一个登陆页的，如果重写后可以通过formLogin()再次添加


* 错误处理

    AuthenticationEntryPoint 用来解决匿名用户访问无权限资源时的异常  （未登录）
    AccessDeniedHandler 用来解决认证过的用户访问无权限资源时的异常 （已登录）


#### JSON WEB TOKEN （JWT）

为解决以往session认证需要存储的问题

结构：

    1、header  头部信息，包含声明类型和加密算法
    2、payload 载荷信息  存储一些有用信息，例如用户相关的信息或token本身的有效期
    3、signature 签证信息 又三部分组合而成，header base64编码后加payload base64编码后 再加私钥结合header中声明的算法加密而成

[什么是 JWT -- JSON WEB TOKEN](https://www.jianshu.com/p/576dbf44b2ae)

#### CSRF

跨站点请求伪造 （Cross-Site request forgery）

[CSRF是什么](https://www.jianshu.com/p/e825e67fcf28)

#### 日志相关

日志输出一般使用的架构

1、logback + slf4j

    slf4j实现日志api，logback实现日志底层

2、Commons Logging + Log4j

    Commons Logging实现日志api，Log4j实现日志底层

总体而言，logback的性能好于Log4j


#### SpringBoot部署

构建并运行springboot应用有多种方式：

    1、在IDE中运行应用程序（如Spring ToolSuite或IntelliJ IDEA）
    2、使用Maven的spring-boot：run或Gradle的bootRun，在命令行里运行
    3、使用Maven或Gradle生成可运行的JAR文件，随后在命令行中运行
    4、使用Spring Boot CLI在命令行中运行Groovy脚本
    5、使用Spring Boot CLI来生成可运行的JAR文件，随后在命令行中运行


#### 属性配置

通过外部的属性文件application.properties或application.yml可以引入配置

同时可以通过profile来指定测试环境与生产环境各自的profile配置


#### MyBatis相关配置

发现一个坑，如果使用MyBatis Generator生产了一次mapper xml文件，然后再次执行Generator时，原来的文件不会删除掉，而新生成的代码会添加在之前内容后面，这样就会造成编译异常。。。

    例如：Cause: java.lang.IllegalArgumentException: Result Maps collection already contains value for com.m4coding.mallmbg.mbg.mapper.PmsProductMapper.BaseResultMap


#### @Controller和@RestController的区别

@RestController相当于是整合了@Controller和@ResponseBody

方法参数如果指定了@ResponseBody，默认是接受json参数，可以通过@RequestMapping的consumes参数指定修改

[Springboot @Controller和@RestController](https://blog.csdn.net/qq_37866486/article/details/90700557)


#### @Valid加BindingResult，校验入参

有些时候需要校验入参时，可以通过注解来在对应的bean字段指定判断条件，避免在Controller中一个一个的if判断处理

如注解@NotEmpty，如果String字段为null或""，则会提示message信息

[使用@Valid+BindingResult进行controller参数校验](https://blog.csdn.net/fu250/article/details/80247930)


#### 接口版本管理

目前接口版本管理有两种做法：

    1、在url中加入版本号
    2、在请求中加入版本号
    
[spring boot中restfull api版本控制](https://blog.csdn.net/u013467442/article/details/89382501)


添加对应的版本控制后，发现swagger ui出现404，找不到，是由于继承了WebMvcConfigurationSupport，影响到了。。。

重新在WebMvcConfigurationSupport注册swagger ui的静态资源即可，可能是继承后冲掉原来swagger的配置

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }    
    
然后在springboot Application上加上@EnableWebMvc即可

[SpringBoot swagger-ui.html 配置类继承 WebMvcConfigurationSupport 类后 请求404](https://www.cnblogs.com/laotan/p/9324717.html)


[springboot-接口版本区分](https://blog.csdn.net/j903829182/article/details/81836551)


#### 利用“切面”编程

描述切面的术语：

    1、advice 通知
        |--- 前置通知(Before)：在目标方法被调用之前调用通知功能；
        |--- 后置通知(After)：在目标方法完成之后调用通知，此时不会关心方法的输出时什么；
        |--- 返回通知(After-returning)：在目标方法成功执行之后调用通知；
        |--- 异常通知(After-throwing)： 在目标方法抛出异常后调用通知；
        |--- 环绕通知(Around)：通知包裹了被通知的方法，在被通知方法调用之前和调用之后执行自定义的行为
                （类似于在一个通知方法中同时编写前置通知和后置通知）

    2、pointcut 切点

    3、join point 连接点

        连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、
            甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

Spring提供了4种类型的AOP支持：

    1、基于代理的经典String AOP
    2、纯POJO切面
    3、@AspecJ注解驱动的切面
    4、注入式AspecJ切面（适用于Spring各版本）

Spring自带的AOP只支持方法拦截，若需要方法之外的连接点拦截功能，那么可以利用Aspect来补充AOP的功能

为了使用切面，还使能配置@EnableAspectJAutoProxy，启用AspectJ自动代理  （springboot中不需要配置，如果引入了aop的依赖，会自动配置的）


#### 单元测试

@SpringRunner和@SpringJUnit4ClassRunner的区别：

    @RunWith(SpringRunner.class)
    @RunWith(SpringJUnit4ClassRunner.class)
    
    SpringRunner 继承了SpringJUnit4ClassRunner，没有扩展任何功能；使用前者，名字简短而已。。。
    

@SpringApplicationConfiguration已被@SpringBootTest替代，从1.4.0版本开始


对于@SpingBootTest有四种webEnvironment：

    1、MOCK —提供一个虚拟的Servlet环境，内置的Servlet容器并没有真实的启动
    2、RANDOM_PORT — 提供一个真实的Servlet环境，也就是说会启动内置容器，然后使用的是随机端口
    3、DEFINED_PORT — 这个配置也是提供一个真实的Servlet环境，使用的配置文件中配置的端口，如果没有配置，默认是8080
    4、NONE  - 没有web环境的配置
    
[SpringBoot（五）SpringBoot的单元测试](https://segmentfault.com/a/1190000014731019?utm_source=tag-newest)