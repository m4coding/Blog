# Flutter

[Flutter中文网](https://flutterchina.club/)

[Flutter-gsy](https://guoshuyu.cn/home/wx/Flutter-1.html)


[Flutter混合开发(一)：Android项目集成Flutter模块详细指南](https://juejin.im/post/5dc4012df265da4d500f92a5)

[Flutter混合开发(二)：iOS项目集成Flutter模块详细指南](https://juejin.im/post/5dca941df265da4d1713909d)

[Flutter混合开发(三)：Android与Flutter之间通信详细指南](https://juejin.im/post/5dce51edf265da0c0c1fe649)

[一些flutter变动](https://juejin.im/post/5dc90f9a6fb9a04a7a05dc1e)

## 一些命令使用

    flutter doctor 检测环境配置


## 工程创建

基于android studio

需要支持flutter需要，先安装flutter插件和dart插件

开始创建flutter工程：

    File ->  New -> New Flutter Project...
    
    
## 一些坑

1、导入flutter module时出现：

    异常：提示flutter.grdale 681行出错

    原因是脚本查找名称为app的模块没找到导致异常。。。
     // Flutter module included as a subproject in add to app.
            Project appProject = project.rootProject.findProject(':app')
            assert appProject != null
            
            