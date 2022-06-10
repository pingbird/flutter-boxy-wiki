# 🔹 Introduction To Layout

When Flutter was created, it set out to provide a render architecture that was simple, performant, and modular. Compare this to [HTML](https://developer.chrome.com/articles/layoutng/) or Android [View](https://developer.android.com/reference/android/view/View)s, which have many complex, slow, and implementation-specific layout algorithms.

Most of the widgets you use in Flutter are actually pretty simple and elegant under the hood, there is rarely any magic, so try not to be intimidated by it!

### Constraints go down, Sizes go up

{% code title="over-simplification" %}
```dart
Size layout(BoxConstraints constraints) {
  final childSize = child.layout(BoxConstraints()));
  child.position(Offset.zero);
  return childSize;
}
```
{% endcode %}

Layout in Flutter happens in a few steps:

1. Pass [BoxConstraints](https://api.flutter.dev/flutter/rendering/BoxConstraints-class.html) to children
2. Lay out children
3. Position children
4. Give self a size

This is called the [RenderBox](https://api.flutter.dev/flutter/rendering/RenderBox-class.html) protocol, and is why animations in Flutter out-perform native Android and iOS.

### Interactive Example

Here's a DartPad that you can play around with:

{% embed url="https://dartpad.dartlang.org/?id=0d12f9092860c0793e57eb3b9cad2926" %}
DartPad - [square-opacity.dart](https://gist.github.com/PixelToast/0d12f9092860c0793e57eb3b9cad2926)
{% endembed %}

It takes a child (hello world text) and sizes itself to be a square, it also animates opacity to show how [Layer](https://api.flutter.dev/flutter/rendering/Layer-class.html)s work.

The actual layout algorithm itself is simple, the hard part is just the boilerplate:

1. Subclass [LeafRenderObjectWidget](https://api.flutter.dev/flutter/widgets/LeafRenderObjectWidget-class.html), [SingleChildRenderObjectWidget](https://api.flutter.dev/flutter/widgets/SingleChildRenderObjectWidget-class.html), or [MultiChildRenderObjectWidget](https://api.flutter.dev/flutter/widgets/MultiChildRenderObjectWidget-class.html)
2. Implement [createRenderObject](https://api.flutter.dev/flutter/widgets/RenderObjectWidget/createRenderObject.html) / [updateRenderObject](https://api.flutter.dev/flutter/widgets/RenderObjectWidget/updateRenderObject.html) to pass variables to your RenderObject
3. Subclass [RenderBox](https://api.flutter.dev/flutter/rendering/RenderBox-class.html)
4. Mix in [RenderObjectWithChildMixin](https://api.flutter.dev/flutter/rendering/RenderObjectWithChildMixin-mixin.html), [ContainerRenderObjectMixin](https://api.flutter.dev/flutter/rendering/ContainerRenderObjectMixin-mixin.html), or [SlottedContainerRenderObjectMixin](https://api.flutter.dev/flutter/widgets/SlottedContainerRenderObjectMixin-mixin.html)
5. Implement setters to check and call [markNeedsPaint](https://api.flutter.dev/flutter/rendering/RenderObject/markNeedsPaint.html) or [markNeedsLayout](https://api.flutter.dev/flutter/rendering/RenderObject/markNeedsLayout.html)
6. Implement [performLayout](https://api.flutter.dev/flutter/rendering/RenderBox/performLayout.html)
7. Implement [paint](https://api.flutter.dev/flutter/rendering/RenderObject/paint.html)

The original goal of Boxy was to boil this down into a simple, intuitive delegate class, sort of like a [CustomPainter](https://api.flutter.dev/flutter/rendering/CustomPainter-class.html) on steroids.

### Resources

&#x20;Here are some good resources on layout:

{% embed url="https://docs.flutter.dev/development/ui/layout/constraints" %}

{% embed url="https://docs.flutter.dev/development/ui/widgets/layout" %}

{% embed url="https://docs.flutter.dev/development/ui/layout" %}

{% embed url="https://www.youtube.com/watch?v=zmbmrw07qBc" %}

{% embed url="https://flutter.megathink.com/data-model/widgets" %}
