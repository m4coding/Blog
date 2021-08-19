# 交叉编译mono

编译过程中，发现如果交叉编译出android的bin后，在lib中的mono中是找不到对应的exe文件，然而编译pc上的mono是可以编译出exe文件的
这点需要注意，所以可以直接利用pc上编译出的exe给交叉编译的mono使用

https://www.mono-project.com/docs/getting-started/mono-basics/

编译了很多次，发现termux-mono是可以直接在android上使用的，不过需要注意的是mcs、csc等脚本需要修改对应mono-sgen和exe位置

例如mcs：

    #!/bin/sh
    exec /data/data/com.termux/files/usr/local/bin/mono $MONO_OPTIONS /data/data/com.termux/files/usr/local/lib/mono/4.5/mcs.exe "$@"


在新版的mono中，mono命令行其实对应就是mono-sgen

[termux-mono](https://github.com/IanusInferus/termux-mono)

[Mono を Android にインストールする](https://gitpress.io/u/1391/installing-mono-on-android)

[cross compiling mono for android targeting armva7](https://github.com/mono/mono/issues/20567) 

[Mono Android编译步骤](https://www.jianshu.com/p/b801a9b7cff8)