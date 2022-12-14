---
layout: editorial
---

# Welcome

![](zncIM%20(1).png)

Boxy is a Flutter package created to overcome the limitations of built-in layout widgets, it provides utilities for flex, custom multi-child layouts, dynamic widget inflation, slivers, and more!

The package is ready for production use, it has excellent documentation, test coverage, and passes strict analysis.

| [🐙 GitHub](https://github.com/PixelToast/flutter-boxy) | [📖 API Docs](https://pub.dev/documentation/boxy/latest/) | [🐦 Pub](https://pub.dev/packages/boxy) | [🗪 Discord](https://discord.com/invite/N7Yshp4) |
| ------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------- | ------------------------------------------------ |

### Content

{% content-ref url="renderobjects/introduction-to-layout.md" %}
[introduction-to-layout.md](introduction-to-layout.md)
{% endcontent-ref %}

{% content-ref url="customboxy/introduction-to-customboxy.md" %}
[introduction-to-customboxy.md](introduction-to-customboxy.md)
{% endcontent-ref %}

{% content-ref url="slivers/slivercontainer.md" %}
[slivercontainer.md](slivercontainer.md)
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
