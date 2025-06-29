# TMP doc

- [gitbook](https://skyao.gitbooks.io/learning-gitbook/content/publish/gitbook.html)
- [iOS 效能](https://medium.com/@hooy123456_58230/ios-%E6%95%88%E8%83%BD%E6%96%B9%E9%9D%A2%E7%9A%84%E5%95%8F%E9%A1%8C-in-progress-5579269e5364)
  - [Swift dispatch dive deep](https://blog.jacobstechtavern.com/p/compiler-cocaine-the-swift-method)
  - [Swift actor](https://juejin.cn/post/7076738494869012494)
    - 一种新的并发机制。
    - actor修饰的类会自我保护代码不受多线程影响。
  - [深入剖析 iOS 性能优化](https://ming1016.github.io/2017/06/20/deeply-ios-performance-optimization/)
- [Architecture](https://medium.com/@KodeFlap/choosing-android-architectures-mvc-mvp-mvvm-clean-architecture-and-mvi-8ad2a43f7f9b)
  - MVC, model /view/ controller
  - MVP, presenter, which resposiblity for data request and refresh the view. 
    - Use MVP for medium-sized projects with more complex requirements.
    - In MVP, the view holds a reference to the presenter, and the presenter interacts with the view through an interface.
  - MVVM, usually use data-driver (reactive programming / data binding) app development. VM, bind the UI/Model event.
    - In MVVM, the view model does not directly reference the view; instead, the view binds to properties and commands exposed by the view model
    - In MVVM, changes in the view model automatically update the view through data binding mechanisms, while in MVP, the presenter updates the view by invoking methods defined in the view interface.
  - Clean architecture. A define of softer architecture.
    - Data(Entity) -> Domain -> Presenter -> Framework
  - MVI. Intent
    - 严格控制数据的流动方向
    - Use MVI for projects that require a strict unidirectional data flow and highly interactive UIs.

## OC

### OC run time

- isa 会存储引用计数。(据说)

```c #include <objc/objc.h>
#if !OBJC_TYPES_DEFINED
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class;

/// Represents an instance of a class.
struct objc_object {
    Class _Nonnull isa  OBJC_ISA_AVAILABILITY;
};

/// A pointer to an instance of a class.
typedef struct objc_object *id;
#endif

```

```c
struct objc_class {
    Class _Nonnull isa  OBJC_ISA_AVAILABILITY;
#if !__OBJC2__
    Class _Nullable super_class                              OBJC2_UNAVAILABLE;
    const char * _Nonnull name                               OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list * _Nullable ivars                  OBJC2_UNAVAILABLE;
    struct objc_method_list * _Nullable * _Nullable methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache * _Nonnull cache                       OBJC2_UNAVAILABLE;
    struct objc_protocol_list * _Nullable protocols          OBJC2_UNAVAILABLE;
#endif
} OBJC2_UNAVAILABLE;
/* Use `Class` instead of `struct objc_class *` */

typedef struct objc_object *id;
typedef struct objc_class *Class;

struct objc_object {
    Class isa;
};

struct objc_class : objc_object {
    Class superclass;
    cache_t cache;     
    class_data_bits_t bits;  
    //...
}
```

### Weak implementation

- [Objective-C weak 弱引用实现](https://triplecc.github.io/2019/03/20/objective-c-weak-implement/)

- SideTable entry.
- Insert key when `weak` decorate the variable.

## iOS 内存泄漏检测

- [](https://engineering.fb.com/2016/04/13/ios/automatic-memory-leak-detection-on-ios/)
- Tool: PLeakSniffer / MLeaksFinder
  - [MLeaksFinder](https://wereadteam.github.io/2016/02/22/MLeaksFinder/)

### OOM


## Core animation

当前layer的动画时间计算 `t = (tp - begin) * speed + offset`

- t 当前动画执行的时间点。
- tp，parent layer 动画执行的时间点。
- 动画开始的时间（相对于动画定义），默认为0
- 动画速度。默认0
- offset，执行动画的时间偏移

动画暂停和resume代码。

```oc
-(void)pauseLayer:(CALayer*)layer {
   CFTimeInterval pausedTime = [layer convertTime:CACurrentMediaTime() fromLayer:nil];
   layer.speed = 0.0;
   layer.timeOffset = pausedTime;
}
 
-(void)resumeLayer:(CALayer*)layer {
   CFTimeInterval pausedTime = [layer timeOffset];
   layer.speed = 1.0;
   layer.timeOffset = 0.0; // convertTime 受影响。且该值也会影响下一次的pause时间计算。
   layer.beginTime = 0.0; // pause -> resume 时，必须先把 `beginTime` 设置回0，因为 `convertTime` 受该值的影响。
   CFTimeInterval timeSincePause = [layer convertTime:CACurrentMediaTime() fromLayer:nil] - pausedTime;
   layer.beginTime = timeSincePause;
}
```

## SwiftUI

- [Model observeable](https://www.sunyazhou.com/2022/11/swiftuipropertywrapper/)
  - Statu
  - Binding
  - ObserverabelObject
  - EnviormentObject
  - All those will cause the current view refresh.
- [Difference](https://www.hackingwithswift.com/quick-start/swiftui/whats-the-difference-between-observedobject-state-and-environmentobject)
  - Use @State for simple properties that belong to a single view. They should usually be marked private.
  - Use @ObservedObject for complex properties that might belong to several views. Most times you’re using a reference type you should be using @ObservedObject for it. 观察对象，从其他地方获取
  - Use @StateObject once for each observable object you use, in whichever part of your code is responsible for creating it. 创建对象
  - Use @EnvironmentObject for properties that were created elsewhere in the app, such as shared data. 共享全局

`Of the four you will find that @ObservedObject is both the most useful and the most commonly used, so if you’re not sure which to use start there.`

- [resultBuilder](https://www.hackingwithswift.com/swift/5.4/result-builders) [ViewBuilder 研究（上）—— 掌握 Result builders](https://fatbobman.com/zh/posts/viewbuilder1/)
  - static method buildBlock
  - ViewBuilder 使用者

-[Scrollable](https://www.swiftyplace.com/blog/how-to-use-swiftui-scrollview)

## Flutter performance

- [17 个提高性能的 Flutter 最佳实践](https://inficial.medium.com/flutter-best-practices-for-improve-performance-7e21e14efebb)
- [Flutter Under the Hood: Owners](https://medium.com/@mbixjkee1392/flutter-under-the-hood-owners-2ec741d45bea)

### Flutter layout
Render Objects are responsible for this. Three rules make it possible:

- Constraints go down by the tree from the parents to the children.
- Sizes go up by the tree from the children to the parents.
- Parents set the position of the children.

### Flutter Overflow

- [method](paintOverflowIndicator)
  - Flex will report overflow if the items totoal size bigger than real bound.
  - RenderConstraintsTransformBox
  - RenderFlex


## TO-DO List

### iOS knowleage

- Combin
- URLSession