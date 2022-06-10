# Introduction to CustomBoxy

{% embed url="https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html" %}

[CustomBoxy](https://pub.dev/documentation/boxy/latest/boxy/CustomBoxy-class.html) is a widget that uses a delegate to control the layout of multiple children.

This is essentially a more powerful version of [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html) or [CustomPaint](https://api.flutter.dev/flutter/widgets/CustomPaint-class.html), it allows you to inflate, constrain, and lay out each child manually, it also allows its size to depend on the layout of its children.

This is overkill in most cases, check if some combination of [Stack](https://api.flutter.dev/flutter/widgets/Stack-class.html), [LayoutBuilder](https://api.flutter.dev/flutter/widgets/LayoutBuilder-class.html), [CustomMultiChildLayout](https://api.flutter.dev/flutter/widgets/CustomMultiChildLayout-class.html), or [Flow](https://api.flutter.dev/flutter/widgets/Flow-class.html) is more suitable.
