# 对判断底部导航栏NavigationBar是否显示的一点思考

1、DecorView组成分析

    StatusBar
    content  1、标题栏  2、界面内容显示
    NavigationBar
    
    navigationbar的高度 = window的高度(DecorView的高度) - statusbar高度 - content的高度
    也等于 navigationbar的高度 = window的高度(DecorView的高度) - content的bottom


    public static int getNavigationBarRealHeight(Activity activity) {
       Window window = activity.getWindow();
       View decorView = window.getDecorView();
       Rect rect = new Rect();
       //获取到程序显示的区域，包括标题栏，但不包括状态栏，也就是content部分
       decorView.getWindowVisibleDisplayFrame(rect);
       DisplayMetrics outMetrics = new DisplayMetrics();
       WindowManager windowManager = window.getWindowManager();
       windowManager.getDefaultDisplay().getRealMetrics(outMetrics);
       return outMetrics.heightPixels - rect.bottom;
    }

2、getNavigationBarRealHeight实现在具体机型上的表现分析：

（1）Mate20 pro android10 能正确获取，当切换到系统设置导航栏方式后，切换回应用时，当前的Activity会重新OnCreate一次

（2）OPPO Find X3 android10 能正确获取，当切换到系统设置导航栏方式后，切换回应用时，当前的Activity不会重新OnCreate

（3）Redmi K40 android11 能正确获取，当切换到系统设置导航栏方式后，应用当前的Activity会马上重新OnCreate，不用等到切换回应用时

（4）VIVO X30 android10 能正确获取，当切换到系统设置导航栏方式后，切换回应用时，当前的Activity不会重新OnCreate

（5）Galaxy A52 5G android11 能正确获取，当切换到系统设置导航栏方式后，切换回应用时，当前的Activity不会重新OnCreate


## 参考

[Android NavigationBar Height](https://xiuyuantech.github.io/2020/05/23/android-navigationbar/)

[android DecorView的使用](https://blog.csdn.net/bzlj2912009596/article/details/78620766)