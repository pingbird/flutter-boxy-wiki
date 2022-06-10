# Introduction To Layout

When Flutter was created, it set out to provide a render architecture that was simple, performant, and modular. Compare this to [HTML](https://developer.chrome.com/articles/layoutng/) or Android [View](https://developer.android.com/reference/android/view/View)s, which have many complex, slow, and implementation-specific layout algorithms.

Most of the widgets you use in Flutter are actually pretty simple and elegant under the hood, there is rarely any magic, so try not to be intimidated by it!

### The many trees

Before touching layout, we first need to understand the relationship between the Widget, Element, and Render trees.

![](.gitbook/assets/trees.png)

If you have some experience in Flutter you are probably familiar with how State works, it's persistent and has lifecycle hooks that tell you when to initialize, build, and dispose.

State is just a fancy delegate for ComponentElement so that Flutter doesn't have to expose its ugly internals to widget code, other than that Elements and States are essentially the same thing!



{% hint style="info" %}
There technically is no "widget tree" since widgets are more of a user-defined data structure that does not share a root node. The term is often conflated with the element tree, since elements form a real tree and provide a context.
{% endhint %}

### Layout

You can think of layout in Flutter as a bunch of functions that take in BoxConstraints and return a Size:

{% code title="(oversimplification)" %}
```dart
layout(BoxConstraints constraints) {
  final childSize = child.layout(BoxConstraints()));
  child.position(Offset.zero);
  return childSize;
}
```
{% endcode %}

This is called the [RenderBox](https://api.flutter.dev/flutter/rendering/RenderBox-class.html) protocol, it's simplicity is what enables animations in Flutter to outperform native Android and iOS.

The downside of this simplicity, however, is that developers have to put in a little extra effort to constrain widgets and avoid those pesky flex overflow, unbounded constraints, and infinite size errors. The Flutter team made a great article on the design philosophy and performance implications of the RenderObject model: [https://docs.flutter.dev/resources/inside-flutter](https://docs.flutter.dev/resources/inside-flutter)



### Interactive Example

![](<.gitbook/assets/image (2).png>)

Here's a DartPad that you can play around with:

{% embed url="https://dartpad.dartlang.org/?id=0d12f9092860c0793e57eb3b9cad2926" %}
DartPad - [square-opacity.dart](https://gist.github.com/PixelToast/0d12f9092860c0793e57eb3b9cad2926)
{% endembed %}

It takes a child (Hello, World!) and ensures the height equals the width, the custom RenderObject also animates opacity to show how [Layer](https://api.flutter.dev/flutter/rendering/Layer-class.html)s work.

The actual layout algorithm itself is quite simple, the annoying part is just the amount of boilerplate:

1. Subclass [LeafRenderObjectWidget](https://api.flutter.dev/flutter/widgets/LeafRenderObjectWidget-class.html), [SingleChildRenderObjectWidget](https://api.flutter.dev/flutter/widgets/SingleChildRenderObjectWidget-class.html), or [MultiChildRenderObjectWidget](https://api.flutter.dev/flutter/widgets/MultiChildRenderObjectWidget-class.html)
2. Implement [createRenderObject](https://api.flutter.dev/flutter/widgets/RenderObjectWidget/createRenderObject.html) / [updateRenderObject](https://api.flutter.dev/flutter/widgets/RenderObjectWidget/updateRenderObject.html) to pass variables to your RenderObject
3. Subclass [RenderBox](https://api.flutter.dev/flutter/rendering/RenderBox-class.html)
4. Mix in [RenderObjectWithChildMixin](https://api.flutter.dev/flutter/rendering/RenderObjectWithChildMixin-mixin.html), [ContainerRenderObjectMixin](https://api.flutter.dev/flutter/rendering/ContainerRenderObjectMixin-mixin.html), or [SlottedContainerRenderObjectMixin](https://api.flutter.dev/flutter/widgets/SlottedContainerRenderObjectMixin-mixin.html)
5. Implement setters to check and call [markNeedsPaint](https://api.flutter.dev/flutter/rendering/RenderObject/markNeedsPaint.html) or [markNeedsLayout](https://api.flutter.dev/flutter/rendering/RenderObject/markNeedsLayout.html)
6. Implement [performLayout](https://api.flutter.dev/flutter/rendering/RenderBox/performLayout.html)
7. Implement [paint](https://api.flutter.dev/flutter/rendering/RenderObject/paint.html)

The original goal of Boxy was to boil this down into a simple, intuitive delegate class, sort of like a [CustomPainter](https://api.flutter.dev/flutter/rendering/CustomPainter-class.html) on steroids.

Check out the introduction to CustomBoxy next:

{% content-ref url="customboxy/introduction-to-customboxy.md" %}
[introduction-to-customboxy.md](customboxy/introduction-to-customboxy.md)
{% endcontent-ref %}

### Learn More

{% embed url="https://docs.flutter.dev/development/ui/layout/constraints" %}

{% embed url="https://docs.flutter.dev/development/ui/widgets/layout" %}

{% embed url="https://docs.flutter.dev/development/ui/layout" %}

{% embed url="https://www.youtube.com/watch?v=zmbmrw07qBc" %}

{% embed url="https://flutter.megathink.com/data-model/widgets" %}
