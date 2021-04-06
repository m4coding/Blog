# ADB操作

1、查看特定包名的app的当前Activity显示情况

adb shell dumpsys activity | findstr 包名 | findstr Run

2、查看特定包名的app的window显示情况

adb shell dumpsys window w |findstr 包名
