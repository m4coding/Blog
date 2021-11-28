# Google Play上架的一些坑

1、创建App版本时，若启用了Play App Signing，那么google play会将你上传的应用重新签名，然后再发布出去。如果你的App里面有应用签名检验或者第三方应用需要对应的签名信息
那么就会出现问题。因为自己release打包添加的签名已被改变。。。可以在创建时管理应用签名，选择退出 Play App Signing 计划

    可以在google play Console的设置->应用完整性中看对应的签名信息

如果启动了Play App Signing计划，需要换回原来的签名，补救方法  google play Console的设置->应用完整性->升级应用签名密钥

[Android Google Play app signing 最终完美解决方式](vhttps://www.cnblogs.com/zhaoyanjun/p/12715125.html)


2、Google Play Console选项中的App Bundle探索器可以查看和下载已上传的apk

3、android app bundle的一些坑

    （1）如果业务中利用到了so的路径，并进行对应的复制，那么会有个坑，之前的apk包是会将so解压好放在/data/app/app包名-数字的目录下的
        而aab合成的apk是不会解压生成的，会直接压缩放到/data/app/app包名-数字/split_config.arm64_v8a.apk中，此时是不能直接取的。。
        解决方法是在gradle.properties文件中添加android.bundle.enableUncompressedNativeLibs=false，关闭这个配置即可

## 参考

[Google Play crash logs not symbolicated with Android App Bundle](https://stackoverflow.com/questions/55966582/google-play-crash-logs-not-symbolicated-with-android-app-bundle#)