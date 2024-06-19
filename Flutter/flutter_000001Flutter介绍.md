---
tags: flutter
---

# Flutter介绍

## Widget

* `StatelessWidget`：无状态widget从它们的父widget接收参数， 它们被存储在`final`型的成员变量中。 当一个widget被要求构建时，它使用这些存储的值作为参数来构建widget。
* `StatefulWidget`：StatefulWidgets是特殊的widget，它知道如何生成State对象，然后用它来保持状态。

## Hello World

<div style="display: flex;flex-direction: row;width: 100%;">
<div style="width: 60%;">
<pre>
<code class="language-dart">
import 'package:flutter/material.dart';
void main() {
  runApp(
    const Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}
</code>
</pre>
</div>
<div style="width: 40%;padding: 10px;display: flex;flex-direction: row;justify-content: center;">
<img src="file:///Users/yinzhongzheng/Library/Mobile Documents/iCloud~md~obsidian/Documents/Develop/assets/imgs/flutter/flutter_g_001.png" />
</div>
</div>

## 基础Widget布局

确保在**pubspec.yaml**文件中，将`flutter`的值设置为：`uses-material-design: true`。这允许我们可以使用一组预定义[Material icons](https://design.google.com/icons/)。

```
name: my_app
flutter:
  uses-material-design: true
```

为了继承主题数据，widget需要位于[`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html)内才能正常显示， 因此我们使用[`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html)来运行该应用。

<div style="display: flex;flex-direction: row;width: 100%;">
<div style="width: 60%;">
<pre>
<code class="language-dart">
import 'package:flutter/material.dart';
void main(List<String> args) {
  runApp(const MaterialApp(
    title: 'My App',
    home: SafeArea(
      child: MyScaffold(),
    ),
  ));
}
class MyAppBar extends StatelessWidget {
  const MyAppBar({required this.title, super.key});
  final Widget title;
  @override
  Widget build(BuildContext context) {
    return Container(
      height: 56.0,
      padding: const EdgeInsets.symmetric(horizontal: 8.0),
      decoration: BoxDecoration(color: Colors.blue[500]),
      child: Row(
        children: [
          const IconButton(
              onPressed: null,
              icon: Icon(Icons.menu),
              tooltip: 'Navigation menu'),
          Expanded(child: title),
          const IconButton(
            onPressed: null,
            icon: Icon(Icons.search),
            tooltip: 'Search',
          )
        ],
      ),
    );
  }
}
class MyScaffold extends StatelessWidget {
  const MyScaffold({super.key});
  @override
  Widget build(BuildContext context) {
    return Material(
      child: Column(children: [
        MyAppBar(
            title: Text(
          'Example title',
          style: Theme.of(context).primaryTextTheme.headline6,
        )),
        const Expanded(
            child: Center(
          child: Center(
            child: Text('Hello, world!'),
          ),
        ))
      ]),
    );
  }
}
</code>
</pre>
</div>
<div style="width: 40%;padding: 10px;display: flex;flex-direction: row;justify-content: center;">
<img src="file:///Users/yinzhongzheng/Library/Mobile Documents/iCloud~md~obsidian/Documents/Develop/assets/imgs/flutter/flutter_g_003.png" style="width: 280px;height: 600px" />
</div>
</div>

## 使用Material组件

Flutter提供了许多widgets，可帮助您构建遵循Material Design的应用程序。Material应用程序以[`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html) widget开始， 该widget在应用程序的根部创建了一些有用的widget，其中包括一个[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)， 它管理由字符串标识的Widget栈（即页面路由栈）。[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)可以让您的应用程序在页面之间的平滑的过渡。 是否使用[`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html)完全是可选的，但是使用它是一个很好的做法。

<div style="display: flex;flex-direction: row;width: 100%;">
<div style="width: 60%;">
<pre>
<code class="language-dart">
import 'package:flutter/material.dart';
void main(List<String> args) {
  runApp(const MaterialApp(
    title: 'Flutter Tutorial',
    home: TutorialHome(),
  ));
}
class TutorialHome extends StatelessWidget {
  const TutorialHome({super.key});
  @override
  Widget build(BuildContext context) {
    // Scaffold是Material中主要的布局组件.
    return Scaffold(
      appBar: AppBar(
        leading: const IconButton(
          onPressed: null,
          icon: Icon(Icons.menu),
          tooltip: "Navigation Menu",
        ),
        title: const Text('Example title'),
        actions: const [
          IconButton(
            onPressed: null,
            icon: Icon(Icons.search),
            tooltip: "Search",
          )
        ],
      ),
      // body占屏幕的大部分
      body: const Center(
        child: Text("Hello, World!"),
      ),
      floatingActionButton: const FloatingActionButton(
        onPressed: null,
        tooltip: "Add",
        child: Icon(Icons.add),
      ),
    );
  }
}
</code>
</pre>
</div>
<div style="width: 40%;padding: 10px;display: flex;flex-direction: row;justify-content: center;">
<img src="file:///Users/yinzhongzheng/Library/Mobile Documents/iCloud~md~obsidian/Documents/Develop/assets/imgs/flutter/flutter_g_004.png" style="width: 280px;height: 600px" />
</div>
</div>

## 手势处理

```dart
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(const MaterialApp(
    title: 'Flutter Tutorial',
    home: Scaffold(
      body: Center(
        child: MyButton(),
      ),
    ),
  ));
}

class MyButton extends StatelessWidget {
  const MyButton({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print("MyButton was tapped!");
      },
      child: Container(
        height: 50.0,
        padding: const EdgeInsets.all(8.0),
        margin: const EdgeInsets.symmetric(horizontal: 8.0),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(5.0),
          color: Colors.lightGreen[500],
        ),
        child: const Center(
          child: Text("Engage"),
        ),
      ),
    );
  }
}
```

该[`GestureDetector`](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html) widget并不具有显示效果，而是检测由用户做出的手势。 当用户点击[`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)时， [`GestureDetector`](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html)会调用它的[`onTap`](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html#onTap)回调， 在回调中，将消息打印到控制台。

## 根据用户输入改变widget

> ⚠为什么`StatefulWidget`和`State`是单独的对象。
> 
> 在Flutter中，这两种类型的对象具有**不同的生命周期**： `Widget`是**临时对象**，用于构建当前状态下的应用程序，而`State`对象在多次调用`build()`之间**保持不变**，允许它们记住信息(状态)。

```dart
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(const MaterialApp(
    title: 'Flutter Tutorial',
    home: Scaffold(
      body: Center(
        child: Counter(),
      ),
    ),
  ));
}

class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        ElevatedButton(onPressed: _increment, child: const Text("Increment")),
        const SizedBox(
          width: 16,
        ),
        Text('Count: $_counter'),
      ],
    );
  }
}
```

在Flutter中，事件流是**“向上”**传递的，而状态流是**“向下”**传递的，重定向这一流程的共同父元素是`State`。

```dart
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(const MaterialApp(
    title: 'Flutter Tutorial',
    home: Scaffold(
      body: Center(
        child: Counter(),
      ),
    ),
  ));
}

class CounterDisplay extends StatelessWidget {
  const CounterDisplay({required this.count, super.key});

  final int count;

  @override
  Widget build(BuildContext context) {
    return Text('Count: $count');
  }
}

class CounterIncrementor extends StatelessWidget {
  const CounterIncrementor({required this.onPressed, super.key});

  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(onPressed: onPressed, child: const Text("Increment"));
  }
}

class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        CounterIncrementor(onPressed: _increment),
        const SizedBox(
          width: 16,
        ),
        CounterDisplay(count: _counter),
      ],
    );
  }
}
```










