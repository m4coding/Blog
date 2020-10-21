# Termux-app

## 自制bootstrap包

git clone https://github.com/termux/termux-packages.git

如果是在ubuntu中编译，需要先运行脚本setup-ubuntu.sh配置好编译环境

1、./build-all.sh

报错处理：

（1）NDK not pointing at a directory! 

 执行scripts/setup-android-sdk.sh配置android sdk和ndk环境即可
 
 [NDK not pointing at a directory!](https://github.com/termux/termux-packages/issues/896)
 
 
 (2) /data/TERMUX_ARCH: No such file or directory
 
 由于没有/data目录导致的，创建个/data目录即可
 
 
2、网络问题导致包下载异常

由于一些包无法下载，需要翻墙才能下载成功，可以考虑使用proxychains

（1）go: github.com/alecthomas/chroma@v0.7.3: Get "https://proxy.golang.org/github.com/alecthomas/chroma/@v/v0.7.3.mod": dial tcp 216.58.200.49:443: connect: connection refused

[一键解决 go get golang.org/x 包失败](https://shockerli.net/post/go-get-golang-org-x-solution/)

配置下go代理export GOPROXY=https://goproxy.io重新编译下载
    
（2）twpayne/go-internal/@v/v1.5.3-0.20200706163000-4426ab554b0a.mod: 404 Not Found

 更新chezmoi到1.8.6，因为对应的go-internal 15.3包已不存在
 
 （3）编译gzip包出现config.log提示exit 为1的异常，是由于proxychains4导致的，去掉代理，重新编译即可
 
## 从系统上取出bootstrap包来自制

现在在手机上配置好环境，然后从手机系统上复制出来进行打包成bootstrap

要注意修改和添加SYMLINKS.txt文件，做好对应关系

## UserLAnd

修改了脚本  /data/data/test.ula/files/support/busybox

## 参考

[[译]非系统盘下安装Linux子系统的方式](https://www.jianshu.com/p/f5499a7388b2)