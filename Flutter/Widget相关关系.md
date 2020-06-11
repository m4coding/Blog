# Widget相关关系


## 布局类

根据子节点的关系，分为如下几个分类：

Widget | 对应的Element | 用处
:-:|:-:|:-:|
MultiChildRenderObjectWidget | MultiChildRenderObjectElement| 可包含多个Widget，会有一个children数组参数，用于保存Widget，如实现类Row、Column、Statck等
SingleChildRenderObjectWidget|SingleChildRenderObjectElement| 包含一个widget，一般会有child参数，如ConstrainedBox等
LeafRenderObjectWidget|LeafRenderObjectElement|Widget树的叶子节点，用于没有子节点的widget，通常基础组件都属于这一类，如Image。


RenderObjectWidget继承于Widget，是个虚类

```dart
/// RenderObjectWidgets provide the configuration for [RenderObjectElement]s,
/// which wrap [RenderObject]s, which provide the actual rendering of the
/// application.
abstract class RenderObjectWidget extends Widget {
  /// Abstract const constructor. This constructor enables subclasses to provide
  /// const constructors so that they can be used in const expressions.
  const RenderObjectWidget({ Key key }) : super(key: key);

  /// RenderObjectWidgets always inflate to a [RenderObjectElement] subclass.
  @override
  RenderObjectElement createElement();

  /// Creates an instance of the [RenderObject] class that this
  /// [RenderObjectWidget] represents, using the configuration described by this
  /// [RenderObjectWidget].
  ///
  /// This method should not do anything with the children of the render object.
  /// That should instead be handled by the method that overrides
  /// [RenderObjectElement.mount] in the object rendered by this object's
  /// [createElement] method. See, for example,
  /// [SingleChildRenderObjectElement.mount].
  @protected
  RenderObject createRenderObject(BuildContext context);

  /// Copies the configuration described by this [RenderObjectWidget] to the
  /// given [RenderObject], which will be of the same type as returned by this
  /// object's [createRenderObject].
  ///
  /// This method should not do anything to update the children of the render
  /// object. That should instead be handled by the method that overrides
  /// [RenderObjectElement.update] in the object rendered by this object's
  /// [createElement] method. See, for example,
  /// [SingleChildRenderObjectElement.update].
  @protected
  void updateRenderObject(BuildContext context, covariant RenderObject renderObject) { }

  /// A render object previously associated with this widget has been removed
  /// from the tree. The given [RenderObject] will be of the same type as
  /// returned by this object's [createRenderObject].
  @protected
  void didUnmountRenderObject(covariant RenderObject renderObject) { }
}
```

1、createElement方法用于创建RenderObjectElement元素

2、createRenderObject -- 用于创建RenderObject

3、updateRenderObject -- 更新RenderObject

4、didUnmountRenderObject -- RenderObject从widget树中移除时，参数renderObject和createRenderObject的一致