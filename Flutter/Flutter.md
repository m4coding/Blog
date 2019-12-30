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
            
            
            
2、通过android studio创建的flutter module，导入时出现的问题解决

    由于android studio创建的flutter module，相关目录都是带.的，即表示是隐藏文件夹，如.android, .ios等目录
    通过git版本管理时这些隐藏文件夹是不会版本管理到仓库的，这个时候如果从其他地方git clone导入工程，那么就会
    提示不存在.android目录中的include_flutter.groovy文件。。
    
    为了重新生成这些隐藏目录，需要重新以project的方式导入这个flutter module以去生成隐藏目录 (导入方式是as的import project导入那个flutter module)
    
    生成后再到原生工程修改settings.gradle导入
    如例子：
    
        setBinding(new Binding([gradle: this]))
        evaluate(new File(
          settingsDir,
          'flutter_module/.android/include_flutter.groovy'
        ))
        rootProject.name='TestFlutterApplication'
        
        include ':flutter_module'
        
    最后app模块依赖模块即可
        implementation project(path: ':flutter')


## flutter的两种运行模式

    1、以module的方式运行调试
    
    2、以一个工程的方式去打开module来进行调试
    
            
            