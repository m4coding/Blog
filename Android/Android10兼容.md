# Android10兼容

1、外部存储文件访问相关


当targetSdk设置为29以上时，新安装的apk，当访问外部存储file.listFiles时就会出现null的情况，同时Android10开始应用将不可直接访问外部存储（/sdcard）文件，否则抛异常

为了处理这种情况有如下三种方案：

（1）继续使用旧版存储模式

    <manifest ... >
     <application android:requestLegacyExternalStorage="true" ... >
       ...
     </application>
   </manifest>

（2）将文件存储到自己应用的外部目录中

    // /Android/data/应用包名/files/Documents
   File dir = context.getExternalFilesDir(Environment.DIRECTORY_DOCUMENTS);

优点：不用申请读写权限

缺点：随应用卸载而删除


（3）使用存储访问框架（SAF），由用户指定要读写的文件

（4）使用MediaStore访问，不过只能访问媒体文件，如图片、音乐、视频等

## 参考

[Android10(Api 29)新特性适配小结](http://www.voycn.com/article/android10api-29xintexingshipeixiaojie)

[使用SAF框架的demo-ActionOpenDocumentTree](https://github.com/TranceLove/ActionOpenDocumentTree)

[DocumentFile的包装-DocumentFileX](https://github.com/tom5079/DocumentFileX)
