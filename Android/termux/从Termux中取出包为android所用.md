# 从Termux中取出包为android所用

由于termux是直接运行在android上，其对应的package包的命令是可以直接在android上运行，只要对应的库路径修改正确，
所以可以按需取termux的软件deb包，取出lib和bin为android所用，以避免取浪费时间去编译源码包

动态链接库运行配置：

LD_LIBRARY_PATH环境变量用于在程序加载运行期间查找动态链接库时指定除了系统默认路径之外的其他路径，注意，LD_LIBRARY_PATH中指定的路径会在系统默认路径之前进行查找。

例如export LD_LIBRARY_PATH=/demo/lib:$LD_LIBRARY_PATH

[termux各种架构deb包地址 http://termux.net/dists/stable/main/](http://termux.net/dists/stable/main/)

termux package aarch64 binary地址：[https://termux.net/dists/stable/main/binary-aarch64](https://termux.net/dists/stable/main/binary-aarch64/)