### JSP

    全称为Java Server Pages 
    是由 Sun Microsystems 公司倡导和许多公司参与共同创建的一种使软件开发者可以响应客户端请求，而动态生成 HTML、XML 或其他格式文档的Web网页的技术标准
    
    后缀名为：.jsp
    
    

JSP处理流程：

    客户端浏览器  --(1)请求-->  
                            服务器 （具有JSP容器） 
                                              ----> （2）读取对应的jsp文件 
                                                ----> （3）生成Java文件 
                                                 ----> （4）编译生成class文件
     （6）返回结果给客户端<-------------------- （5）执行  <----
     
     

语法：

    1、脚本程序、代码片段
    
        <% 代码片段 %>
        
    2、声明
    
        一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用。在JSP文件中，您必须先声明这些变量和方法然后才能使用它们
        
        <%! int i = 0; %> 
        <%! int a, b, c; %> 
        <%! Circle a = new Circle(2.0); %> 
        
    3、表达式
    
        <%= 表达式 %>
        
        
        <%@ page language="java" contentType="text/html; charset=UTF-8"
            pageEncoding="UTF-8"%>
        <!DOCTYPE html>
        <html>
        <head>
        <meta charset="utf-8">
        <title>菜鸟教程(runoob.com)</title>
        </head>
        <body>
        <p>
           今天的日期是: <%= (new java.util.Date()).toLocaleString()%>
        </p>
        </body> 
        </html> 
        
    4、注释
    
        <%-- 注释 --%>  JSP注释，注释内容不会被发送至浏览器甚至不会被编译
        
        <!-- 注释 --> HTML注释，通过浏览器查看网页源代码时可以看见注释内容
        
    5、指令
    
        JSP指令用来设置与整个JSP页面相关的属性。
        
        <%@ page ... %>	定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等
        <%@ include ... %>	包含其他文件
        <%@ taglib ... %>	引入标签库的定义，可以是自定义标签
        
    6、行为
    
        jsp:include	用于在当前页面中包含静态或动态资源
        jsp:useBean	寻找和初始化一个JavaBean组件
        jsp:setProperty	设置 JavaBean组件的值
        jsp:getProperty	将 JavaBean组件的值插入到 output中
        jsp:forward	从一个JSP文件向另一个文件传递一个包含用户请求的request对象
        jsp:plugin	用于在生成的HTML页面中包含Applet和JavaBean对象
        jsp:element	动态创建一个XML元素
        jsp:attribute	定义动态创建的XML元素的属性
        jsp:body	定义动态创建的XML元素的主体
        jsp:text	用于封装模板数据
        
    7、隐含对象
    
        request	HttpServletRequest类的实例
        response	HttpServletResponse类的实例
        out	PrintWriter类的实例，用于把结果输出至网页上
        session	HttpSession类的实例
        application	ServletContext类的实例，与应用上下文有关
        config	ServletConfig类的实例
        pageContext	PageContext类的实例，提供对JSP页面所有对象以及命名空间的访问
        page	类似于Java类中的this关键字
        Exception	Exception类的对象，代表发生错误的JSP页面中对应的异常对象
        
#### XMLHttpRequest使用参数为中文乱码问题

[XMLHttpRequest 传递中文 乱码](https://blog.csdn.net/javaee_sunny/article/details/53129635)

[XMLHttpRequest对象解决中文乱码问题](https://blog.csdn.net/zzh920625/article/details/50468789)

    使用js的encodeURI两次编码避免Servlet服务器接收参数中文乱码问题
    
#### dom4j 输出UTF-8 XML时中文乱码

    OutputFormat format = OutputFormat.createPrettyPrint();   
    format.setEncoding("utf-8");  
    try {   
        output = new XMLWriter(new FileOutputStream("entity.xml"), format);   
        output.write(document);   
        output.close();   
    } catch (IOException e) {   
        e.printStackTrace();   
    } 
    
    使用FileOutputStream替换FileWriter，避免utf-8时中文写入乱码