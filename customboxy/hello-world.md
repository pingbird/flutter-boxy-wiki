# Hello, World!

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
