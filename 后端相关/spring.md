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