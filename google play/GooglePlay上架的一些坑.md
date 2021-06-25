# Google Play上架的一些坑

1、创建App版本时，若启用了Play App Signing，那么google play会将你上传的应用重新签名，然后再发布出去。如果你的App里面有应用签名检验或者第三方应用需要对应的签名信息
那么就会出现问题。因为自己release打包添加的签名已被改变。。。可以在创建时管理应用签名，选择退出 Play App Signing 计划

    可以在google play Console的设置->应用完整性中看对应的签名信息

如果启动了Play App Signing计划，需要换回原来的签名，补救方法  google play Console的设置->应用完整性->升级应用签名密钥

[Android Google Play app signing 最终完美解决方式](vhttps://www.cnblogs.com/zhaoyanjun/p/12715125.html)