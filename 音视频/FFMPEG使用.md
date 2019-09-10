# FFMPEG使用

## 编译

1、添加x264

[x264各个版本下载地址](https://download.videolan.org/pub/videolan/x264/snapshots/)

    ffmpeg自带有h264解码，如果要添加编码功能，需要添加x264
    
    编译过程中可能会出现如下问题：
       （1） 
        libavcodec/libx264.c: In function 'x264_frame' :
        libavcodec/libx264.c:282:9 error: 'x264_bit_depth' undeclared(first use in this function)  
                     if(x264_bit_depth>8)     
        
        libavcodec/libx264.c: In function 'x264_init_static':
        libavcodec/libx264.c:892.9 error: 'x264_bit_depth' undeclared(first use in this function)
                     if(x264_bit_depth== 8)  
                     
        这个是用的x264库版本，ffmpeg不兼容。需要换对应版本的x264库
        
2、添加fdk_aac库

    使用fdk_aac是因为性能比ffmpeg内置的aac性能好
    
    
3、添加librtmp库

    ffmpeg本身是自带rtmp功能的，但是功能并不强大，只支持rtmp协议，并不支持其变种协议，如：rtmpt,rtmpe,rtmpte和rtmps等。
    所以考虑添加librtmp第三方库
        
[librtmp库下载地址](https://rtmpdump.mplayerhq.hu/)

    编译过程出现的问题：
    （1）ERROR: librtmp not found using pkg-config
        由于ffmpeg的configure在配置过程中是通过pkg-cofig查看librtmp的所以就出现这个错误
        解决方法：（对于其他类库，也适用）
        **第一种方法：**
            将enabled librtmp    && require_pkg_config librtmp librtmp librtmp/rtmp.h RTMP_Socket这行注释掉
        
        **第二种方法：**
            由于ndk的交叉编译工具链没有arm-linux-androideabi-pkg-config，所以需要引用系统本身的
            
             # 创建一个软引用，指向系统的pkg-config  (有些系统并没有安装pkg-config，需要安装)
             ln -s /usr/bin/pkg-config  工具链所在目录/arm-linux-androideabi-pkg-config
             # 更改权限
             chmod 777 工具链所在目录/arm-linux-androideabi-pkg-config
             
             # 在编译脚本，添加librtmp库pkgconfig目录所在路径
             export PKG_CONFIG_PATH=librtmp库的pkgconfig目录路径:$PKG_CONFIG_PATH
    
4、添加libfreetype库

    为了使用drawtext滤镜，需要这个库
    
    编译过程中出现的问题：
    （1）/system/bin/linker: No such file or directory  或 找不到C编译器（编译源码主机的）
    发现编译过程中，freetype/objs/apinames命令会执行，而apinames却是要编译架构的执行文件，却在编译源码的主机上执行，所以就会报错
    解决方法：（第一种方法）安装gcc编译工具（编译源码主机的）。。由于我用的是docker系统，并没有安装gcc工具，编译报错了。。
    （第二种方法）编译脚本添加export CC_BUILD="/usr/bin/gcc"，为CC_BUILD指定gcc路径
    
    所以引发上面一系列问题的原因是编译源码主机系统没有安装gcc编译器导致的。。。
    
    --enable-libfontconfig  如果默认不开启，如果不开启这个选项drawtext时需要指定字体文件
    --enable-libfribidi 这个选项用于字体变形
    
5、添加libfontconfig库

    （1）编译这个库比较坑，是由于里面用了python访问网络内容，需要代理访问才能编译成功。。利用proxychains即可

6、添加硬编码--android内嵌的MediaCodec

    由于3.4版本的ffmpeg只支持MediaCodec解码，若要支持编码需要外部添加
    
    configure配置添加--target-os=android和--enable-jni，针对于android平台编译，同时添加jni支持
    
    代码添加:
        //在JNI_OnLoad入口处设置javaVm
        av_jni_set_java_vm(vm, nullptr);
        
        //定义AVCodec结构，注册MediaCodec相关的编码信息
        static AVCodec ff_h264_mediacodec_encoder = {
                .name           = MEDIA_CODEC_H264_ENCODER_NAME,
                .long_name      = MEDIA_CODEC_H264_ENCODER_NAME,
                .type           = AVMEDIA_TYPE_VIDEO,
                .id             = AV_CODEC_ID_H264,
                .priv_data_size   = sizeof(MediaCodecH264EncContext),
                .init           = mediacodec_encode_init,
                .encode2        = mediacodec_encode_encodeFrame,
                .close          = mediacodec_encode_close,
        };
        
        mediacodec_encode_init指定MediaCodec编码初始化函数入口
        mediacodec_encode_encodeFrame指定MediaCodec编码函数入口
        mediacodec_encode_close指定MediaCodec编码资源释放函数入口
        
        在要使用的时候，需要先注册下才行 avcodec_register(&ff_h264_mediacodec_encoder);
        
        然后就可以，使用了。。。。
            
[Android平台MediaCodec避坑指北 ](https://github.com/Denislyl/MyTechBlog2/issues/6)

    
## 命令行使用

     //播放裸的aac文件，-ar指定采样率  -ac指定通道数
    ffplay -ar 44100 -ac 1 E:\test.aac  
    
    //播放pcm文件  -ar指定采样率  -ac指定通道数 -f 指定采样格式
    ffplay -ar 44100 -ac 1 -f s16le E:\test.pcm
    
    //推流
    ffmpeg -re -i E:work\database\good-48656992.1.mp4 -c copy -f flv rtmp://192.168.2.117:1935/stream/example
    
    ./ffmpeg -re -i /root/test.mp4 -c:v libx264 -c:a copy -vf "drawtext=text='hello':x=100:y=100:fontsize=24:fontcolor=yellow:shadowy=2" -f flv rtmp://192.168.2.117:1935/stream/example
    
    //docker 启动推流服务器
    docker run -it -p 1935:1935 -p 8080:80 alfg/nginx-rtmp
    
## 使用

1、使用libyuv来代替ffmpeg的颜色空间转换，如RGB转YUV或YUV转RGB等，因为libyuv的转换效率比ffmpeg高

2、使用ffmpeg进行muxer时，分配AVFormatContext使用avformat_alloc_output_context2方法

     指定封装格式为mp4时-->
        默认的视频编码器为：AV_CODEC_ID_H264
        默认的音频编码器为：AV_CODEC_ID_AAC
        
     指定封装格式为avi时-->
            默认的视频编码器为：AV_CODEC_ID_MPEG4
            默认的音频编码器为：AV_CODEC_ID_MP3
                
     指定封装格式为flv时-->
            默认的视频编码器为：AV_CODEC_ID_FLV1
            默认的音频编码器为：AV_CODEC_ID_MP3
            
3、添加水印

    1、文字水印可使用drawtext
    例如：
    drawtext=fontsize=30:fontfile=字体绝对路径:text='%{n}':x=20:y=20:fontcolor=0xFFFF00@0.5
    
        fontsize指定字体大小
        fontfile指定字体文件路径
        text指定要显示的文字  （也可以用textfile，用外部文件的内容来指定，不过这个项不能和text同时用）
        x指定距离屏幕左方的像素值
        y指定距离屏幕上方的像素值
        fontcolor指定颜色值   @后面表示的是不透明度，值范围为0.0~1.0   0xFFFF00依次表示的是RGB的值
    
    2、图片水印可使用movie
    movie=图片绝对路径,scale=223:50[watermask];[in][watermask] overlay=1:300[out]
    
        scale表示将图片宽缩放到223，高缩放到50 
        [watermask]表示的是指定一个名称
        overlay=1:300表示的是图片水印显示的位置，1表示的距离屏幕左方的像素值为1,300表示的是距离屏幕顶部的像素值为300

## 开发注意项

（1）在C++中使用ffmpeg时，当引入头文件时需要使用extern "C"{ }包括，否则会出现编译异常

    例如：
    error: undefined reference to 'av_register_all()'
    error: undefined reference to 'avcodec_register_all()'
    error: undefined reference to 'avformat_network_init()'
    
    包括使用如下：
    extern "C" {
    #include "libavcodec/avcodec.h"
    #include "libavformat/avformat.h"
    #include "libavutil/avutil.h"
    #include "libswscale/swscale.h"
    }
    
（2） A/libc: Fatal signal 4 (SIGILL), code 0, fault addr 0x46e2 in tid 18298 (oftVideoEncoder), pid 18146 (oding.coolvideo)
  
    由于在添加x264时出现了这个问题，一直以为是编译x264不正确导致的，因为SIGILL表示的是非法指令，所以耗费了大量时间多次编译x264。
    然而却不是这个导致的。。。
    
    后来通过android studio打包编译信息查看相关的警告信息，发现有如下警告：
    warning: control may reach end of non-void function [-Wreturn-type]
    是因为初始化函数漏了添加返回值，编译器出了警告，但是编译没报错，而运行后就报SIGILL异常
    
    添加返回值后，就运行正确了。
    
    所以程序运行出错后，看看编译警告信息可能是个办法。。。
    
 （3）使用av_frame_get_buffer问题
 
    av_frame_get_buffer(frame, 8); 对齐为8，分配AVFframe正常，yuv420编码h264正常
    若对齐为32或0，就会编码异常，导致编码出来视频花屏，很奇怪。。。。
    
    发现对齐为8后，720p视频编码没问题，但是换为其他的分辨率视频之后就还是会花屏。。android系统有这个问题，window和mac则没有这个问题
    所以还是继续用旧版接口可靠点。。
    
    //创建一个AvFrame
    mFrame = av_frame_alloc();
    if (!mFrame) {
        LOGE_TAG(LOG_TAG, "%s", "av_frame_alloc could not allocate video frame");
        return -1;
    }
    int pictureInYUV420PSize = avpicture_get_size(mCodecContext->pix_fmt, mCodecContext->width, mCodecContext->height);
    mPictureBuf = (uint8_t *) av_malloc(pictureInYUV420PSize);
    avpicture_fill((AVPicture *) mFrame, mPictureBuf, mCodecContext->pix_fmt, mCodecContext->width, mCodecContext->height);
    
    对于音频，对齐为0，是可以的
    
  （4）aac编码时的注意点
  
    在ffmpeg3.4中，对于内置的aac的编码，pcm采样格式（sample_fmt）为AV_SAMPLE_FMT_S16的，是不支持的，需要
    先通过swr_convert转换为AV_SAMPLE_FMT_FLTP,才能进行编码
    
    而第三方库fdk-aac支持的编码，是可以AV_SAMPLE_FMT_S16支持编码，不需要转换
    
    音频帧的概念：
    
        一个音频帧的大小由编码格式、通道数、采样位数决定的。
        例如aac编码，一个音频帧包含1024个采样，若通道数为2，采样位数16位，那么一帧音频帧大小为：
        1024 * 2 * （16/8） = 1024 * 2 * 2 = 4096个字节
    
        此时编码一帧音频帧，应该送4096个字节到ffmpeg编码器，如果送入过多，那么就会导致编码出来后的音频播放速度加快
        
   （5）x264编码时的注意点：
   
    为了提高编码速度，可以针对CodecContext中priv_data的各值进行设置。
    例如：
        av_opt_set(mCodecContext->priv_data, "preset", "ultrafast", 0);
        av_opt_set(mCodecContext->priv_data, "tune", "zerolatency", 0);
        av_opt_set(mCodecContext->priv_data, "profile", "baseline", 0);
        
        这些设置要在avcodec_open2之前设置才会有效果，否则无效，注意！！！！！
        
    preset参数：主要调节编码速度和质量的平衡
        有ultrafast、superfast、veryfast、faster、fast、medium、slow、slower、veryslow、placebo这10个选项，从快到慢
        
    tune参数：主要配合视频类型和视觉优化的参数
        film：  电影、真人类型，对视频的质量非常严格时使用该选项
        animation：  动画，压缩的视频是动画片时使用该选项
        grain：      颗粒物很重，该选项适用于颗粒感很重的视频      
        stillimage：  静态图像，该选项主要用于静止画面比较多的视频    
        psnr：      提高psnr，该选项编码出来的视频psnr比较高
        ssim：      提高ssim，该选项编码出来的视频ssim比较高   
        fastdecode： 快速解码，该选项有利于快速解码    
        zerolatency：零延迟，用在需要非常低的延迟的情况下，比如电视电话会议的编码，视频直播
        
    profile参数：
        baseline： 基本画质。支持I/P 帧，只支持无交错（Progressive）和CAVLC；
        main：主流画质。提供I/P/B 帧，支持无交错（Progressive）和交错（Interlaced）， 也支持CAVLC 和CABAC 的支持；
        high：高级画质。在main Profile 的基础上增加了8x8内部预测、自定义量化、 无损视频编码和更多的YUV 格式；
        
        通常情况下压缩比例来说，baseline< main < high，对于带宽比较局限的在线视频，可能会选择high，
        但有些时候，做个小视频，希望所有的设备基本都能解码（有些低端设备或早期的设备只能解码 baseline），
        那就牺牲文件大小吧，用baseline！
        
        “当然，压缩比例越高，编码时间也会增加的”
        
        使用范围：
            baseline: 多应用于实时通信领域;
            main: 多应用于流媒体领域;
            high: 多应用于广电和存储领域;
            
  （6）采集输入的帧率和编码输出的帧率相关注意点
  
        1、当采集输入帧率大于编码输出帧率时，需要控制队列入数据，因为单位时间内入队的数据过多时，就会导致编码出来的视频时间加长
        
        2、当采集输入帧率小于编码输出帧率时，编码出来的视频播放时间就会变短
        
        所以采集输入帧率和编码输出帧率，两者需要兼容好
        
        在Android使用ImageReader时，发现其回调回来的image帧率比较慢，明显跟不上25帧的帧率,720p分辨率下帧率只为8帧左右，测试于三星手机（SM-C7100）
        
        用opengl构造输入源的情况下，在720p的情况下输入帧率也是只有8帧左右，测试于三星手机（SM-C7100）,在小米手机上帧率就更低了，只有4帧左右
        
  （7）RTMP推流时的优化
  
        1、10000000 微妙
        FFmpeg推流延迟10秒问题记录 (https://blog.csdn.net/vanjoge/article/details/79545490)
        2、
        FFMPEG关于推流端降低延迟调节（一） https://blog.csdn.net/zhuweigangzwg/article/details/82223011
        3、
        弱网环境下的RTMP推流策略  https://blog.csdn.net/carryinfo/article/details/54237230
        
        4、弱网推流优化
       
            android 弱网工具：QNET
            (1)av_interleaved_write_frame在网络不好时，会卡住
            (2)avio_open 会卡住
            
   （8）RTMP推aac音频时，av_interleaved_write_frame返回0，但是服务器端并没有收到推流信息
   
        //http://cn.voidcc.com/question/p-yehozwgx-bnt.html
        发现如果同时添加了video stream和audio stream，但是只write aac音频，这个时候av_interleaved_write_frame返回0，也不会写成功。。。
        如果只添加了audio stream，那么write aac音频时，这个时候av_interleaved_write_frame返回0，会写入成功