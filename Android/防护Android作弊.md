# 防护Android作弊


## 模拟定位防护

模拟定位有两种方式：

1、通过手机的开发者模式-选择模拟位置应用的方式来修改gps模拟定位

2、通过hook方式来修改系统级别api来欺骗定位sdk （如双开、Xposed）

第一种方式，只能欺骗gps定位，如果是wifi定位（网络定位）是不能被欺骗的，同时定位sdk是可以获知这个方式的

第二种方式：可以欺骗定位sdk，腾讯定位sdk、百度定位sdk、高德定位sdk都可以

    百度地图定位sdk有概率防护定位功能，这个很好


## 检测双开

1、动态so路径检测出包名，从而推测双开应用

    这种方式需要包名对比，当双开的应用修改包名后，就失效了，需要重新添加新包名

2、



## 系统安装软件检测

[Android禁止读取已安装列表后获取是否安装某应用的方法](https://www.jianshu.com/p/ee50b3e3bfc7)
