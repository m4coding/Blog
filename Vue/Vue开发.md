# Vue开发

## 创建工程

    通过WebStorm创建vue工程即可，会创建一个完整的vue工程
    
 
## 部署到服务器
    
    执行npm run build会生成dist目录
    
    这个目录就是需要执行的文件
    
    注意：vue-cli2 和vue-cli3的配置打包方式不一致的  （我使用的是vue-cli 2.9.6,其打包配置方式也和vue-cli3一致了）
    
[Vue-cli3.0打包部署到Nginx](https://www.cnblogs.com/jdWu-d/p/12197156.html)

    在vue-cli3中，打包的相关配置放到了vue.config.js
    这个配置文件默认是没有的，需要自己手动创建
    
    如：
    
    module.exports = {
        // 基本路径  配置静态资源为相对路径，确保dist中index.html能本地打开而不空白显示
        publicPath:"./"
    }