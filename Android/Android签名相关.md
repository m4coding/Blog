# Android签名相关

[Android 查看Apk签名方式V1和V2](https://www.cnblogs.com/zhou-guobao/p/how-to-know-apk-sign.html)

v2版本签名在android7.0 （24）开始支持，当minSdkVersion设置为24时，默认就会打v2版本的签名，当minSdkVersion小于24时就会默认同时打v1和v2的签名

通过v1SigningEnabled和v2SigningEnabled对应的开关配置，可以设置签名


  android {
    ...
    defaultConfig {
      ...
      signingConfigs {
        myConfig {
            ...
            v1SigningEnabled true
            v2SigningEnabled true
        }
      }
    }
  }
  
需要注意的是在minSdkVersion设置为24时，同时设置v1SigningEnabled和v2SigningEnabled为true时，发现只能打v2签名，不能v1，配置v1SigningEnabled为true，

v2SigningEnabled为false时就可以只打v1

2021-04-19 应用宝市场需要包支持v1签名才能上线，因为其不能只兼容v2
