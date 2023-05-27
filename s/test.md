---
layout: share
title: Share Test
include_in_header: false
---

<div markdown="1" class="msg-item user">

<img class='avatar' src="/assets/ic_avatar_user.svg" alt="Avatar of user"/>

<div markdown="1" class="msg-content">

在 flutter 中使用 getx 的路由框架，如何监听 hero 动画

- test1
- test2

</div>

</div>

<div markdown="1" class="msg-item assistant">

<img class='avatar' src="/assets/ic_avatar_ai.svg" alt="Avatar of Q.ai"/>

<div markdown="1" class="msg-content">

在 Flutter 中，如果希望在使用 GetX 路由框架时监听 Hero 动画，可以使用以下方法：

首先，在 pubspec.yaml 文件中添加 GetX 依赖：

```
dependencies:
  get: ^4.3.8
```

然后，在你的代码中进行以下步骤：

1. 在项目中导入 GetX：

```dart
import 'package:get/get.dart';
```

2. 使用 GetMaterialApp 作为你的项目主入口：

```dart
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}
```

3. 在项目页面添加 Hero 动画。（以下示例假设存在两个页面，一个是 MyHomePage，另一个是 DestinationPage，其中有 Hero 动画。）

MyHomePage（源页面）：

```dart
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Home Page"),
      ),
      body: Center(
        child: Hero(
          tag: 'hero_example', // 确保源和目标页面的 Hero 元素具有相同的标签
          child: InkWell(
            onTap: () {
              // 在这里使用 GetX 进行页面跳转
              Get.to(DestinationPage());
            },
            child: Container(
              width: 100,
              height: 100,
              color: Colors.red,
            ),
          ),
        ),
      ),
    );
  }
}
```

DestinationPage（目标页面）：

```dart
class DestinationPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Destination Page"),
      ),
      body: Center(
        child: Hero(
          tag: 'hero_example', // 确保源和目标页面的 Hero 元素具有相同的标签
          child: Container(
            width: 200,
            height: 200,
            color: Colors.red,
          ),
        ),
      ),
    );
  }
}
```

4. 为了监听 Hero 动画，您可以创建一个自定义的 PageRoute，并覆盖`buildTransitions()`方法以执行所需的操作。

```dart
class CustomPageRoute<T> extends GetPageRoute<T> {
  CustomPageRoute({
    required WidgetBuilder page,
    String? tag,
    RouteSettings? settings,

    // 其他属性
  }) : super(
          page: page,
          tag: tag,
          settings: settings,

          // 其他属性
        );

  @override
  Widget buildTransitions(BuildContext context, Animation<double> animation,
      Animation<double> secondaryAnimation, Widget child) {

    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        print("Hero Animation Completed");
      }
    });

    // 这里可以根据需要返回 Hero 动画的其他自定义包装
    return super.buildTransitions(
        context, animation, secondaryAnimation, child);
  }
}
```

5. 使用 CustomPageRoute 进行页面跳转：

```dart
Get.to(() => DestinationPage(), transition: Transition.custom, customTransition: CustomPageRoute());
```

现在，每当 Hero 动画完成时，都会输出 "Hero Animation Completed"。您可以在`buildTransitions()`方法中根据需要对 Hero 动画进行其他操作。

</div>

</div>
