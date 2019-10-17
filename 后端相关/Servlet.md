### Servlet

    Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。
    

#### Servlet api包下载

    由于java标准包没有包含Servlet，所以需要自行下载jar包
    1、Servlet api包可以在tomcat目录lib中找到，例如apache-tomcat-9.0.26/lib/servlet-api.jar
    2、通过maven进行下载，例如
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        当然也可以直接通过intellij进行maven导入servlet api包
    
#### Servlet生命周期

    
    Servlet容器实例化Servlet时    -->初始化Servlet：调用init()方法
                                  -->启客户端请求时，调用service()方法，每次请求会调用一次，各调用过程会在运行在不同线程中，互不干扰
                                  -->根据请求执行doGet()或doPost()等方法      
    当服务关闭时                  -->最后调用destroy()方法，卸载Servlet，释放内存资源
    

#### Servlet配置

    继承于HttpServlet，创建了Servlet类后，还需要配置才可以使用
    在Servlet3.0之前版本配置：
        只能在web.xml文件配置。如：
        <servlet>
                <!-- 自定义servlet名称，一般为类名-->
                <servlet-name>LoginAction</servlet-name>
                <!-- 指定Servlet类的路径-->
                <servlet-class>com.m4coding.chatroom.servlet.LoginAction</servlet-class>
            </servlet>

            <servlet-mapping>
                <!-- 这里要填<servlet>中的<servlet-name>值，保持一致-->
                <servlet-name>LoginAction</servlet-name>
                <!-- 表示访问路径http://localhost:8080/LoginAction  /是不能丢掉的，一定要写的-->
                <url-pattern>/LoginAction</url-pattern>
            </servlet-mapping>

    在Servlet3.0之后版本配置（包括3.0版本）：
        支持web.xml配置时，同时又添加注解方式的支持：

        @WebServlet("/LoginAction")
        public class LoginAction extends HttpServlet {
            @Override
            protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                super.doPost(req, resp);
            }
        }