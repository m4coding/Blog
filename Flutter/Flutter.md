# Flutter

[Flutter中文网](https://flutterchina.club/)

[Flutter-gsy](https://guoshuyu.cn/home/wx/Flutter-1.html)


[Flutter混合开发(一)：Android项目集成Flutter模块详细指南](https://juejin.im/post/5dc4012df265da4d500f92a5)

[Flutter混合开发(二)：iOS项目集成Flutter模块详细指南](https://juejin.im/post/5dca941df265da4d1713909d)

[Flutter混合开发(三)：Android与Flutter之间通信详细指南](https://juejin.im/post/5dce51edf265da0c0c1fe649)

[一些flutter变动](https://juejin.im/post/5dc90f9a6fb9a04a7a05dc1e)

[一张图理解Flutter中Dart与原生环境通信](https://blog.csdn.net/joye123/article/details/100562767)

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


3、flutter默认是忽略状态栏的高度的

[flutter入门之兼容Android和IOS的状态栏](https://blog.csdn.net/email_jade/article/details/85701437)

4、 

[Flutter : ListView, GridView inside ScrollView](https://medium.com/flutterpub/flutter-listview-gridview-inside-scrollview-68b722ae89d4)

5、

[flutter GestureDetector的onTap事件无效，原因?](https://blog.csdn.net/qq_43245438/article/details/93657854)

6、在CustomScrollView中使用StaggeredGridView 

    StaggeredGridView用在CustomScrollView中，会报异常，应该使用SliverStaggeredGrid来代替实现瀑布流，但是有个问题，在SliverStaggeredGrid后面再放置其他GridView或者ListView会出现滚动异常。。。
    将SliverStaggeredGrid放在最后面就不会有问题

7、为了使页面在PageView中初始化后，切换到其他页面，再切回来不重新初始化，可以使用AutomaticKeepAliveClientMixin

8、使用android studio开发flutter时，忽然出现target of uri doesn't exist，编辑器出现一片红色。。。一些类表示找不到，但其实这个库之前都导入正确的，解决这个问题可以在命令行Terminal执行1、flutter clean, 2、flutter packages get，重新导入包，正确找到依赖

## flutter的两种运行模式

    1、以module的方式运行调试
    
    2、以一个工程的方式去打开module来进行调试
    
            
## flutter断点调试

    通过as的Flutter Attach按钮或者命令行flutter attach来进行断点调试或者hot reload
    
    当没在flutter页面时，会一直提示Waiting for a connection from Flutter on XXX
    进入到flutter页面时会done成功，若还是不成功，那么重新杀掉app再来一次就会成功了。。
    

## flutter web开发

    通过flutter channel可以看到相关的flutter渠道，默认渠道是stable稳定渠道下，由于1.9.1版本的sdk
    稳定版还没有支持web，而在beta版本是支持的，所以需要切换到channel为beta时才能支持开发web
    
    flutter channel beta  即可以切换到beta渠道
    
    flutter config --enable-web  执行命令，打开web支持
    
    对于已有的flutter app工程，可以直接在其工程目录下执行flutter create . 即可生成web相关支持的目录
    
## 切换升级flutter

    当需要切换flutter版本时，现在android studio setting中找到flutter选项，然后设置切换，删除掉.package文件，如果还不行就还要设置修改系统的flutter path命令路径
    
[Flutter for web 最新填坑](https://juejin.im/post/5d7de304e51d45620c1c5460#heading-0)

## flutter下拉刷新

[Flutter 下拉刷新花式玩法](https://juejin.im/post/5bebcc44f265da61682aedb8)

[Flutter嵌套刷新填坑](http://blog.hacktons.cn/2019/11/25/refresh-with-nestscrollview/)


## flutter json序列化与反序列化

[Flutter Json自动反序列化](https://juejin.im/post/5b5f00e7e51d45190571172f)

    注意要添加part部分，否则build_runner会生成异常，提示miss part部分

自动生成FromJson和ToJson方法的一些问题解决：

    当自定义的Bean的字段是另一个B Bean时，import B bean时的需要相对路径，如果使用绝对路径就会生成异常，导致运行的时候不能正确解析json
    例子：
    
    import '../b/B.dart'; //正确的导包
    import 'package:test/b/B.dart'; //绝对路径导包，自动反序列化会异常。。。
    ///包路径：a/A.dart
    @JsonSerializable()
    class A {
        List<B> bList;
    }
    
    ///包路径：b/B.dart
    @JsonSerializable()
    class B {
        int num;
    }

    当使用import 'package:test/b/B.dart';导包时，app运行时会报
    type 'List<dynamic>' is not a subtype of type 'List<B>'
    异常。。。。。
    
    使用import '../b/B.dart'; 导包时，运行正确
         生成的正确代码段
        ..bList = (json['bList'] as List)
        ?.map((e) => e == null
            ? null
            : B.fromJson(e as Map<String, dynamic>))
        ?.toList();
    
    如果只能使用绝对路径导包的话，那就只能使用@JsonKey的fromJson，指定解析方法来实现了。。。    

## Flutter插件开发

    buildscript {
        repositories {
            google()
            jcenter()
        }
    
        dependencies {
            classpath 'com.android.tools.build:gradle:3.3.2'
        }
    }
    
    引入aar包后，发现基于3.5.3版本的工具，在flutter工程编译后会有Manifest重复的异常，改用3.3.2就正常了。。
    而直接打开example的android工程编译是没有问题的。。打开flutter工程有异常。  （example工程的gradle）
    
    * What went wrong:
    Execution failed for task ':ysbang_flutter_plugin:mergeDebugJavaResource'.
    > A failure occurred while executing com.android.build.gradle.internal.tasks.Workers$ActionFacade
       > More than one file was found with OS independent path 'AndroidManifest.xml'
       
       
## 页面之间传参和返回数据

[Flutter页面跳转和传值传参，接收页面返回数据、以及解决返回（pop）页面时黑屏的问题](https://blog.csdn.net/yuzhiqiang_1993/article/details/89090742)


## 在Flutter的pubspec.yaml中依赖版本号之前的插入符号（^）是什么？

[在Flutter的pubspec.yaml中依赖版本号之前的插入符号（^）是什么？](https://stackoom.com/question/3ckCd/%E5%9C%A8Flutter%E7%9A%84pubspec-yaml%E4%B8%AD%E4%BE%9D%E8%B5%96%E7%89%88%E6%9C%AC%E5%8F%B7%E4%B9%8B%E5%89%8D%E7%9A%84%E6%8F%92%E5%85%A5%E7%AC%A6%E5%8F%B7-%E6%98%AF%E4%BB%80%E4%B9%88)

## ListView的子View有Column引起的异常，特别是当Column中有Expand时

[Flutter踩坑：ListView布局报错，RenderFlex children have non-zero flex but incoming height constraints are...](https://blog.csdn.net/kaixuan_dashen/article/details/102308861)

## Image.file和Image.asset的一些坑

    Image.file对应加载本地图片，如手机存储上的文件 
    Image.asset对应加载app包里面的资源图片
    这里需要注意：v1.12.13+hotfix.5 版本的flutter，在android debug包时Image.asset可以加载本地图片。。。，Image.asset在android release包不可以加载本地图片
    需要特别注意。。。。


## 编译engine引擎代码

[搭建Flutter Engine源码编译环境](http://gityuan.com/2019/08/03/flutter_engine_setup/)

整个执行的编译命令流程：

    ./flutter/tools/gn  --no-lto  --runtime-mode profile
    ./flutter/tools/gn --android --runtime-mode profile

    ninja -C out/host_profile -j 4 
    ninja -C out/android_profile -j 4


代码流程调用梳理：

    library_loader.cc
        flutter_main.cc
        platform_view_android.cc
            platform_view_android_jni.cc
                android_shell_holder.cc
                     platform_view_android = std::make_unique<PlatformViewAndroid>();   
    
    java层
       FlutterJNI.java                            


编译过程中遇到问题：

1、_ld: reference to bitcode symbol '___gxx_personality_v0' which LTO has not compiled in '__gxx_personality_v0' for architecture x86_64

./flutter/tools/gn --no-lto --runtime-mode=profile

添加--no-lto编译选项即可

[Flutter engine host release build fails on macos ](https://github.com/flutter/flutter/issues/57207)

2、编译android的产物，需要配置java sdk，可以使用android studio里面自带的

    export PATH=$PATH:/Applications/Android\ Studio.app/Contents/jre/jdk/Contents/Home/bin
    export JAVA_HOME=/Applications/Android\ Studio.app/Contents/jre/jdk/Contents/Home

