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