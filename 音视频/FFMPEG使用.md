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
        
## 命令行使用

     //播放裸的aac文件，-ar指定采样率  -ac指定通道数
    ffplay -ar 44100 -ac 1 E:\test.aac  
    
    //播放pcm文件  -ar指定采样率  -ac指定通道数 -f 指定采样格式
    ffplay -ar 44100 -ac 1 -f s16le E:\test.pcm
    
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