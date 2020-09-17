# 基于appium抓包实现

Appium是一个用于本地、混合和移动web应用程序的开源测试自动化框架。它通过WebDriver协议驱动iOS、Android和Windows应用程序。

## VirtualExpose + JustTrustMe

如果需要在android7.0以上的系统手机app抓包，需要通过VirtualExpose和JustTrustMe3.0来hook绕过证书认证，实现中间人证书抓包https

## 安装配置

1、安装appium服务端

服务端支持三大系统平台:window、mac、linux
    
[服务端下载地址](https://github.com/appium/appium-desktop/releases/latest)    
    
（1）window安装方式：

直接下载exe文件，安装即可

同时需要配置ANDROID_HOME和JAVA_HOME系统环境变量


Inspector--可视化操作

检查器是应用程序状态的可视化表示，并且能够通过Appium在应用程序中执行某些交互。

启动Inspector的步骤是先启动服务端页面到终端页面，然后再启动一个“启动检查器会话”，通知配置好platformName就可以


2、appium客户端库

appium客户端支持Ruby、Python、Java、JavaScript、PHP、C#、RobotFramework等语言

[Ruby](http://rubygems.org/gems/appium_lib)

[Python](https://pypi.python.org/pypi/Appium-Python-Client)

[Java](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22io.appium%22%20AND%20a%3A%22java-client%22)

[JavaScript](https://www.npmjs.org/package/wd)

[PHP](https://github.com/appium/php-client)

[C#](https://www.nuget.org/packages/Appium.WebDriver/)

[RobotFramework](https://github.com/jollychang/robotframework-appiumlibrary)

## Android端操作

### 一些adb操作

（1）查看当前操作的是哪个Activity

adb shell dumpsys window windows | grep mFocusedApp


## 中间人抓包配置

选用mitmproxy框架来进行抓包中间人配置

（1）安装

pip3 install mitmproxy

需要注意的是mitmproxy在window系统下是不能使用的，可以使用mitmdump来代替

例子：

    mitmdump -p 8877 -s main.py
    代理监听8877端口，同时执行python处理脚本main.py

默认情况下，只能抓取http的包，如果想要抓取https的包，需要安装证书才行

已连接的代理客户端，访问http://mitm.it/，安装对应的证书即可

## protobuf反序列化解析

序列化和反序列都需要.proto文件，当然也可以通过protoc命令来进行反序列化获取解码后的数据，但为了转bean，还是需要.proto文件的，这个时候，就需要逆写.proto文件

[Protobuf协议逆向解析-APP爬虫](https://www.yuanrenxue.com/app-crawl/parse-protobuf.html)

## MySQL数据库

1、安装

[windows10安装mysql-8.0.21](https://www.cnblogs.com/sharry/p/13414455.html)

通过msi包安装的方式，出现了error 1042：Unable to connect to any of the specified MySQL hosts的问题

很奇怪，百度了一些解决方法也没有用，只能通过zip包的方式安装。。。即可成功

2、启动MySQL服务

net start mysql8  #mysql8为创建的服务名称

[python 数据库orm框架 Peewee 使用](https://www.jianshu.com/p/8d1bdd7f4ff5)

## 参考文档

[appium官方文档](http://appium.io/docs/cn/about-appium/intro/)

[APP爬虫-mitmproxy安装与简单使用](https://cloud.tencent.com/developer/article/1631554)