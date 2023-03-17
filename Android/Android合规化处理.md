# Android合规化处理

## C层代码动态调试风险检测  和 动态注入攻击风险检测

[Android应用防止so注入防止动态调试参考代码](https://blog.csdn.net/weixin_37459943/article/details/109315103?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-109315103-blog-70256529.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-109315103-blog-70256529.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1)

## 未使用编译器堆栈保护技术风险检测

[Android 未使用编译器堆栈保护技术解决方法](https://www.cnblogs.com/zhouyong0330/p/14277980.html)

## 3.3.15 未使用地址空间随机化技术风险检测

在使用NDK编译SO文件时，在Android.mk文件中增加以下参数LOCAL_CFLAGS := -fPIC

如果不是用Android.mk编译，用的build.gradle里面配置的ndk，可以这样配置

    ndk {
        cFlags "-fPIC"
    }


# Android合规化检测

[camille](https://github.com/zhengjim/camille)

[PrivacySentry](https://github.com/allenymt/PrivacySentry)
