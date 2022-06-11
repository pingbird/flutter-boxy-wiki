# Example: Evenly Sized Row

### Original Layout

![](../.gitbook/assets/ftest\_4KetSQxXfe.png)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Material(
          borderRadius: const BorderRadius.horizontal(
            left: Radius.circular(100),
          ),
          color: Colors.blue,
          child: Container(
            padding: const EdgeInsets.all(4.0),
            alignment: Alignment.center,
            child: const Text('Thing', textAlign: TextAlign.center),
          ),
        ),
        Material(
          color: Colors.purple,
          child: Container(
            padding: const EdgeInsets.all(4.0),
            alignment: Alignment.center,
            child: const Text(
              'Other thing',
              textAlign: TextAlign.center,
            ),
          ),
        ),
        Material(
          borderRadius: const BorderRadius.horizontal(
            right: Radius.circular(100),
          ),
          color: Colors.pink,
          child: Container(
            padding: const EdgeInsets.all(4.0),
            alignment: Alignment.center,
            child: const Text('Other other thing', textAlign: TextAlign.center),
          ),
        ),
      ],
    );
  }
}
```

### Custom Layout

![](../.gitbook/assets/ftest\_2ETeGIqwH8.png)

```dart
class EvenSized extends StatelessWidget {
  const EvenSized({
    Key? key,
    required this.children,
  }) : super(key: key);

  final List<Widget> children;

  @override
  Widget build(BuildContext context) {
    return CustomBoxy(
      delegate: EvenSizedBoxy(),
      children: children,
    );
  }
}

class EvenSizedBoxy extends BoxyDelegate {
  @override
  Size layout() {
    // Find the max intrinsic width
    var childWidth = 0.0;
    for (final child in children) {
      childWidth = max(
        childWidth,
        child.render.getMaxIntrinsicWidth(double.infinity),
      );
    }

    // Clamp the width so we don't overflow
    childWidth = min(childWidth, constraints.maxWidth / children.length);

    // Find the max intrinsic height
    var childHeight = 0.0;
    for (final child in children) {
      childHeight = max(
        childHeight,
        child.render.getMinIntrinsicHeight(double.infinity),
      );
    }

    // Force each child to be the same size as the biggest
    final childConstraints = BoxConstraints.tight(
      Size(childWidth, childHeight),
    );

    var x = 0.0;
    for (final child in children) {
      child.layout(childConstraints);

      // Space them out evenly
      child.position(Offset(x, 0));
      x += childWidth;
    }

    return Size(childWidth * children.length, childHeight);
  }
}
```

![](../.gitbook/assets/ftest\_pqqiRVzZwz.png)
