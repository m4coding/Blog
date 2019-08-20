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

[异常断网，推流端重连，提示“已经存在推流”的问题](http://www.yeegee.com/view/newsdetail?id=19)

    github地址：https://github.com/alfg/docker-nginx-rtmp

    以命令行的形式进入，注意是/bin/sh 而不是/bin/bash，因为 alfg/nginx-rtmp基于的linux系统是Alpine
    docker run -it  alfg/nginx-rtmp /bin/sh  
    
    1、当需要修改nginx.conf配置时，对于docker可以采用外挂的方式，如
    # 使用-v来指定替换alfg/nginx-rtmp系统中的/opt/nginx/nginx.conf
    docker run -it -p 1935:1935 -p 8080:80 -v E:\work\video_test\nginx\nginx.conf:/opt/nginx/nginx.conf  --rm alfg/nginx-rtmp
    
    2、查看nginx rtmp module的状态：
    由于alfg/nginx-rtmp配置了stat，所以可以查看其状态
    http://192.168.2.117:8080/stat  （指定8080端口，因为docker run运行时-p指定了8080:80）
    
    3、配置nginx-rtmp-monitoring，监控服务器状态
        #clone 仓库
        git clone https://github.com/fiftysoft/nginx-rtmp-monitoring.git
        
        # cd到nginx-rtmp-monitoring目录
        cd nginx-rtmp-monitoring
        
        # docker 编译容器 
        docker build -t nginx-rtmp-monitoring .
        
        # 运行容器  利用-v绑定自己定义的config.json
        docker run -it --rm -p 9991:9991 -v E:\work\video_test\nginx\config.json:/usr/src/app/config.json  nginx-rtmp-monitoring
        
        config.json如下：             
            {
              "site_title":"RTMP Monitoring",
              "http_server_port":9991,
              "rtmp_server_refresh":3000,
              "rtmp_server_timeout":15000,
              "rtmp_server_url":"http://192.168.2.117:8080/stat",
              "rtmp_server_stream_url":"rtmp://192.168.2.117:8080/stream/",
              "rtmp_server_control_url":"http://192.168.2.117:8080/control",
              "session_secret_key":"change_me_random",
              "username":"admin",
              "password":"123123",
              "language":"en",
              "template":"default",
              "login_template":"login",
              "version":"1.0.2"
            }
            
        说明：
        site_title 指定网页标题
        http_server_port 绑定的服务器端口  docker run -p指定的
        rtmp_server_refresh  多久时间刷新一次网页 单位：毫秒
        rtmp_server_timeout  超时  单位：毫秒
        rtmp_server_url  rtmp服务器地址  如果rtmp服务器也是用docker容器挂载的，需要注意端口问题
        rtmp_server_stream_url  rtmp对应的live应用地址
        rtmp_server_control_url  这个字段。。暂未知作用
        username 指定登录账户
        password 指定登录密码
        
        # 浏览器网页登录查看 
        http://192.168.2.117:9991/

### 推流延迟

    10秒级延时hls， 秒级延时rtmp， 毫秒级延时p2p
    
    一般市场上可用是rtmp 2-3秒左右
    
### 推流时延测试

    1、使用ffplay播放来进行测试时延
        由于播放器播放时一般会有缓存，ffplay播放后会有一定的延迟性，所以为了测试时延，减低播放器的影响，设置如下：
        //probesize设置探针大小，这个用于获取视频相关内容的，大小需要灵活设置 （我在测试时设置的probesize为3072，如果设置为32，视频会侦探不了，只能侦探音频）
        //probesize设置合适的话，也有利于快速打开播放页面 （也即是秒开）
        //-sync ext使用外部时钟来进行同步音视频播放
        ffplay -probesize 32 -sync ext rtmp://[server]/[application]/[stream]
        
        //第二种方法  不能秒开，但是可以播放时时延很低
        ffplay  -fflags nobuffer -rtmp_buffer 0 -rtmp_live live  rtmp://[server]/[application]/[stream]
        
     2、对视频添加帧标记，从而对比测试
        https://zhuanlan.zhihu.com/p/26800890   