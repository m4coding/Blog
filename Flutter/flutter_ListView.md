# Flutter ListView

### ListView嵌套ListView

ListView嵌套ListView，子ListView需要设置两个属性：

1、shrinkWrap: true //处理无限高度问题、将高度跟随内容变化，不过这里会有点性能损耗

2、physics: new NeverScrollableScrollPhysics() //设置其不能滚动，由外部的父ListView处理滚动

### 由于ListView不能支持滚动到特定的index位置，很恶心。。推荐使用第三方框架的ListView实现这个功能

1、scrollable_positioned_list 一个包装的ListView，用于和官方的ListView类似

[scrollable_positioned_list](https://pub.dev/packages/scrollable_positioned_list)

2、scroll_to_index  （针对于子Widget的包装，浸入性会强一点）

[scroll_to_index](https://pub.dev/packages/scroll_to_index)