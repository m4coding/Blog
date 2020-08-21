# 阿里云移动端推送Sdk


## 接入

1、使用官方提供maven仓库接入即可

2、在AndroidManifest中接入appKey，appSecret，当然也可以选择EMAS统一接入方式（基于gradle插件接入）

3、创建消息接收Receiver，继承自com.alibaba.sdk.android.push.MessageReceiver，并在对应回调中添加业务处理逻辑

    当然也可以尝试继承AliyunMessageIntentService，实现IntentService,为避免推送广播被系统拦截的小概率事件 (V3.0.10及以上版本支持)

4、辅助通道接入，为了能让app应用在杀死的情况下也能收到对应的系统消息，需要用到辅助通道弹窗 -- 也即是对接手机系统的推送sdk

    首先在阿里云推送控制台录入app对应各个手机平台的key，这个很重要，同时再看app启动时的初始化的打印是否正确

     “在对应的手机开发者平台上注册好应用，同时获取对应的配置写入成功后，有可能需要等一段时间后，才能初始化成功”

    对于厂家通道，需要配置辅助弹窗推送内容：

      1、通知点击后跳转activity  （例如：com.alibaba.cloudpushdemo.bizactivity.ThirdPushPopupActivity）
      2、辅助弹窗title内容
      3、辅助弹窗body内容
      4、通知通道要填对 （8.0以上系统需要 -- 创建NotificationManager时指定的id）

    三项一定要填，缺一项就不会有推送通知

    --app在被杀死的情况下，走厂家通道的通知栏消息显示的标题是辅助弹窗的title内容，内容显示的是辅助弹窗的body内容 （点击会跳转到ThirdPushPopupActivity）

    --app在存活的情况下，是不走厂家通道的，所以通知栏消息显示的标题是“消息的通知标题”，内容显示的是“消息的通知内容”，
    同时MessageReceiver的子类回调或者AliyunMessageIntentService的子类回调（看注册方式）


华为：

    <meta-data
        android:name="com.huawei.hms.client.appid"
        android:value="xxxx" /><!-- 请填写你自己华为平台上注册的appid -->

    android:value对应的值不要appid，如"appid=xxxx" demo的坑。。


## 注意项

[Android 8.0以上设备接收不到推送通知](https://help.aliyun.com/knowledge_detail/67398.html)

    自Android 8.0（API Level 26）起，Android推出了NotificationChannel机制，旨在对通知进行分类管理。如果用户App的targetSdkVersion大于等于26，且并未设置NotificaitonChannel，那么创建的通知是不会弹出显示。


[Android 9及以上系统初始化失败出现“errorCode：10109”报错](https://help.aliyun.com/knowledge_detail/141757.html)

    Android 9及以上系统，默认只允许使用HTTPS请求，但是目前SDK中使用的是HTTP请求，所以导致初始化请求失败。