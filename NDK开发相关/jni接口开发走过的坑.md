# jni接口开发走过的坑

**1、多线程中使用JNIEnv**

    在C++多线程开发过程中，有时会需要回调java方法，需要用到JNIEnv*。。需要从哪里获取到它呢？
    由于JNIEnv*与线程有关，如果需要javaVm的AttachCurrentThread来获取才行，直接通过全局变量的方式
    保存JNIEnv*是不可行的。获取例子如下：
    
    static JavaVM *gVm = nullptr;
    //自定义结构体变量，全局保存自定义类相关信息
    static struc {
        jclass clazz;
        jMethodId ctor;
    } gMyInfo;
    
    //java static{ }里面调用，表示在类初始化里面使用
    static void nativeInitForClass(JNIEnv *env) {
        jclass myInfoClass = env->FindClass("com/m4coding/xxx/MyInfo");
        //全局保存
        gMyInfo.clazz = (jclass)(env->NewGlobalRef(clazz.get()));
        //构造方法
        gMyInfo.ctor = env->GetMethodID(clazz.get(), "<init>", "(ILjava/lang/String;)V");
    }
    
    jint JNI_OnLoad(JavaVM *vm, void *reserved) {
        gVm = vm; //此处全局变量保存JavaVm指针
    
        JNIEnv *env = NULL;
        jint result = -1;
    
        if (vm->GetEnv((void **) &env, JNI_VERSION_1_4) != JNI_OK) {
            LOGE_TAG(LOG_TAG, "%s", "ERROR: GetEnv failed");
            goto bail;
        }
    
        if (registerNativeMethods(env) < 0) {
            LOGE_TAG(LOG_TAG, "%s", "ERROR: HelloWorld native registration failed");
            goto bail;
        }
    
        /* success -- return valid version number */
        result = JNI_VERSION_1_4;
    
        bail:
        return result;
    }
    
    //利用JavaVm指针来获取JNIEnv*
    static JNIEnv* attachCurrentThread() {
        JNIEnv* env = nullptr;
        auto result = gVm->AttachCurrentThread(&env, nullptr);
        CHECK(result == JNI_OK);
        return env;
    }
    
    //假设这个方法在多线程中运行。。
    void threadRun() {
        JNIEnv *env = attachCurrentThread();
        
        //查找系统类
        jclass stringClass = env->FindClass("java/lang/String");
        //stringClass可以实现查找成功
        
        //查找自定义类
        jclass infoClass = env->FindClass("com/m4coding/xxx/MyInfo")
        //infoClass为NULL，因为env只加载了系统类，自定义类无法找到。。需要另一种方法。。
        
        //另一种方法  直接利用在nativeInitForClass保存的类信息进行实例化对象
        jobject obj2 = env->NewObject(gMyInfo.clazz, gMyInfo.ctor);
        
        //最后要记得DetachCurrentThread，释放当前线程的JNIEnv*
        if (env) {
            gVm->DetachCurrentThread();
            env = nullptr;
        }
    }
    
    
**2、System.loadLibrary多次加载同一so库时，JNI_Onload只会回调一次，也即如果使用相同的库名称多次调用此方法,则忽略第二次和后续调用**

    这个场景主要出现在一个so库，包含了多个对外使用的Java功能类，这个时候在每个java功能类内
    static {
        System.loadLibrary("multimedia");
    }
    静态代码块里面引入so，从而造成多次引入
    
    为了对每个java类进行动态注册，可以考虑在JNI_Onload方法内分别注册各个java类对应的jni接口
    
    可参考android源码   --> android_media_MediaPlayer.cpp
    extern int register_android_media_ImageReader(JNIEnv *env);
    extern int register_android_media_ImageWriter(JNIEnv *env);
    extern int register_android_media_Crypto(JNIEnv *env);
    extern int register_android_media_Drm(JNIEnv *env);
    extern int register_android_media_Descrambler(JNIEnv *env);
    extern int register_android_media_MediaCodec(JNIEnv *env);
    extern int register_android_media_MediaExtractor(JNIEnv *env);
    extern int register_android_media_MediaCodecList(JNIEnv *env);
    extern int register_android_media_MediaHTTPConnection(JNIEnv *env);
    extern int register_android_media_MediaMetadataRetriever(JNIEnv *env);
    extern int register_android_media_MediaMuxer(JNIEnv *env);
    extern int register_android_media_MediaRecorder(JNIEnv *env);
    extern int register_android_media_MediaScanner(JNIEnv *env);
    extern int register_android_media_MediaSync(JNIEnv *env);
    extern int register_android_media_ResampleInputStream(JNIEnv *env);
    extern int register_android_media_MediaProfiles(JNIEnv *env);
    extern int register_android_mtp_MtpDatabase(JNIEnv *env);
    extern int register_android_mtp_MtpDevice(JNIEnv *env);
    extern int register_android_mtp_MtpServer(JNIEnv *env);
    
    jint JNI_OnLoad(JavaVM* vm, void* /* reserved */)
    {
        JNIEnv* env = NULL;
        jint result = -1;
    
        if (vm->GetEnv((void**) &env, JNI_VERSION_1_4) != JNI_OK) {
            ALOGE("ERROR: GetEnv failed\n");
            goto bail;
        }
        assert(env != NULL);
    
        if (register_android_media_ImageWriter(env) != JNI_OK) {
            ALOGE("ERROR: ImageWriter native registration failed");
            goto bail;
        }
    
        if (register_android_media_ImageReader(env) < 0) {
            ALOGE("ERROR: ImageReader native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaPlayer(env) < 0) {
            ALOGE("ERROR: MediaPlayer native registration failed\n");
            goto bail;
        }
    
        if (register_android_media_MediaRecorder(env) < 0) {
            ALOGE("ERROR: MediaRecorder native registration failed\n");
            goto bail;
        }
    
        if (register_android_media_MediaScanner(env) < 0) {
            ALOGE("ERROR: MediaScanner native registration failed\n");
            goto bail;
        }
    
        if (register_android_media_MediaMetadataRetriever(env) < 0) {
            ALOGE("ERROR: MediaMetadataRetriever native registration failed\n");
            goto bail;
        }
    
        if (register_android_media_ResampleInputStream(env) < 0) {
            ALOGE("ERROR: ResampleInputStream native registration failed\n");
            goto bail;
        }
    
        if (register_android_media_MediaProfiles(env) < 0) {
            ALOGE("ERROR: MediaProfiles native registration failed");
            goto bail;
        }
    
        if (register_android_mtp_MtpDatabase(env) < 0) {
            ALOGE("ERROR: MtpDatabase native registration failed");
            goto bail;
        }
    
        if (register_android_mtp_MtpDevice(env) < 0) {
            ALOGE("ERROR: MtpDevice native registration failed");
            goto bail;
        }
    
        if (register_android_mtp_MtpServer(env) < 0) {
            ALOGE("ERROR: MtpServer native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaCodec(env) < 0) {
            ALOGE("ERROR: MediaCodec native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaSync(env) < 0) {
            ALOGE("ERROR: MediaSync native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaExtractor(env) < 0) {
            ALOGE("ERROR: MediaCodec native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaMuxer(env) < 0) {
            ALOGE("ERROR: MediaMuxer native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaCodecList(env) < 0) {
            ALOGE("ERROR: MediaCodec native registration failed");
            goto bail;
        }
    
        if (register_android_media_Crypto(env) < 0) {
            ALOGE("ERROR: MediaCodec native registration failed");
            goto bail;
        }
    
        if (register_android_media_Drm(env) < 0) {
            ALOGE("ERROR: MediaDrm native registration failed");
            goto bail;
        }
    
        if (register_android_media_Descrambler(env) < 0) {
            ALOGE("ERROR: MediaDescrambler native registration failed");
            goto bail;
        }
    
        if (register_android_media_MediaHTTPConnection(env) < 0) {
            ALOGE("ERROR: MediaHTTPConnection native registration failed");
            goto bail;
        }
    
        /* success -- return valid version number */
        result = JNI_VERSION_1_4;
    
    bail:
        return result;
    }