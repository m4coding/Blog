# UserLAnd


## 编译自己的NDK工具链

https://android.googlesource.com/toolchain/llvm_android/+/master/README.md

https://github.com/Lzhiyong/termux-ndk

已过时：

    https://hardenedlinux.github.io/toolchains/2016/04/01/How_to_build_Clang_toolchains_for_Android.html

准备：

    1、cmake ninja 
    2、自己的gcc链 想办法从系统自身获取
    3、clang clang++  （llvm）
    4、 aapt aapt2  （find . -type f -name "aapt2*-linux.jar"）
        .gradle/caches/modules-2/files-2.1/com.android.tools.build/aapt2

## Ubuntu系统 18.04

1、编译python

    （1）异常 zipimport.ZipImportError: can't decompress data; zlib not available

        sudo apt-get install zlib1g-dev
    
        如果有安装时有异常，则先sudo apt --fix-broken install  


2、ninja  rsync

r383902b

## 编译gcc

https://android.googlesource.com/toolchain/gcc/

编译脚本：

    toolchain/gcc/build.py
        toolchain/gcc/do_build.py
        toolchain/gcc/build-gcc.sh    

    ndk/build/tools/prebuilt-common.sh  
    prebuilts/gcc/linux-86/host/x86_64-linux-glibc2.15-4.8/build-precise-multilib-toolchain.sh


生成的自身构建工具链路径 /tmp/ndk-root/build/toolchain/host-x86_64-linux-gnu/install

从ndk中生成自身构建链

    make-standalone-toolchain.sh --toolchain=aarch64-linux-android-4.9 --install-dir=要安装的路径
    make-standalone-toolchain.sh --toolchain=arm-linux-androideabi-4.9 --install-dir=要安装的路径
    make-standalone-toolchain.sh --toolchain=x86_64-4.9 --install-dir=要安装的路径

1、交叉编译参数说明

https://sites.google.com/site/readliner/study/host-target-build

https://blog.csdn.net/dragon101788/article/details/17557343

    --build=编译该软件所使用的平台
    --host=该软件将运行的平台  
    --target=该软件所处理的目标平台


2、编译异常：

    修改host为aarch64-linux-android时，出现的问题
    checking host system type... Invalid configuration `aarch64-linux-android': machine `aarch64' not recognized
        由于gmp-5.0.5没有适配aarch64(gmp-6.2.0是有的)，所以需要在configfsf.sub其加上 | aarch64 | aarch64_be \
        case $basic_machine in
            # Recognize the basic CPU types without company name.
            # Some are omitted here because they have special meanings below.
            1750a | 580 \
            | a29k \
            | alpha | alphaev[4-8] | alphaev56 | alphaev6[78] | alphapca5[67] \
            | alpha64 | alpha64ev[4-8] | alpha64ev56 | alpha64ev6[78] | alpha64pca5[67] \
            | am33_2.0 \
            | aarch64 | aarch64_be \
            | arc | arm | arm[bl]e | arme[lb] | armv[2345] | armv[345][lb] | avr | avr32 \
    

    configure: error: ABI=64 is not among the following valid choices: standard
    
    修改configure，添加aarch64支持
    
    aarch64*)
        abilist="64 32"
        path="arm/v7a/cora15/neon arm/neon arm/v7a/cora15 arm/v6t2 arm/v6 arm/v5 arm"
        path_64="arm64"
        gcc_cflags_arch="-march=armv8-a"
        gcc_cflags_neon="-mfpu=neon"
        gcc_cflags_tune=""
        ;;
      *)

/tmp/ndk-root/build/toolchain/temp-src/gmp-6.2.0/configure   '--enable-bionic-libs' '--enable-libatomic-ifuncs=no' '--enable-initfini-array' '--disable-nls' '--prefix=/tmp/7652141cba9b3784ff304cac0f5ebfc0' '--with-binutils-version=2.27' '--with-ppl-version=1.2' '--with-mpfr-version=3.1.1' '--with-mpc-version=1.0.1' '--with-gmp-version=6.2.0' '--with-gcc-version=4.9' '--with-gdb-version=none' '--with-gxx-include-dir=/tmp/7652141cba9b3784ff304cac0f5ebfc0/include/c++/4.9.x' '--with-bugurl=http://source.android.com/source/report-bugs.html' '--enable-languages=c,c++' '--disable-bootstrap' '--enable-plugins' '--enable-libgomp' '--enable-gnu-indirect-function' '--disable-libsanitizer' '--enable-gold' '--enable-ld=default' '--enable-threads' '--enable-eh-frame-hdr-for-static' '--enable-fix-cortex-a53-835769' '--enable-graphite=yes' '--with-isl-version=0.11.1' '--with-cloog-version=0.18.0' --program-transform-name='s&^&aarch64-linux-android-&' --prefix=/tmp/ndk-root/build/toolchain/temp-install --disable-shared --host=aarch64-linux-android --build=x86_64-linux-gnu --enable-cxx


/opt/jenkins/aosp/gcc/toolchain/build/configure  
'--enable-bionic-libs'  
'--enable-libatomic-ifuncs=no'  
'--enable-initfini-array'  
'--disable-nls'  
'--prefix=/tmp/7652141cba9b3784ff304cac0f5ebfc0'  
'--with-sysroot=/tmp/7652141cba9b3784ff304cac0f5ebfc0/sysroot'  
'--with-binutils-version=2.27'  
'--with-ppl-version=1.2'  
'--with-mpfr-version=3.1.1'  
'--with-mpc-version=1.0.1'  
'--with-gmp-version=6.2.0'  
'--with-gcc-version=4.9'  
'--with-gdb-version=none'  
'--with-gxx-include-dir=/tmp/7652141cba9b3784ff304cac0f5ebfc0/include/c++/4.9.x'  
'--with-bugurl=http://source.android.com/source/report-bugs.html'  
'--enable-languages=c,c++'  
'--disable-bootstrap'  
'--enable-plugins'  
'--enable-libgomp'  
'--enable-gnu-indirect-function'  
'--disable-libsanitizer'  
'--enable-gold'  
'--enable-ld=default'  
'--enable-threads'  
'--enable-eh-frame-hdr-for-static'  
'--enable-fix-cortex-a53-835769'  
'--enable-graphite=yes'  
'--with-isl-version=0.11.1'  
'--with-cloog-version=0.18.0'  
--program-transform-name='s&^&aarch64-linux-android-&'  
--with-gdb-version=none  
--build=x86_64-linux-gnu
--host=x86_64-linux-gnu  
--target=aarch64-linux-android  
--prefix=/tmp/ndk-root/build/toolchain/host-x86_64-linux-gnu/install


aarch64-linux-android-g++ -W -Wall    -Wstack-usage=262144 -Werror -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -frandom-seed=dwp -I/opt/jenkins/aosp/gcc/toolchain/build/../binutils/binutils-2.27/gold/../zlib -O2 -s  -Wno-implicit-fallthrough -Wno-int-in-bool-context -static-libgcc -static-libstdc++ -static -static-libstdc++ -static-libgcc  -o dwp dwp.o libgold.a ../libiberty/libiberty.a   -lpthread  -L./../zlib -lz
/opt/jenkins/aosp/aarch64-linux-android/bin/../lib/gcc/aarch64-unknown-linux-android/7.4.1/../../../../aarch64-unknown-linux-android/bin/ld: cannot find -lpthread


ndk中不包含libpthread，已将它融合在libc中


dev_defaults.sh 默认的配置变量

prebuilt-common.sh 通用的方法

build-gcc.sh 编译脚本

制作补丁：

/tmp/ndk-root/build/toolchain/temp-src/ppl-1.2/src/Determinate_inlines.hh:293:1: error: missing 'typename' prior to dependent type template name 'Determinate<PSET>::Binary_Operator_Assign_Lifter

    改为：
     inline 
     typename Determinate<PSET>::template Binary_Operator_Assign_Lifter<Binary_Operator_Assign>
     Determinate<PSET>::lift_op_assign(Binary_Operator_Assign op_assign) {
       return Binary_Operator_Assign_Lifter<Binary_Operator_Assign>(op_assign);
     }

/tmp/ndk-root/build/toolchain/temp-src/ppl-1.2/src/OR_Matrix_inlines.hh  
    template <typename U>
    inline typename OR_Matrix<T>::template Pseudo_Row<U>&
    OR_Matrix<T>::Pseudo_Row<U>::operator=(const Pseudo_Row& y) {


/opt/jenkins/aosp/gcc/toolchain/build/../gcc/gcc-4.9/gcc/gengtype-state.c:754:4: error: use of undeclared identifier 'fputs_unlocked'; did you mean 'putc_unlocked'?

/opt/jenkins/aosp/ndk_my_toolschain/arm64/bin/../sysroot/usr/include/stdio.h:321:5: note: 'putc_unlocked' declared here
int putc_unlocked(int __ch, FILE* __fp);

./toolchain/gcc/build.py --toolchain aarch64-linux-android

  build_cmd = [
            'bash',
            'build-gcc.sh',
            build_support.toolchain_path(),
            build_support.ndk_path(),
            toolchain_name,
            build_support.jobs_arg(),
            sysroot_arg,
            '--try-64',
        ]




## 编译arm linux交叉编译gcc工具

[构建gcc交叉编译工具链](https://blog.csdn.net/fickyou/article/details/51671006)


打包

tar -czf ../aarch64-linux-gcc-toolchain-host-x86_64-linux.tar.gz *

使用host=aarch64-linux进行编译出现的问题：

    1、binutils包用2.24编译会提示异常，换用binutils-2.27.tar.gz就可以
    
    2、


    最后一步出现了异常。。第七步。。 make -j4  （编译过程中aarch64-linux 有发生改变导致的异常）



替换clang clang++  /toolchains/llvm/prebuilt/linux-x86_64/bin

替换./toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64/bin

/tmp/ndk-root取出不包含gcc的其他组件包

替换 /toolchains/llvm/prebuilt/linux-x86_64/各个架构的bin （如arm-linux-androideabi）

i686-linux-android-gcc -dumpspecs > tmp-specs

/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=armv7-none-linux-androideabi21 --gcc-toolchain=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64 --sysroot=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/sysroot -g -DANDROID -fdata-sections -ffunction-sections -funwind-tables -fstack-protector-strong -D_FORTIFY_SOURCE=2 -march=armv7-a -mthumb -Wformat -Werror=format-security  -Wl,--exclude-libs,libgcc.a -Wl,--exclude-libs,libgcc_real.a -Wl,--exclude-libs,libatomic.a -static-libstdc++ -Wl,--build-id -Wl,--fatal-warnings -Wl,--exclude-libs,libunwind.a -Wl,--no-undefined -Qunused-arguments -Wl,--gc-sections CMakeFiles/cmTC_f1235.dir/testCCompiler.c.o  -o cmTC_f1235  -latomic -lm

/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=armv7-none-linux-androideabi21 --gcc-toolchain=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64 --sysroot=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/sysroot -g -DANDROID -fdata-sections -ffunction-sections -funwind-tables -fstack-protector-strong -D_FORTIFY_SOURCE=2 -march=armv7-a -mthumb -Wformat -Werror=format-security  -Wl,--exclude-libs,libgcc.a -Wl,--exclude-libs,libgcc_real.a -Wl,--exclude-libs,libatomic.a -static-libstdc++ -Wl,--build-id -Wl,--fatal-warnings -Wl,--exclude-libs,libunwind.a -Wl,--no-undefined -Qunused-arguments -Wl,--gc-sections HelloWorld.c  -o cmTC_f1235  -latomic -lm

/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=arm64-v8a-none-linux-androideabi21 --gcc-toolchain=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64 --sysroot=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/sysroot -g -DANDROID -fdata-sections -ffunction-sections -funwind-tables -fstack-protector-strong -D_FORTIFY_SOURCE=2 -march=arm64-v8a -mthumb -Wformat -Werror=format-security  -Wl,--exclude-libs,libgcc.a -Wl,--exclude-libs,libgcc_real.a -Wl,--exclude-libs,libatomic.a -static-libstdc++ -Wl,--build-id -Wl,--fatal-warnings -Wl,--exclude-libs,libunwind.a -Wl,--no-undefined -Qunused-arguments -Wl,--gc-sections HelloWorld.c  -o cmTC_f1235  -latomic -lm

/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=armv7-none-linux-androideabi21 --gcc-toolchain=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64 --sysroot=/opt/ndk/android-ndk-r21b/toolchains/llvm/prebuilt/linux-x86_64/sysroot -g -DANDROID -fdata-sections -ffunction-sections -funwind-tables -fstack-protector-strong -D_FORTIFY_SOURCE=2 -march=armv7-a -mthumb -Wformat -Werror=format-security  -Wl,--exclude-libs,libgcc.a -Wl,--exclude-libs,libgcc_real.a -Wl,--exclude-libs,libatomic.a -static-libstdc++ -Wl,--build-id -Wl,--fatal-warnings -Wl,--exclude-libs,libunwind.a -Wl,--no-undefined -Qunused-arguments -Wl,--gc-sections SecurityInfo.c  -o cmTC_f1235  -latomic -lm


sdk build_tools 里面的appt2全部都要指向系统的appt2