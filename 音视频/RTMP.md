## RTMP

    Real Time Messaging Protocol 实时消息传输协议
    
    变种协议：
    1、默认使用1935的协议
    2、RTMPS：通过一个TLS/SSL连接传输的RTMP
    3、RTMPE：使用Adobe自有安全机制加密的RTMP
    4、RTMPT：用HTTP封装以穿透防火墙。RTMPT通常在TCP通信端口80和443上使用明文请求来绕过大多数的公司流量过滤。封装的会话中可能携带纯粹的RTMP、RTMPS或RTMPE数据包。
    5、RTMFP：使用UDP而非TCP的RTMP
    
        RTMPE = RTMP + ENC，已加密的RTMP
        RTMPS = RTMP + SSL，通过SSL传输层传输RTMP
        RTMPT = RTMP + HTTP，在HTTP协议中传输RTMP
        RTMPTE = RTMP + HTTP + ENC，在HTTP协议中传输已加密RTMP
        RTMPTS = RTMP + HTTP + SSL，通过SSL传输层在HTTP协议中传输RTMP
    
    
### 主流的RTMP库

    1、librtmp
    
    
    2、srs_librtmp
    
        srs （Simple RTMP Server）中的客户端例子
        
        代码量少，可导出成一个代码文件
        
        
### 推流端方案选择

    1、librtmp结合ffmpeg，ffmpeg具备强大功能，开发起来比较容易，但是具体库的细节不好控制
    
    2、x264 + libfaac + librtmp方案，采集、控制，时间戳等都要自己实现，难度比较大，但是灵活度高
    
    3、x264 + libfaac + srs_librtmp方案，在第二种基础上换用rtmp库，实现难度类似第二种，srs_librtmp库就接口简洁点，代码少
    
### RTMP直播流服务器搭建

[利用docker搭建RTMP直播流服务器实现直播](https://blog.csdn.net/lipei1220/article/details/80234281)

### 推流延迟

    10秒级延时hls， 秒级延时rtmp， 毫秒级延时p2p
    
    一般市场上可用是rtmp 2-3秒左右