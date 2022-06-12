# Introduction to CustomBoxy

{% embed url="https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html" %}

[CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) is a widget that uses a delegate to control the layout of multiple children.

This is essentially a more powerful version of [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html) or [CustomPaint](https://api.flutter.dev/flutter/widgets/CustomPaint-class.html), it allows you to inflate, constrain, and lay out each child manually, it also allows its size to depend on the layout of its children.

This is overkill in most cases, before diving in you may want to check if some combination of [Stack](https://api.flutter.dev/flutter/widgets/Stack-class.html), [LayoutBuilder](https://api.flutter.dev/flutter/widgets/LayoutBuilder-class.html), [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html), or [Flow](https://api.flutter.dev/flutter/widgets/Flow-class.html) is more suitable.

### Basics

To start, create a [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) widget and pass it a subclass of [BoxyDelegate](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate-class.html):

![](<../.gitbook/assets/image (1).png>)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: const [
        Text('Hello, World!'),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {}
```

[CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) accepts a list of children, it's default behavior is similar to a [Stack](https://api.flutter.dev/flutter/widgets/Stack-class.html), passing through the constraints to each child and sizing itself to the biggest one.

There are many methods you can override in [BoxyDelegate](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate-class.html), the most common being [layout](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate/layout.html).

### Customizing Layout

To customize layout, override the [layout](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate/layout.html) method to return a size:

```dart
class MyBoxyDelegate extends BoxyDelegate {
  @override
  Size layout() => constraints.smallest;
}
```

This boxy would have no children and would choose the smallest size that its constraints allow, just like an empty `SizedBox()`.

[BoxyDelegate](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate-class.html) includes several getters that are useful for layout, including:

* [constraints](https://pub.dev/documentation/boxy/latest/render\_boxy/BoxBoxyDelegateMixin/constraints.html), the [BoxConstraints](https://api.flutter.dev/flutter/rendering/BoxConstraints-class.html) provided by the parent
* [children](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/children.html), a list of [BoxyChild](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild-class.html) instances which let you interact with things passed to [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html)
* [layoutData](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/layoutData.html), a variable you can use to hold additional data created during layout
* [render](https://pub.dev/documentation/boxy/latest/render\_boxy/BoxBoxyDelegateMixin/render.html), a raw reference to the [RenderBoxy](https://pub.dev/documentation/boxy/latest/render\_boxy/RenderBoxy-class.html) of [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html)
* [buildContext](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/buildContext.html), the [BuildContext](https://api.flutter.dev/flutter/widgets/BuildContext-class.html) of the [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html).

### Laying out children

To lay out child widgets we first grab a [BoxyChild](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild-class.html) instance, call layout, and optionally position it.

{% code title="" %}
```dart
@override
Size layout() {
  // Find our children, in this case we assume there is exactly one
  final BoxyChild child = children.single;

  // Call layout on the child to obtain their size, we just pass the
  // constraints given to the CustomBoxy by its parent
  final Size size = child.layout(constraints);

  // Optionally position the child
  child.position(Offset.zero);

  // Return a size
  return size;
}
```
{% endcode %}

[BoxyChild](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild-class.html) is just a fancy wrapper around [RenderBox](https://api.flutter.dev/flutter/rendering/RenderBox-class.html), it has a bunch of methods and properties that are useful for layout:

* [layout](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild/layout.html), a method that lays out the child given some constraints
* [layoutFit](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild/layoutFit.html), a method that lays out and transforms the child according to a [Rect](https://api.dart.dev/stable/2.17.3/dart-ui/Rect-class.html) and [BoxFit](https://api.flutter.dev/flutter/painting/BoxFit.html), just like a [FittedBox](https://api.flutter.dev/flutter/widgets/FittedBox-class.html)
* [layoutRect](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild/layoutRect.html), a method that lays out and positions the child so that it fits a [Rect](https://api.dart.dev/stable/2.17.3/dart-ui/Rect-class.html) with an optional alignment property that behaves like an [Align](https://api.flutter.dev/flutter/widgets/Align-class.html)
* [size](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild/size.html), the size of the child after the child is laid out
* [context](https://pub.dev/documentation/boxy/latest/inflating\_element/InflatedChildHandle/context.html), the [Element](https://api.flutter.dev/flutter/widgets/Element-class.html) (aka [BuildContext](https://api.flutter.dev/flutter/widgets/BuildContext-class.html)) of the child
* [id](https://pub.dev/documentation/boxy/latest/inflating\_element/InflatedChildHandle/id.html), the id of the child which is provided by either [BoxyId](https://pub.dev/documentation/boxy/latest/boxy/BoxyId-class.html) or an incrementing integer

&#x20;
