
## Table of Contents
1. [Flame] ()
	1. [File Structure]
	2. [Game Widget]
	3. [Game Loop]
	4. [Components](##1.4.\space Components)



## 1. Flame
### 1.1. File Structure





### 1.2. Game Widget










### 1.3. Game Loop
#### 1.3.1. Resizing


#### 1.3.2. Lifecycle

![[Screenshot 2024-01-10 at 20.44.12.png]]

#### 1.3.6. Low-level Game API

![[Screenshot 2024-01-10 at 20.44.47.png]]






### 1.4. Components

#### 1.4.1. Component
![[Screenshot 2024-01-03 at 14.07.35.png]]

这个图中包含了Flame中常见的components以及它们之间的在整体架构上的关系。<span style="color:cyan">不难看出他们都继承自同一个祖先<code>Component</code>这个abstract class</span>。<span style="color:cyan; font-weight:600">所有任何Component都可以作为其他Components的子类。声明父子关系可以通过<code>add(Component c)</code>来实现；也可以直接在constructor中添加</span>。例如：
```dart
void main() {
	// 直接在constructor中添加：
	final component1 = Component(children: [Component(), Component()]);

	// 通过add() 方法添加：
	final component2 = Component();
	component2.add(Component());

	// 通过addAll() 方法一次性添加多个子Components：
	component2.addAll([Component(), Component()]);
}
```

##### Component lifecycle
> [😼Yinghai Yu] 这里的生命周期非常像React中组件（component）的生命周期，它们所遵循的思想是非常相像或者完全相同的

![[Screenshot 2024-01-03 at 14.20.44.png]]

* `onGameResize` - 只要游戏画面大小有了变化，就会调用`onGameResize`方法。此外，当该方法被`add()`到组件树🌲(Component tree)上的时候，也会被调用。
* `onParentResize` - `onParentResize`和`onGameResize`方法类似，也是会在该方法被mount到组件树🌲上的时候被调用；以及任何当父组件大小发生变化的时候会被调用。
* `onRemove` - 该方法可被override，以便在组件从游戏中移除之前运行代码，即使组件通过使用父代移除方法和组件移除方法移除，该方法也只运行一次。
* `onLoad` - 可以重载 `onLoad` 方法来运行组件的异步初始化代码，例如加载图片。该方法在 `onGameResize` 之后、`onMount` 之前执行。该方法保证在组件生命周期内只执行一次，因此可以将其视为 "异步构造函数"。
* `onMount` - 该方法在每次将组件挂载到游戏树时都会运行。这意味着您不应在此初始化后期的最终变量，因为该方法可能会在组件的整个生命周期中运行多次。该方法只有在父节点已经加载的情况下才会运行。如果父对象尚未安装，则该方法将在队列中等待（这对游戏引擎的其他部分没有影响）。
* `onChildrenChanged` - 如果需要检测父代子代的变化，可以重载 `onChildrenChanged` 方法。每当一个子代被添加到父代或从父代中移除（包括子代正在更改其父代）时，该方法就会被调用。它的参数包含目标子代及其变化类型（添加或移除）。
##### Priority
在 Flame 中，每个组件都有 `int priority` 属性，该属性决定了组件在其父代子代中的排序顺序。在其他语言和框架中，这有时被称为 `z-index`。优先级设置得越高，该组件在屏幕上显示的位置就越靠前，因为它会被渲染到在它之前渲染的优先级较低的组件之上。
> [🤔Yinghai Yu] 将游戏画面想象成一个三维的结构，越靠上的部分越容易被玩家看到。所谓的**上下**就是这里的`z-index`，也就是优先级了。
用法：
```dart
class MyGame extends FlameGame {
	@override
	void onLoad() {
		final myComponent = PositionComponent(priority: 6);
		add(myComponent);
	}
}
```
如果想要改变某个Component的优先级，可以这样写： `myComponent.priority = 2;`，**这可以让`myComponent`的优先级在当前帧被更新。**

这是另外一种更新优先级的方法：
```dart
class MyComponent extends PositionComponent with TapCallbacks {

  MyComponent() : super(priority: 1);

  @override
  void onTapDown(TapDownEvent event) {
    priority = 2;
  }
}
```

##### Composability of components
有时，将其他组件封装在您的组件内也很有用。例如，通过层次结构对可视化组件进行分组。您可以通过向任何组件（例如 `PositionComponent`）添加子组件来实现这一目的。

当一个组件上有子组件时，每次父组件更新和渲染时，所有子组件都会以相同的条件渲染和更新。

使用示例：通过包装器处理两个组件的可见性：
```dart
class GameOverPanel extends PositionComponent {
  bool visible = false;
  final Image spriteImage;

  GameOverPanel(this.spriteImage);

  @override
  void onLoad() {
    final gameOverText = GameOverText(spriteImage); // GameOverText is a Component
    final gameOverButton = GameOverButton(spriteImage); // GameOverRestart is a SpriteComponent

    add(gameOverText);
    add(gameOverButton);
  }

  @override
  void render(Canvas canvas) {
    if (visible) {
    } // If not visible none of the children will be rendered
  }
}
```

向组件添加子组件有两种方法。首先是 `add()`、`addAll()` 和 `addToParent()`，这些方法可在游戏过程中随时使用。传统上，子组件会在组件的 `onLoad()` 方法中创建和添加，但在游戏过程中添加新的子组件也很常见。

第二种方法是在组件的构造函数中使用 `children`: 参数。这种方法更接近标准的 Flutter API：
```dart
class MyGame extends FlameGame {
  @override
  void onLoad() {
    add(
      PositionComponent(
        position: Vector2(30, 0),
        children: [
          HighScoreDisplay(),
          HitPointsDisplay(),
          FpsComponent(),
        ],
      ),
    );
  }
}
```
这两种方法可以自由组合：首先添加构造函数中指定的子组件，然后再添加其他子组件。

需要注意的是，通过这两种方法添加的子组件都只能保证在加载和安装后最终可用。我们只能保证它们会按照计划添加的顺序出现在子组件列表中。

##### Access to the `World` from a `Component`
如果一个组件的祖先是一个 `World`，并需要访问该 `World` 对象，则可以使用 `HasWorldReference` mixin。
```dart
class MyComponent extends Component with HasWorldReference<MyWorld>,
    TapCallbacks {
  @override
  void onTapDown(TapDownEvent info) {
    // world is of type MyWorld
    world.add(AnotherComponent());
  }
}
```
> [!Yinghai😈] 
> 解释一下`extends`, `with`, 和`implements`之间的区别。
> `extends` -> `class C2 extends C1`用来表示：`C2`是`C1`的子类subclass，如果有`C2`需要继承`C1`的成员变量，则需要`super`下来
> `implements` -> `class C2 implements C1`。这里的`C1`通常是一个接口interface（虽然dart中没有接口这个东西）。
> `with` -> 着就要先讲一下`Mixin`，这是一种用来在多个类结构中 reuse  类中的方法的 方式。可以理解成abstract class。`Mixin`**是抽象和重用一系列操作和状态的一种方式。它类似于扩展类的重用，但不是多重继承。超类仍然只有一个**。`with` is used to include `Mixins`. A mixin is a different type of structure, which can only be used with the keyword `with`.
```dart
// mixin with name First
mixin First {
	void firstFunc(){
		print('hello');
	}
}
// mixin with name temp
mixin temp { 
	void number(){
		print(10);
	}
}
// mixin type used with keyword
class Second with First, temp{ 
	@override
	void firstFunc(){
		print('can override if needed');
	}
}
void main(){
	var second = Second();
	second.firstFunc();
	second.number();
}
```


##### Ensuring a component has a given parent
【省略】

##### Ensuring a component has a given ancestor
【省略】


##### Component Keys
是组件的ID，类似于组件的身份证。

##### Querying child components
【省略】


##### Querying components at a specific point on the screen
通过 `componentsAtPoint()` 方法，可以检查在屏幕上的某个点渲染了哪些组件。返回值是组件的可迭代值，但您也可以通过提供一个可写的 `List<Vector2>` 作为第二个参数，获取每个组件本地坐标空间中初始点的坐标。

可迭代以前后顺序检索组件，即先检索前面的组件，再检索后面的组件。

该方法只能返回实现了 `containsLocalPoint()` 方法的组件。`PositionComponent`（Flame 中许多组件的基类）提供了这样的实现。不过，如果你要定义一个派生自 `Component` 的自定义类，就必须自己实现 `containsLocalPoint()` 方法。

下面是一个如何使用 `componentsAtPoint()` 的示例
```dart
void onDragUpdate(DragUpdateInfo info) {
  game.componentsAtPoint(info.widget).forEach((component) {
    if (component is DropTarget) {
      component.highlight();
    }
  });
}
```


##### PositionType
> [!注意]
> 如果你用的是`CameraComponent`，那就不应该用`PositionType`，而是直接将组件添加到`viewport`，例如，如果您想将它们用作 HUD。

如果您想创建一个 HUD（平视显示器）或其他不按游戏坐标定位的组件，您可以更改组件的 `PositionType`。默认的 `PositionType` 是 `positionType = PositionType.game`，也可以根据组件的定位方式更改为 `PositionType.viewport`或 `PositionType.widget`。
* `PositionType.game` （默认）- 尊重摄像机和视口。
* `PositionType.viewport` - 仅尊重视口（忽略摄像头）。
* `PositionType.widget` - 相对于 Flutter 游戏部件（即原始画布）坐标系的位置。

您的大部分组件可能都会根据 `PositionType.game` 定位，因为您希望它们尊重`Camera`和`Viewport`。但在很多情况下，例如无论您是否移动摄像机，您都希望按钮和文本始终显示在屏幕上，这时您就需要使用 `PositionType.viewport`。在某些罕见的情况下，如果您不想让组件尊重摄像机或视口，那么您就需要使用 `PositionType.widget` 来定位组件；例如，如果控件或操纵杆必须保持在视口中，那么使用起来就不符合人体工程学。

请注意，只有当该组件直接添加到 `FlameGame` 根目录下，而不是作为其他组件的子组件时，该设置才会生效。

##### Visibility of components
【省略】

#### 1.4.2. PositionComponent
该类表示屏幕上的一个定位对象，可以是一个浮动矩形、一个旋转精灵 (rotating sprite) 或其他任何具有位置和大小的物体。如果添加了子类，它还可以表示一组定位组件。
<span style="color: cyan; font-weight: 700;"><code>PositionComponent</code> 的基本特征是它具有位置、大小、比例、角度和锚点，可以改变组件的渲染方式。</span>
##### Position
`position`是一个 `Vector2`，表示组件锚点相对于其父节点的位置；如果父节点是 `FlameGame`，则表示相对于视口 (viewport) 的位置。

##### Size
当摄像机的缩放级别为 1.0（无缩放，默认值）时组件的`size`。该`size`与组件的父节点无关。

##### Scale
`scale`是指组件及其子组件的缩放程度。由于它是由一个 `Vector2` 表示的，因此可以通过改变 `x` 和 `y` 的相同大小来统一缩放，也可以通过改变 `x` 或 `y` 的不同大小来非统一缩放。

##### Angle
`angle`是围绕锚点 (anchor) 的旋转角度，用双倍弧度表示。**它是相对于父对象的角度而言的**。

##### Native Angle
`nativeAngle` 是以**弧度**为单位的**角度**，顺时针测量，代表组件的默认方向。当角度为零时，它可以用来定义组件的朝向。

在使基于精灵的组件 (sprite-based component) 朝向特定目标时，它特别有用。如果精灵 (sprite) 的原始图像不是朝上/朝北，那么计算出的使组件看向目标的角度就需要一定的偏移量才能使其看起来正确。在这种情况下，可以使用 `nativeAngle` 来让组件知道原始图像的朝向。

例如，子弹图像朝东。在这种情况下，可以将 `nativeAngle` 设置为 pi/2 弧度。以下是一些常见的方向及其对应的原生角度值。
![[Screenshot 2024-01-03 at 15.50.20.png]]
##### Anchor
`anchor`是定义位置和旋转的组件位置（默认为 `Anchor.topLeft`）。因此，如果将锚点设置为 `Anchor.center`，则组件在屏幕上的位置将位于组件的中心，如果应用`angle`，则围绕锚点旋转，在本例中就是围绕组件的中心旋转。您可以将其视为 `Flame` "抓取 "组件的点。

在查询组件的`position`或`absolutePosition`时，返回的坐标是组件锚点的坐标。如果您想查找某个组件的特定`anchor`的位置，而该`anchor`实际上并不是该组件的`anchor`，您可以使用 `positionOfAnchor` 和 `absolutePositionOfAnchor` 方法。
```dart
final comp = PositionComponent(
  size: Vector2.all(20),
  anchor: Anchor.center,
);

// Returns (0,0)
final p1 = component.position;

// Returns (10, 10)
final p2 = component.positionOfAnchor(Anchor.bottomRight);
```

<span style="color: cyan; font-weight: 700">到<a href="https://docs.flame-engine.org/latest/flame/components.html#anchor">这里</a>来玩一下</span> - 这个例子说明的是当我们有两个组件（蓝色为子组件，红色为父组件）时，子组件的改动不会影响到父组件`anchor`的改动。反之，父组件的改动会影响到子组件`anchor`的改动。

使用`anchor`时的一个常见误区是将其混淆为子组件的连接点。例如，将父组件的`anchor`设置为 `Anchor.center`，并不意味着子组件将以父组件为中心放置。

> [!Note]
> 子组件的本地原点始终是其父组件的左上角，与它们的`anchor`值无关。

##### PositionComponent children
`PositionComponent` 的所有子代都将相对于父代进行变换，这意味着`position`、`angle`和`scale`都将相对于父代的状态。因此，举例来说，如果您想将一个子代定位在父代的中心，您可以这样做：
```dart
@override
void onLoad() {
  final parent = PositionComponent(
    position: Vector2(100, 100),
    size: Vector2(100, 100),
  );
  final child = PositionComponent(
    position: parent.size / 2,
    anchor: Anchor.center,
  );
  parent.add(child);
}
```

请记住，在屏幕上渲染的大多数组件都是 `PositionComponent`，因此这种模式也可用于 [`SpriteComponent`](https://docs.flame-engine.org/latest/flame/components.html#spritecomponent) 和 [`SpriteAnimationComponent`](https://docs.flame-engine.org/latest/flame/components.html#spriteanimationcomponent) 等。

##### Render PositionComponent
在为扩展了 `PositionComponent` 的组件实现`render`方法时，请记住要从左上角（0.0）开始呈现。您的呈现方法不应处理组件在屏幕上的呈现位置。要处理组件的渲染位置和渲染方式，请使用 `position`、`angle` 和 `anchor` 属性，Flame 会自动为您处理其余部分。

**如果您想知道组件的边界框在屏幕的哪个位置，可以使用 `toRect` 方法。**

**如果您想改变组件的渲染方向，也可以使用 `flipHorizontally()` 和 `flipVertically()` 在 `render(Canvas canvas)`过程中围绕锚点翻转画在画布上的任何内容。这些方法适用于所有 `PositionComponent` 对象，尤其适用于 `SpriteComponent` 和 `SpriteAnimationComponent`。**

如果想围绕中心翻转组件，而无需将锚点更改为 `Anchor.center`，可以使用 `flipHorizontallyAroundCenter()` 和 `flipVerticallyAroundCenter()`。

#### 1.4.3. SpriteComponent
最常用的 `PositionComponent` 实现是 `SpriteComponent`，它可以用一个 `Sprite` 来创建：
```dart
import 'package:flame/components/component.dart';

class MyGame extends FlameGame {
  late final SpriteComponent player;

  @override
  Future<void> onLoad() async {
    final sprite = await Sprite.load('player.png');
    final size = Vector2.all(128.0);
    final player = SpriteComponent(size: size, sprite: sprite);

    // Vector2(0.0, 0.0) by default, can also be set in the constructor
    player.position = Vector2(10, 20);

    // 0 by default, can also be set in the constructor
    player.angle = 0;

    // Adds the component
    add(player);
  }
}
```

#### 1.4.4. SpriteAnimationComponent
该类用于表示具有以单个循环动画运行的精灵的组件。
它将使用 3 个不同的图像创建一个简单的三帧动画：
```dart
@override
Future<void> onLoad() async {
  // 加载3帧：
  final sprites = [0, 1, 2]
      .map((i) => Sprite.load('player_$i.png'));

  // 用上面的3帧 生成动画
  final animation = SpriteAnimation.spriteList(
    await Future.wait(sprites),
    stepTime: 0.01,
  );

  // 玩家形象就是基于该 3帧动画
  this.player = SpriteAnimationComponent(
    animation: animation,
    size: Vector2.all(64.0),
  );
}
```

如果您有一个精灵表，可以使用 `SpriteAnimationData` 类中的顺序构造函数（详情请查看[images > Animation](https://docs.flame-engine.org/latest/flame/rendering/images.html#animation)）：
```dart
@override
Future<void> onLoad() async {
  final size = Vector2.all(64.0);
  final data = SpriteAnimationData.sequenced(
    textureSize: size,
    amount: 2,
    stepTime: 0.1,
  );
  this.player = SpriteAnimationComponent.fromFrameData(
    await images.load('player.png'),
    data,
  );
}
```

**所有动画组件都在内部维护一个 `SpriteAnimationTicker`，用于勾选 `SpriteAnimation`。这允许多个组件共享同一个动画对象。**
举例说明:
```dart
final sprites = [/*You sprite list here*/];
final animation = SpriteAnimation.spriteList(sprites, stepTime: 0.01);

final animationTicker = SpriteAnimationTicker(animation);

// or alternatively, you can ask the animation object to create one for you.

final animationTicker = animation.createTicker(); // creates a new ticker

animationTicker.update(dt);
```

要监听动画完成（到达最后一帧且不循环）的时间，可以使用 `animationTicker.completed`。
```dart
await animationTicker.completed;

doSomething();

// or alternatively

animationTicker.completed.whenComplete(doSomething);
```

此外，`SpriteAnimationTicker` 还有以下可选事件回调：`onStart`、`onFrame` 和 `onComplete`。要监听这些事件，可以执行以下操作：
```dart
final animationTicker = SpriteAnimationTicker(animation)
  ..onStart = () {
    // Do something on start.
  };

final animationTicker = SpriteAnimationTicker(animation)
  ..onComplete = () {
    // Do something on completion.
  };

final animationTicker = SpriteAnimationTicker(animation)
  ..onFrame = (index) {
    if (index == 1) {
      // Do something for the second frame.
    }
  };
```


#### 1.4.5. SpriteAnimationGroupComponent
TODO from [here](https://docs.flame-engine.org/latest/flame/components.html#spriteanimationgroupcomponent)















