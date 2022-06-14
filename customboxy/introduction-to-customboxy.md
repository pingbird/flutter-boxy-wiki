# Introduction to CustomBoxy

{% embed url="https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html" %}

[CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) is a widget that uses a delegate to control the layout of multiple children.

This is essentially a more powerful version of [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html) or [CustomPaint](https://api.flutter.dev/flutter/widgets/CustomPaint-class.html), it allows you to inflate, constrain, and lay out each child manually, it also allows its size to depend on the layout of its children.

This is overkill in most cases, before diving in you may want to check if some combination of [Stack](https://api.flutter.dev/flutter/widgets/Stack-class.html), [LayoutBuilder](https://api.flutter.dev/flutter/widgets/LayoutBuilder-class.html), [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html), or [Flow](https://api.flutter.dev/flutter/widgets/Flow-class.html) is more suitable.

### Basics

To start, create a [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) widget and pass it a subclass of [BoxyDelegate](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate-class.html):

![](<../.gitbook/assets/image (1) (1) (1) (1).png>)

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
  // Choose the smallest size that our constraints allow, just like
  // an empty SizedBox().
  Size layout() => constraints.smallest;
}
```

[BoxyDelegate](https://pub.dev/documentation/boxy/latest/boxy/BoxyDelegate-class.html) includes several getters that are useful for layout, including:

* [constraints](https://pub.dev/documentation/boxy/latest/render\_boxy/BoxBoxyDelegateMixin/constraints.html), the [BoxConstraints](https://api.flutter.dev/flutter/rendering/BoxConstraints-class.html) provided by the parent
* [children](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/children.html), a list of [BoxyChild](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild-class.html) instances which let you interact with things passed to [CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html)
* [getChild](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/getChild.html), a method that can get a child by id using [BoxyId](https://pub.dev/documentation/boxy/latest/boxy/BoxyId-class.html)
* [hasChild](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/hasChild.html), a method that returns true if there is a child with a given id
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
* [position](https://pub.dev/documentation/boxy/latest/boxy/BaseBoxyChild/position.html), a method that sets the offset of the child
* [setTransform](https://pub.dev/documentation/boxy/latest/boxy/BaseBoxyChild/setTransform.html), a more advanced version of position that takes a [Matrix4](https://pub.dev/documentation/vector\_math/2.1.2/vector\_math\_64/Matrix4-class.html) transform
* [size](https://pub.dev/documentation/boxy/latest/boxy/BoxyChild/size.html), the size of the child after it's laid out
* [context](https://pub.dev/documentation/boxy/latest/inflating\_element/InflatedChildHandle/context.html), the [Element](https://api.flutter.dev/flutter/widgets/Element-class.html) (aka [BuildContext](https://api.flutter.dev/flutter/widgets/BuildContext-class.html)) of the child
* [id](https://pub.dev/documentation/boxy/latest/inflating\_element/InflatedChildHandle/id.html), the id of the child which is provided by either [BoxyId](https://pub.dev/documentation/boxy/latest/boxy/BoxyId-class.html) or an incrementing integer

### Identifying children

In some cases you might want to identify children by name rather than index, [BoxyId](https://pub.dev/documentation/boxy/latest/boxy/BoxyId-class.html) is your friend:

![](../.gitbook/assets/ftest\_XBEjnnpsdS.png)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: const [
        // BoxyId allows children to be accessed by name.
        BoxyId(
          // This `#hello` is called a Symbol, they are like strings but
          // slightly more performant for naming things.
          id: #hello,
          child: Text('Hello,'),
        ),
        BoxyId(
          id: #world,
          child: Text('World!'),
        ),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {
  @override
  Size layout() {
    // Grab the children by name.
    final BoxyChild hello = getChild(#hello);
    final BoxyChild world = getChild(#world);

    // Lay them out and store their sizes.
    final Size helloSize = hello.layout(constraints);
    final Size worldSize = world.layout(constraints);

    // Position the "World!" text below the "Hello,".
    world.position(Offset(0, helloSize.height));

    // Return the size of our little column.
    return Size(
      max(helloSize.width, worldSize.height),
      helloSize.height + worldSize.height,
    );
  }
}
```

### Widget Inflation

The most powerful feature of Boxy is the ability to [inflate](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/inflate.html) arbitrary widgets at layout time.&#x20;

![](../.gitbook/assets/ftest\_m3xCKjHuvM.png)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: [
        Container(
          color: Colors.blue,
          width: 50,
          height: 50,
        ),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {
  @override
  Size layout() {
    // Lay out the container first.
    final container = children.single;
    final containerSize = container.layout(constraints);

    // Inflate a Text widget based on the containers size.
    final text = inflate(
      Text('^ $containerSize'),
      id: #text,
    );
    final textSize = text.layout(constraints);

    // Position the text below the container.
    text.position(Offset(0, containerSize.height));

    return Size(
      max(containerSize.width, textSize.width),
      containerSize.height + textSize.height,
    );
  }
}
```

### Painting

By overriding [paint](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/paint.html) or [paintForeground](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/paintForeground.html) you can get functionality similar to [CustomPaint](https://api.flutter.dev/flutter/widgets/CustomPaint-class.html):

![](../.gitbook/assets/ftest\_frMkXTvID9.png)

```dart
class MyBoxyDelegate extends BoxyDelegate {
  @override
  Size layout() => const Size(32, 32);

  @override
  void paint() {
    canvas.drawRect(
      Offset.zero & render.size,
      Paint()..color = Colors.blue,
    );
  }
}
```

The [paint](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/paint.html) and [paintForeground](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/paintForeground.html) methods are the same, but [paintForeground](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/paintForeground.html) is called after [paintChildren](https://api.flutter.dev/flutter/rendering/FlowDelegate/paintChildren.html).

### Painting children

We can customize the way children are painted by overriding [paintChildren](https://api.flutter.dev/flutter/rendering/FlowDelegate/paintChildren.html), this is useful if you want to change their paint order for example:

![Without paintChildren](../.gitbook/assets/ftest\_fcR5Z2lEZD.png) ![With paintChildren](<../.gitbook/assets/image (1) (1) (1).png>)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: [
        Container(
          color: Colors.blue,
          width: 48,
          height: 48,
        ),
        Container(
          color: Colors.red,
          width: 96,
          height: 48,
        ),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {
  @override
  void paintChildren() {
    children[1].paint();
    children[0].paint();
  }
}
```

Note that the [canvas](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/canvas.html) is still available, so we can use [paintChildren](https://api.flutter.dev/flutter/rendering/FlowDelegate/paintChildren.html) to paint things between children:

![](<../.gitbook/assets/image (3).png>)

{% code title="" %}
```dart
class MyBoxyDelegate extends BoxyDelegate {
  @override
  void paintChildren() {
    children[1].paint();
    canvas.save();
    canvas.drawCircle(
      // Unlike the paint method, the canvas of paintChildren is not transformed
      // into the local coordinate space, so we need to offset by paintOffset.
      paintOffset + const Offset(48, 24),
      16,
      Paint()..color = Colors.white,
    );
    canvas.restore();
    children[0].paint();
  }
}
```
{% endcode %}

### Layers

Another way we can customize the way children are painted is with [layers](https://pub.dev/documentation/boxy/latest/render\_boxy/BaseBoxyDelegate/layers.html), this is a fancy wrapper around the low level compositing [Layers](https://api.flutter.dev/flutter/rendering/Layer-class.html) used for widgets like [Opacity](https://api.flutter.dev/flutter/widgets/Opacity-class.html) and [BackdropFilter](https://api.flutter.dev/flutter/widgets/BackdropFilter-class.html).

![](<../.gitbook/assets/image (1) (1).png>)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: [
        Container(
          color: Colors.blue,
          width: 48,
          height: 48,
        ),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {
  @override
  void paintChildren() {
    // Make the square blurry
    layers.imageFilter(
      imageFilter: ImageFilter.blur(
        sigmaX: 2,
        sigmaY: 2,
        tileMode: TileMode.decal,
      ),
      paint: children.single.paint,
    );
  }
}
```

### Parent Data

You can pass data to the delegate using the [data](https://pub.dev/documentation/boxy/latest/boxy/BoxyId/data.html) parameter of [BoxyId](https://pub.dev/documentation/boxy/latest/boxy/BoxyId-class.html).

This is the same underlying mechanism that [Expanded](https://api.flutter.dev/flutter/widgets/Expanded-class.html) uses to tell the [Row](https://api.flutter.dev/flutter/widgets/Row-class.html) or [Column](https://api.flutter.dev/flutter/widgets/Column-class.html) how much space it should take up.

![](../.gitbook/assets/image.png)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: MyBoxyDelegate(),
      children: [
        const Text('👻 I am hiding '),
        BoxyId(
          child: Container(
            color: Colors.blue,
            width: 50,
            height: 50,
          ),
          // This gets passed to BoxyChild.parentData
          data: 0.5,
        ),
      ],
    );
  }
}

class MyBoxyDelegate extends BoxyDelegate {
  @override
  void paintChildren() {
    children[0].paint();
    layers.opacity(
      opacity: children[1].parentData,
      paint: children[1].paint,
    );
  }
}
```

