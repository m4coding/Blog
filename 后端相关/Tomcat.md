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
    
    
#### itellij配置tomcat
    
 ![itellij-1](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-1.png)
 ![itellij-2](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-2.png)
 ![itellij-3](https://github.com/m4coding/Blog/blob/master/%E5%90%8E%E7%AB%AF%E7%9B%B8%E5%85%B3/pic/itellij-tomcat-3.png)
    