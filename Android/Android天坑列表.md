# Android天坑列表

1、android 8.0系统透明Activity设置方向后，出现闪退问题

[修复Android8.0系统BUG导致透明度+转向崩溃](https://www.jianshu.com/p/226754ee33cb)

[安卓8.0，设置activity背景透明且设置方向，引发Only fullscreen opaque activities can request orientation问题](https://blog.csdn.net/hzqaqz/article/details/125368607)

2、7.0以上，外部文件分享处理问题

    E/DatabaseUtils(22545): Writing exception to parcel
    E/DatabaseUtils(22545): java.lang.SecurityException: Permission Denial: reading androidx.core.content.FileProvider uri content://com.example.app.provider/external_files/Android/data/com.example.app/files/test.json from pid=22857, uid=1000 requires the provider be exported, or grantUriPermission()
    E/DatabaseUtils(22545): 	at android.content.ContentProvider.enforceReadPermissionInner(ContentProvider.java:729)
    E/DatabaseUtils(22545): 	at android.content.ContentProvider$Transport.enforceReadPermission(ContentProvider.java:602)
    E/DatabaseUtils(22545): 	at android.content.ContentProvider$Transport.query(ContentProvider.java:231)
    E/DatabaseUtils(22545): 	at android.content.ContentProviderNative.onTransact(ContentProviderNative.java:104)
    E/DatabaseUtils(22545): 	at android.os.Binder.execTransactInternal(Binder.java:1021)
    E/DatabaseUtils(22545): 	at android.os.Binder.execTransact(Binder.java:994)
  
注意下是否已给权限 Intent.FLAG_GRANT_READ_URI_PERMISSION和Intent.FLAG_GRANT_WRITE_URI_PERMISSION
