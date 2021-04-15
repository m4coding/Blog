# WebView调试

1、WebView添加调试代码

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
         webView.setWebContentsDebuggingEnabled(true);
    }

2、手机usb连接，可调试

3、使用chrome访问chrome://inspect/#devices

4、使用手机进入对应的WebView，然后选中对应的Web页面进行inspect即可进入DevTool调试 （这个过程需要一段时间的等待）

    DevTools可以在Elements选项tab查看对应ui的元素、Sources选项tab查看对应的源码

## 参考

[android 调试webView中的Html](https://juejin.cn/post/6844903936902561799)