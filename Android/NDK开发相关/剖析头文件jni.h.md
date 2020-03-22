# 剖析头文件jni.h

（1）定义一些与java基本类型匹配的数据类型

    typedef uint8_t  jboolean; /* unsigned 8 bits */     -->  boolean
    typedef int8_t   jbyte;    /* signed 8 bits */       -->  byte
    typedef uint16_t jchar;    /* unsigned 16 bits */    -->  char
    typedef int16_t  jshort;   /* signed 16 bits */      -->  short
    typedef int32_t  jint;     /* signed 32 bits */      -->  int
    typedef int64_t  jlong;    /* signed 64 bits */      -->  long
    typedef float    jfloat;   /* 32-bit IEEE 754 */     -->  float
    typedef double   jdouble;  /* 64-bit IEEE 754 */     -->  double
    
    /* "cardinal indices and sizes" */
    typedef jint     jsize;
    
    
 （2）定义一些与java引用数据类型匹配的数据类型   (**区分C++与C两种情况**)
 
    1、C++语言环境中定义：
 
    class _jobject {};
    class _jclass : public _jobject {};
    class _jstring : public _jobject {};
    class _jarray : public _jobject {};
    class _jobjectArray : public _jarray {};
    class _jbooleanArray : public _jarray {};
    class _jbyteArray : public _jarray {};
    class _jcharArray : public _jarray {};
    class _jshortArray : public _jarray {};
    class _jintArray : public _jarray {};
    class _jlongArray : public _jarray {};
    class _jfloatArray : public _jarray {};
    class _jdoubleArray : public _jarray {};
    class _jthrowable : public _jobject {};
    
    typedef _jobject*       jobject;              -->    java.lang.Object
    typedef _jclass*        jclass;               -->    java.lang.Class
    typedef _jstring*       jstring;              -->    java.lang.String
    typedef _jarray*        jarray;               -->    泛指java中的任何类型数组
    typedef _jobjectArray*  jobjectArray;         -->    java.lang.Object[]
    typedef _jbooleanArray* jbooleanArray;        -->    boolean[]
    typedef _jbyteArray*    jbyteArray;           -->    byte[]
    typedef _jcharArray*    jcharArray;           -->    char[]
    typedef _jshortArray*   jshortArray;          -->    short[]
    typedef _jintArray*     jintArray;            -->    int[]
    typedef _jlongArray*    jlongArray;           -->    long[]
    typedef _jfloatArray*   jfloatArray;          -->    float[]
    typedef _jdoubleArray*  jdoubleArray;         -->    double[]
    typedef _jthrowable*    jthrowable;           -->    java.lang.Throwable
    typedef _jobject*       jweak;                -->    java.lang.ref.WeakReference  弱引用
    
    以_jobject空类为最基本的类，继承扩展，java引用数据类型在native实际表示为类的指针
    
    2、C语言环境中定义：
    
    typedef void*           jobject;              -->    java.lang.Object
    typedef jobject         jclass;               -->    java.lang.Class
    typedef jobject         jstring;              -->    java.lang.String
    typedef jobject         jarray;               -->    泛指java中的任何类型数组
    typedef jarray          jobjectArray;         -->    java.lang.Object[]
    typedef jarray          jbooleanArray;        -->    boolean[]
    typedef jarray          jbyteArray;           -->    byte[]
    typedef jarray          jcharArray;           -->    char[]
    typedef jarray          jshortArray;          -->    short[]
    typedef jarray          jintArray;            -->    int[]
    typedef jarray          jlongArray;           -->    long[]
    typedef jarray          jfloatArray;          -->    float[]
    typedef jarray          jdoubleArray;         -->    double[]
    typedef jobject         jthrowable;           -->    java.lang.Throwable
    typedef jobject         jweak;                -->    java.lang.ref.WeakReference  弱引用
    
    在C语言环境中，java引用数据类型都是以void*指针来表示的
    
    
    //属性Id
    struct _jfieldID;                       /* opaque structure */
    typedef struct _jfieldID* jfieldID;     /* field IDs */
    
    //方法Id
    struct _jmethodID;                      /* opaque structure */
    typedef struct _jmethodID* jmethodID;   /* method IDs */
    
（3）方法参数、方法返回类型 对应  -->基本类型

    typedef union jvalue {
           jboolean    z;
           jbyte       b;
           jchar       c;
           jshort      s;
           jint        i;
           jlong       j;
           jfloat      f;
           jdouble     d;
           jobject     l;
       } jvalue;
      
     对应如下：
     jboolean  --  z  --  boolean
     jbyte     --  b  --  byte
     jchar     --  c  --  char
     jshort    --  s  --  short
     jint      --  i  --  int
     jlong     --  j  --  long
     jfloat    --  f  --  float
     jdouble   --  d  --  double
     jobject   --  l  --  Ojbect
     
     
（4）定义对应于Java的引用类型

    typedef enum jobjectRefType {
        JNIInvalidRefType = 0,
        JNILocalRefType = 1,
        JNIGlobalRefType = 2,
        JNIWeakGlobalRefType = 3
    } jobjectRefType;
    
    JNIInvalidRefType -->  无效引用
    JNILocalRefType   -->  局部引用
    JNIGlobalRefType  -->  全局引用
    JNIWeakGlobalRefType  --> 弱引用
    
（5）JNIEnv 分析

    实际对应于_JNIEnv结构体
    
    
（6）so库加载时回调，以及卸载时回调

    /*
    * Prototypes for functions exported by loadable shared libs.  These are
    * called by JNI, not provided by JNI.
    */
   JNIEXPORT jint JNI_OnLoad(JavaVM* vm, void* reserved);
   JNIEXPORT void JNI_OnUnload(JavaVM* vm, void* reserved);
   
   
   适用于动态注册jni
   