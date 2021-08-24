# Admod广告接入

[在 AdMob 中设置应用](https://support.google.com/admob/answer/2773509)

[Android Admod广告sdk接入入门指南](https://developers.google.com/admob/android/quick-start?hl=zh-CN#import_the_mobile_ads_sdk)

[想要在等待期间使用示例广告单元进行测试？](https://developers.google.com/admob/android/test-ads)


## 注意

1、新广告单元可能需要一小时的时间才能开始展示广告  （也即所谓的等待时间）

2、应用id和广告id是有区别的，应用id在AndroidManifest中注册使用，而广告id是加载广告时使用

3、测试时，可以使用测试广告id，发布时才用正式的广告id

4、由于广告需要认证资源，配置app-ads.txt，对于个人开发者由于没有属于自己的开发者网站时，可以借助firebase来部署自己的网站，同时上传app-ads.txt

[使用 Firebase 托管发布 app-ads.txt](https://support.google.com/admob/answer/9820295?hl=zh-Hans)

[Authorized Sellers for Apps (app-ads.txt) ](https://developers.google.com/admob/android/app-ads#firebase)

[https://firebase.google.com/docs/hosting/](https://firebase.google.com/docs/hosting/)

配置步骤：

    （1）首先先在google play管理中心配置开发者网站地址： 商店发布->商店设置->网站
    （2）从Admod网站中复制信息，然后复制生成app-ads.txt文件
    （3）在自己的网站的根目录上传app-ads.txt（通过firebase deploy --only hosting 进行部署），例如我的网站为https://test.com, 那么对应app-ads.txt下载地址应该为https://test.com/app-ads.txt
    （4）配置好文件后，Admod就会拉取https://test.com/app-ads.txt文件进行认证
     