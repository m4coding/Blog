# Gradle相关

## 刷新Maven包依赖问题

引用maven库的包时，有些时候自己的maven包修改了内容，但是包名版本却还是原来的。为了引用最新包需要重新刷新依赖，否则还是旧包不会重新loadown

刷新缓存命令：

    gradlew build --refresh-dependencies