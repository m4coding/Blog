# CustomScrollView

[在Flutter中创建有意思的滚动效果 - Sliver系列](https://segmentfault.com/a/1190000019902201)

[Flutter - Sliver Layout horizontal scroll inside Sliver List](https://stackoverflow.com/questions/52738034/flutter-sliver-layout-horizontal-scroll-inside-sliver-list)

[Flutter 实现类似美团外卖店铺页面滑动效果](https://zhuanlan.zhihu.com/p/106197796)

Sliver成员：

    1、SliverAppBar
    2、SliverList  
    3、SliverFixedExtentList
    4、SliverGrid
    5、SliverPersistentHeader
    6、SliverFillRemaining
    7、SliverToBoxAdapter
    8、SliverPadding
    9、SliverStaggeredGrid  （第三方库 用于实现瀑布流）
    
由于CustomeScrollView的子组件只能是Sliver系列，所以如果你想将一个普通组件塞进CustomScrollView，那么务必将该组件用SliverToBoxAdapter包裹。
    
## 注意

1、在CustomScrollView中使用StaggeredGridView 

    StaggeredGridView用在CustomScrollView中，会报异常，应该使用SliverStaggeredGrid来代替实现瀑布流，但是有个问题，在SliverStaggeredGrid后面再放置其他GridView或者ListView会出现滚动异常。。。
    将SliverStaggeredGrid放在最后面就不会有问题
    
    
2、TabBarView在CustomScrollView里面使用时，发现如果TabBarView里面是List等滚动的View，不能联动滚动，是个坑。。

    这里可以使用extended_nested_scroll_view的第三方插件库