# Gradle相关

## 刷新Maven包依赖问题

引用maven库的包时，有些时候自己的maven包修改了内容，但是包名版本却还是原来的。为了引用最新包需要重新刷新依赖，否则还是旧包不会重新loadown

刷新缓存命令：

    gradlew build --refresh-dependencies


## .gradlew目录中添加init.gradle，解决库下载问题

例子：

    allprojects{
        repositories {
            def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public'
            def ALIYUN_JCENTER_URL = 'https://maven.aliyun.com/repository/public'
            all { ArtifactRepository repo ->
                if(repo instanceof MavenArtifactRepository){
                    def url = repo.url.toString()
                    if (url.startsWith('https://repo1.maven.org/maven2')) {
                        project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                        remove repo
                    }
                    if (url.startsWith('https://jcenter.bintray.com/')) {
                        project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
                        remove repo
                    }
                }
            }
            maven {
                url ALIYUN_REPOSITORY_URL
                url ALIYUN_JCENTER_URL
            }
        }
    }

[阿里云云效 Maven ](https://maven.aliyun.com/mvn/guide)

## 看懂编译注解annotationProcessor和kapt

[看懂编译注解annotationProcessor和kapt](https://www.jianshu.com/p/472e66632ed0)

## release打包混淆后，导致一些模型的private变量变为了public变量

[Android打包aar后private可见性变public的问题及解决](https://blog.csdn.net/wmadao11/article/details/102613078)
