# Android App Bundle

简称aab

    Android App Bundle 是一种发布格式，其中包含您应用的所有经过编译的代码和资源，它会将 APK 生成及签名交由 Google Play 来完成。
    Google Play 会使用您的 app bundle 针对每种设备配置生成并提供经过优化的 APK，因此只会下载特定设备所需的代码和资源来运行您的应用。


可以通过gradle的任务bundleXXX生成对应aab文件，也可以通过bundletool的命令行方式实现

aab是不能直接安装在手机设备上的，需要合成apk文件才可以安装

## 参考

[Build an app bundle](https://developer.android.com/studio/build/building-cmdline#build_bundle)

[Android App Bundle 使用方法](https://juejin.cn/post/6920880802074984455)