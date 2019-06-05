# jni接口开发走过的坑

1、多线程中使用JNIEnv*

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