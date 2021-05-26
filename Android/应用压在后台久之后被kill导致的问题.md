# 应用压在后台久之后被kill导致的问题

## 模拟这种情况的出现

由于现在手机的内存越来越大，所以出现这种情况的概率并不大，为了模拟这种情况的出现可以借助adb

如我的应用包名为com.xxx.yyy

当应用压到后台后，执行命令adb shell am kill  com.xxx.yy，就可以将应用kill掉

注意的是当应用在前台时，执行这个命令是没有效果的

然后应用被kill掉，当再次点击启动app时，就会走android默认的恢复界面的那套流程了

此时就可验证相关问题了

## 解决Fragment久置后台，App被kill掉后，恢复的问题

在FragmentActivity的onCreate添加

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        if (savedInstanceState != null) {
            savedInstanceState.putParcelable("android:support:fragments", null);//清空保存Fragment的状态数据,这个时候App被kill掉后，fragment不会走恢复流程
        }

        super.onCreate(savedInstanceState);
   }

