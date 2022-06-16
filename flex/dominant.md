# Dominant



![](<../.gitbook/assets/ftest\_nmZYWRSqsy (1).png>)

```dart
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      width: 300,
      child: BoxyRow(
        mainAxisSize: MainAxisSize.min,
        children: [
          Container(
            color: Colors.blue,
            child: const RotatedBox(
              quarterTurns: -1,
              child: Text('Chapter 1'),
            ),
            padding: const EdgeInsets.all(2.0),
          ),
          const SizedBox(width: 8.0),
          const Dominant.expanded(
            child: Text(
              'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed '
              'do eiusmod tempor incididunt ut labore et dolore magna '
              'aliqua. Ut enim ad minim veniam, quis nostrud exercitation '
              'ullamco laboris nisi ut aliquip ex ea commodo consequat.',
            ),
          ),
        ],
      ),
    );
  }
}
```
