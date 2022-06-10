---
coverY: 0
layout: landing
---

# Welcome



![](<.gitbook/assets/zncIM (1).png>)

Boxy is a Flutter package created to overcome the limitations of built-in layout widgets, it provides utilities for flex, custom multi-child layouts, dynamic widget inflation, slivers, and more!

The package is ready for production use, it features:

* Excellent documentation
* Good test coverage
* Passes strict analysis

### Features

{% content-ref url="customboxy/customboxy.md" %}
[customboxy.md](customboxy/customboxy.md)
{% endcontent-ref %}

### Getting Started

To install Boxy, add it to your dependencies in `pubspec.yaml`:

```yaml
dependencies:
  boxy: ^2.0.5+1
```

Alternatively, you can also use the pub command:

```
flutter pub add boxy
```

After installing the package and running `flutter pub get`, import one of the following top-level libraries:

```dart
import 'package:boxy/boxy.dart';
import 'package:boxy/flex.dart';
import 'package:boxy/padding.dart';
import 'package:boxy/slivers.dart';
import 'package:boxy/utils.dart';
```
