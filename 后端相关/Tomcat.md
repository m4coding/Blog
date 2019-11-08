### Tomcat

    免费的开放源代码的Web应用服务器，轻量级应用服务器。擅长于对动态资源的处理，是开发JSP和Servlet程序的首选
    
    
[官网地址](http://tomcat.apache.org/)

目录说明：

    bin      -->  命令相关的目录  如有启动或监视Tomcat等命令
    conf     -->  配置文件目录
    lib      -->  存放着运行所需要的库文件
    logs     -->  存放Tomcat运行时的log文件
    temp     -->  存放Tomcat运行时产生的临时文件
    webapps  -->  Tomcat的主要web发布目录，默认情况下把web应用文件放于此目录
    work     -->  存放JSP编译后产生的class文件
    
    
#### docker配置启动tomcat

    mytomcat:
      image: tomcat
      volumes:
        - E:\work\GoServer\tomcat\webapps:/usr/local/tomcat/webapps/
      ports:
        - "8484:8080"
      expose:
        - "8080"
      restart: always
      container_name: tomcat
      
      
    基于tomcat镜像
    
    
#### intellij配置tomcat
    
 ![itellij-1](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-1.png)
 ![itellij-2](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-2.png)
 ![itellij-3](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-3.png)


#### window平台下intellij显示tomcat log乱码问题

    由于tomcat log默认输出的编码时是utf-8，所以输出中文时在window平台下显示会有乱码的情况出现
    为了避免这些情况，在本地的tomcat服务器上进行修改/conf/logging.properties
    将java.util.logging.ConsoleHandler.encoding = UTF-8修改
    java.util.logging.ConsoleHandler.encoding = GBK即可

#### 阿里云ubuntu系统配置tomcat

    1、安装jdk

        wget https://github.com/frekele/oracle-java/releases/download/8u201-b09/jdk-8u201-linux-x64.tar.gz
        tar -zxvf jdk-8u201-linux-x64.tar.gz

        vim /etc/profile

        #配置对应的环境变量,添加在profile文件的最后
        export JAVA_HOME=/opt/java/jdk1.8.0_201
        export JRE_HOME=$JAVA_HOME/jre
        export PATH=$PATH:$JAVA_HOME/bin
        export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar


        #使环境变量生效
        source /etc/profile

    2、安装tomcat

        wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.46/bin/apache-tomcat-8.5.46.tar.gz
        tar -zxvf apache-tomcat-8.5.46.tar.gz

        vim apache-tomcat-8.5.46.tar.gz/bin/startup.sh

        #在startup.sh文件最后添加对应的环境变量
        export CLASSPATH=$CLASSPATH:%JAVA_HOME/lib/tools.jar
        export TOMCAT_HOME=/opt/tomcat/apache-tomcat-8.5.46/
        export CATALINA_HOME=$TOMCAT_HOME
        export PATH=$PATH:$TOMCAT_HOME/bin

        #启动tomcat
        sh ./startup.sh

    3、阿里云添加安全组规则，开放8080端口

    4、阿里云服务器ubuntu系统开放防火墙端口8080

        #防火墙允许8080端口
        ufw allow 8080

        #查看防火墙状态，可以看到8080端口已被允许开放
        ufw status

    5、在浏览器上输入http://阿里云服务器ip地址:8080即可以看到tomcat首页


#### mac平台IntelliJ配置好工程之后运行，tomcat log提示异常

    org.apache.catalina.LifecycleException: Protocol handler initialization failed

    Caused by: java.net.BindException: Address already in use

    解决方法：https://blog.csdn.net/qq_30507287/article/details/80168023


#### javaWeb部署到服务器

    部署的步骤一般的流程是：
    1、将项目打包成war文件
    2、上传到远程服务器
    3、备份数据库文件
    4、将war文件移动到tomcat对应的webapps目录
    5、重启tomcat
    
[【部署】JavaWeb 项目部署到服务器](https://www.jianshu.com/p/aa7e276118cc)


#### nginx代理tomcat上的javaWeb应用

    ps -ef |grep tomcat 用于查看tomcat是否运行

    由于nginx.conf包含了/etc/nginx/conf.d/*.conf，所以在http块下的配置可以选择到conf.d目录下添加即可对应conf文件即可
    
    nginx -t 检查nginx.conf配置是否正确，可以在修改配置后重新加载服务器之前校验一下
    
    nginx -s reload 重新加载

    例如聊天室JavaWeb项目代理配置：
    在/etc/nginx/conf.d目录，创建tomcat.conf，写入如下信息：
    server {
       listen       80;
       # 服务名称
       server_name  nginx_tomcat;
       location /ChatRoom {
           # 指定实际代理的tomcat ChatRoom地址
           proxy_pass http://localhost:8080/ChatRoom;
           # 反向代理的超时时间
           proxy_connect_timeout 10;
           proxy_redirect default;
    
       }
    }
    
    
    有个问题。。在阿里云服务器配置tomcat服务器后，偶尔看进程是启动了，但是用浏览器访问还是不能。。。不知道是不是内存问题导致。。。
    重启服务器后，再起tomcat就又可以了。。。
    
    
    