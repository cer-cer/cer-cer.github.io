# TMP doc

- [gitbook](https://skyao.gitbooks.io/learning-gitbook/content/publish/gitbook.html)

- [iOS 效能](https://medium.com/@hooy123456_58230/ios-%E6%95%88%E8%83%BD%E6%96%B9%E9%9D%A2%E7%9A%84%E5%95%8F%E9%A1%8C-in-progress-5579269e5364)
  - [Swift dispatch dive deep](https://blog.jacobstechtavern.com/p/compiler-cocaine-the-swift-method)
    - Inline
      - no jump, coupy the code
    - Static
      - one jump.
      - final / private
    - Message forward
    - Dynamic dispatch (vtable) (C++)
      - 2 jumps. table -> function
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
  - [VIPER Architecture in iOS Development](https://medium.com/@authfy/understanding-viper-architecture-in-ios-development-e097a54a7a80)
    - View - presenter - interactor - entity
    - presenter - route
    - Interactor: responsible for business logic and data fetching.
    - Presenter: observable class.
      - The Presenter acts as the middleman between the View and the Interactor. It retrieves data from the Interactor and updates the View with that data.
  - [VIP Architecture pattern](https://nirajpaul2.medium.com/vip-architecture-pattern-vip-viper-302d7d1069df)
    - View Controller: 
      - Listening UI event.
      - Display the View Model.
    - Interacter:
      - fetching data, business logic
      - maintence data status
    - Presenter:
      - Transform the data model to view model.
      - Pass view model to View Controller
      - View model is observable.
    - VIP vs VIPER
      - VIP works in unidirectional
      - VIP uses segue for the navigation
      - VIPER is bidirectional
      - VIPER Not uses segue for the navigation
  - [The Composable Architecture](https://medium.com/@dmitrylupich/the-composable-architecture-swift-guide-to-tca-c3bf9b2e86ef)
    - View -> Action -> Store -> Reducer -> State -> View
    - the View emits some Action, that Action goes to the Store, which calls the Reducer to recalculate State, and the new State is published to the View. 

- [Understanding the Mach-O File Format](https://medium.com/@travmath/understanding-the-mach-o-file-format-66cf0354e3f4)
- [Mach-O 格式](https://www.valiantcat.cn/index.php/2023/08/31/72.html#:~:text=Mach%2DO%20%E6%96%87%E4%BB%B6%E4%B8%BB%E8%A6%81%E5%86%85%E5%AE%B9%20Mach%2DO%20%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F%E6%94%AF%E6%8C%81%E5%A4%9A%E7%A7%8D%E4%B8%8D%E5%90%8C%E7%9A%84CPU%20%E6%9E%B6%E6%9E%84%EF%BC%88%E4%BE%8B%E5%A6%82x86%E3%80%81ARM%E3%80%81PPC%20%E7%AD%89%EF%BC%89%EF%BC%8C%E4%BB%A5%E5%8F%8A%E4%B8%8D%E5%90%8C%E7%9A%84%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%EF%BC%88%E4%BE%8B%E5%A6%82%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6%E3%80%81%E5%8A%A8%E6%80%81%E9%93%BE%E6%8E%A5%E5%BA%93%E3%80%81%E9%9D%99%E6%80%81%E5%BA%93%E7%AD%89%EF%BC%89%E3%80%82%20Mach%2DO,%E9%87%8D%E5%AE%9A%E4%BD%8D%E4%BF%A1%E6%81%AF%E6%8C%87%E7%A4%BA%E9%93%BE%E6%8E%A5%E5%99%A8%E5%A6%82%E4%BD%95%E4%BF%AE%E6%94%B9%E7%A8%8B%E5%BA%8F%E4%BB%A3%E7%A0%81%E5%92%8C%E6%95%B0%E6%8D%AE%EF%BC%8C%E4%BB%A5%E4%BE%BF%E6%AD%A3%E7%A1%AE%E5%9C%B0%E5%9C%A8%E5%86%85%E5%AD%98%E4%B8%AD%E5%8A%A0%E8%BD%BD%E3%80%82%20%E6%89%80%E6%9C%89%E9%9C%80%E8%A6%81%E9%87%8D%E5%AE%9A%E4%BD%8D%E4%BF%A1%E6%81%AF%E7%9A%84section%20%E9%83%BD%E4%BC%9A%E4%BA%A7%E7%94%9F%E4%B8%80%E4%B8%AA%E5%AF%B9%E5%BA%94%E7%9A%84%E9%87%8D%E5%AE%9A%E5%90%91%E7%9A%84section%E3%80%82%20Dynamic%20Loader%20Information%EF%BC%88%E5%8A%A8%E6%80%81%E9%93%BE%E6%8E%A5%E4%BF%A1%E6%81%AF%EF%BC%89%EF%BC%9A%20%E7%94%A8%E4%BA%8E%E6%94%AF%E6%8C%81%E5%8A%A8%E6%80%81%E9%93%BE%E6%8E%A5%E7%9A%84%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%A6%82%E5%85%B1%E4%BA%AB%E5%BA%93%E7%9A%84%E5%BC%95%E7%94%A8%E5%92%8C%E5%AF%BC%E5%87%BA%E3%80%82)
  - header
  - load command
  - segment
  - [Static Code Injection in Mach-O Binaries](https://jon-gabilondo-angulo-7635.medium.com/static-code-injection-in-mach-o-binaries-894fb0c86fbf)

## iOS

### OC run time

- isa 在64位机上会用前20位存储引用计数。(据说)

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
  - no increase the reference count
  - the varible dealloc will set to nil.
- SideTable entry.
- Insert key when `weak` decorate the variable.
- Unowner reference

### Swift

- [The Swift Programming Language (6.1) 中文版](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures/#Implicit-Returns-from-Single-Expression-Closures)

- ~Copyable 不可复制
  - [逆流而上的设计 - Swift 所有权和 ~Copyable](https://onevcat.com/2024/11/noncopyable/)
- consuming
  - 标志已消费，则无法使用原对象. `let s2 = consume s1`
- borrowing
  - `func borrowS(_ s: borrowing S)`
  - 只能读取，不能转移
- Copy-On-Write for value type
  - struct muttable use another solution - add `inout` parameter
- enum is type in Swift which the raw type could be int/string/char/float.
  - OC's enum just a int (same with C/C++)
  - Swift enum can associated value but can't OC enum.
- frozen
- inlinable
- usableFromInline
- targetEnvironment
- some vs any

some 关键特性：
  - 编译时类型确定：编译器知道具体类型
  - 单向类型隐藏：调用者不知道具体类型
  - 性能优势：静态派发，无运行时开销
  - 使用限制：必须返回固定具体类型

any 关键特性：
  - 运行时类型动态：可存储多种符合协议的类型
  - 类型擦除：隐藏具体类型信息
  - 使用灵活性：适合异构集合
  - 性能开销：动态派发和内存管理成本

| 特性               | `some`                          | `any`                           |
|--------------------|---------------------------------|---------------------------------|
| **类型确定性**      | 编译时确定具体类型              | 运行时动态类型                  |
| **是否支持多类型**  | ❌ 只能一种具体类型             | ✅ 支持多种不同类型             |
| **性能**           | ⭐️ 高效（静态派发）            | ⚠️ 有开销（动态派发、装箱）     |
| **Swift 版本**     | 5.1+                        | 5.7+（改进的存在类型）          |
| **典型用途**       | 返回隐藏具体类型的值            | 存储或传递多种协议类型          |
| **类型擦除**       | 否（编译器知道具体类型）        | 是（运行时类型擦除）            |


- repeat each
  - [Iterate Over Parameter Packs in Swift 6.0](https://www.swift.org/blog/pack-iteration/)


#### Swift cocurrent

- Actor 
  - isolator to proprect the cocurrent
- Task
  - cocurrent.

- [Controlling Actors With Custom Executors](https://jackmorris.xyz/posts/2023/11/21/controlling-actors-with-custom-executors/)
- [Task Groups in Swift explained with code examples](https://www.avanderlee.com/concurrency/task-groups-in-swift/)

### OC <-> Swift

#### SWift call OC
- bridge file include swift要调用 的 oc头文件。
- OC 用以下两个修饰 
  - NS_SWIFT_NAME(替换名)：重命名在Swift中的名称，可用来进行方法名隐藏
  - NS_SWIFT_UNAVAILABLE(_msg)：Swift中不可见，不能使用
- NS_REFINED_FOR_SWIFT
  - oc 为swfit
- `xxx-Bridging-Header.h`

#### OC call Swift

- 必须继承于 NSObject
- 必须使用 @objc 修饰
- 在OC文件中引入项目名-Swift.h文件
- OC类不能继承于Swift类，但Swift类可以继承于OC类

## iOS perfromance

- [Optimizing iOS App Performance: Tips and Tools](https://medium.com/@matthewbrain093/optimizing-ios-app-performance-tips-and-tools-397006effc9b)
- [15 iOS App Performance Optimization Techniques](https://daily.dev/blog/15-ios-app-performance-optimization-techniques)

### memory

- Allocate profile (instruments)
  - 追踪内存分配，记录内存分配和释放事件
- [Leaks profile](https://www.cnblogs.com/wgb1234/articles/16834299.html)
- [Automatic memory leak detection on iOS](https://engineering.fb.com/2016/04/13/ios/automatic-memory-leak-detection-on-ios/)
- Tool: PLeakSniffer / MLeaksFinder
  - [MLeaksFinder](https://wereadteam.github.io/2016/02/22/MLeaksFinder/)

#### OOM

- iOS will be crashed by OMM when the membory bigger than a threhold after memory warning.
- The threhold may be 2gb.
- FOOM / BOOM

### CPU

- Focus on minimizing computations on the main thread, utilizing background processing, and optimizing data handling and UI rendering.
  - Minimize Main Thread Work
    - Background Processing
    - Asynchronous Operations
    - Data Fetching and Processing
  - Optimize UI Rendering (FPS)
    - Efficient Table and Collection Views
      - Implement proper cell reuse and data loading strategies.
      - Minimize Layout Calculations
      - Use CALayer
- Adopt advance algorithm
- Utilize Xcode's Instruments
  - Time Profiler.

### FPS

- 减少主线成计算量，包括layout和长时间计算。
- 减少离屏渲染

### Network Optimization

- Minimize Network Requests
  - Combine multiple network requests into single requests whenever possible. 
- Implement Pagination
  -  Load data in chunks (pagination) for large datasets to improve responsiveness. 
-  Prefetching
   -  Anticipate user needs and prefetch data to improve perceived performance. 
-  use HTTPDNS 
   - DNS cache

### Reduce the App Loading Time

- 减少全局变量
- 减少动态库， rebase
- 减少 objct-c 调用 load 方法。

### 包大小

- 删除无用资源
  - LSUnusedResources
- 重复资源
- 资源管理
  - xcassets
- 按需加载资源
  - 从后台下载资源
- 图片资源压缩
  - WebP

### 内存优化

- 主要集中在视图的生命周期管理、数据加载与存储、以及避免不必要的内存占用

### Profile tool

INSTRUMENTS TOOLKIT:
┌────────────────┬─────────────────────────────┐
│    Tool        │          Purpose            │
├────────────────┼─────────────────────────────┤
│ Time Profiler  │ CPU bottlenecks             │
│ Allocations    │ Memory usage                │
│ Leaks          │ Memory usage                │
│ Energy Log     │ Battery consumption         │
│ Core Animation │ UI rendering performance    │
└────────────────┴─────────────────────────────┘



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
  - Status `value type`
  - StateObject `reference type`
    - 只初始化一次。
    - 可以通过改变view的 ID 再次初始化。
  - Binding
  - ObserverabelObject
    - 内部包含 `Published` 属性
  - EnviormentObject
    - 通过 environmentObject 向下传递
  - Environment
    - 系统预置或者自定义
    - 通过 environment 绑定keypath 设置
    - @Environment(\.colorScheme)
  - All those will cause the current view refresh.
- [Difference](https://www.hackingwithswift.com/quick-start/swiftui/whats-the-difference-between-observedobject-state-and-environmentobject)
  - Use @State for simple properties that belong to a single view. They should usually be marked private.
  - Use @ObservedObject for complex properties that might belong to several views. Most times you’re using a reference type you should be using @ObservedObject for it. 观察对象，从其他地方获取
  - Use @StateObject once for each observable object you use, in whichever part of your code is responsible for creating it. 创建对象
  - Use @EnvironmentObject for properties that were created elsewhere in the app, such as shared data. 共享全局
  - @StateObject 管理生命周期，并向下传递 @ObservableObject 对象。@ObservedObject 不管理生命周期，从其他地方获取 `ObservableObject` 对象。 `ObservableObject` 持有 `Published`修饰的property.
  

`Of the four you will find that @ObservedObject is both the most useful and the most commonly used, so if you’re not sure which to use start there.`

- [resultBuilder](https://www.hackingwithswift.com/swift/5.4/result-builders)
  - [Result builder types](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0289-result-builders.md#result-building-methods)
  - Domain special languge
- [ViewBuilder 研究（上）—— 掌握 Result builders](https://fatbobman.com/zh/posts/viewbuilder1/)
  - static method buildBlock
  - ViewBuilder 使用者
- `some View` 
  - `opaque reutrn`
  - 隐藏返回的具体类型。
  - 静态类型保证（编译）。 `some View & protocol`
  - 返回值只能是一种类型。
- `any View`
  - Any view
  - 运行时保证。性能没有some快
- Struct 
  - struct value type, which could use `Equatable`
  - immutable
  - no ARC

- [Scrollable](https://www.swiftyplace.com/blog/how-to-use-swiftui-scrollview)

- Swiftui use Sendable
  - 确保数据在并发环境下可以安全地传递和共享，从而避免数据竞争和潜在的并发问题

## Swift interview

- [iOS Interview Series](https://medium.com/@nimjea/ios-interview-series-part-1-core-swift-and-ios-concepts-you-must-know-1c7d73ef3d63)
  - Value vs Reference
    - struct & enum are value types, which means it can't inherit, no ARC, immutable.
    - class is reference
    - C O W `Copy on Write` 
  - [Swift Error Handling](https://levelup.gitconnected.com/swift-error-handling-build-resilient-bug-free-apps-809992ef5d70)
    - Exception (Do-Try-Catch Blocks)
    - Optionals
    - Error protocol
    - Result<String, Error>
- [HSBC iOS Interview — Real Candidate Experiences](https://medium.com/@infinityvaibhav/hsbc-ios-interview-real-candidate-experiences-6f27f7a1e5fb)
  - SOLID principles
    - 单一职权，open-close，李氏替换，接口隔离，依赖反转
  - What are access specifiers in Swift
    - open: 可访问，可override
    - public 可访问不可override (class修饰)
    - internal， 模块内访问, method 可以override
    - fileprivate， 文件内访问
    - private， 类内访问
  - closures 闭包
    - [Swift - 闭包（Closure）](https://juejin.cn/post/7425871943642005556) 
    - [Closures with Swift](https://medium.com/@Dougly/closures-with-swift-58b274d849ad)
      - Closures are self-contained blocks of functionality that can be passed around and used in your code. Closures in Swift are similar to blocks in C and Objective-C and to lambdas in other programming languages.
    - [iOS-Swift中常见的几种闭包](https://juejin.cn/post/7053987682677948430)
      - Global functions are closures that have a name and don’t capture any values.
      - Nested functions are closures that have a name and can capture values from their enclosing function.
      - Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.
      - 逃逸闭包 @escaping
      - 自动闭包: @autoclosure, 自动把表达式转换为闭包
      - swift 执行的时候再捕获
  - Higher order functions
    - 函数参数 为函数
    - 函数返回值为函数
    - Array
      - map
      - reduce
      - compactMap
      - sorted
      - filter
  - [Ways to secure iOS App -SwiftUI](https://medium.com/@vijaynagarajan93/ways-to-secure-ios-app-swiftui-36f31e32c2c8)
  - [Security Measures in SwiftUI Applications](https://medium.com/@gokhanvaris/security-measures-in-swiftui-applications-8cd13c4df7e3)
    - Secure Data Storage
      - Keychain for Secure Storage
    - Secure Network Communication-HTTPS
      - SSL cert pinning
      - SSL public key pinning
      - Session 攻击
        - 使用超时时间
        - 使用secure header
      - Replay Attack Protection
        - Use nonce (number used once) or timestamps in your API requests to ensure the request is legitimate and prevent replay. 
    - Root/Jailbreak Detection
    - [Code Obfuscation](https://github.com/MartinHuang0933/Blog/blob/master/iOS/iOS%20%E8%B3%87%E8%A8%8A%E5%AE%89%E5%85%A8%E5%8A%A0%E5%9B%BA%E6%96%B9%E6%A1%88-%E7%A8%8B%E5%BC%8F%E7%A2%BC%E6%B7%B7%E6%B7%86(SwiftShield).md)
      - bitcode
      - SwiftShield
    - Detect and Disable taking ScreenRecords
      - UIScreen.capturedDidChangeNotification
    - App Switcher Snapshot Controls
      - .overlay(Color.black.ignoresSafeArea()).
    - Logging Practices
      - While logging is vital for debugging, leaving sensitive information in logs can be a security risk.
    - Regular Updates and Patching
    - Two-Factor Authentication:
  - [Implement OTP (one time password) autofill](https://shankarmadeshvaran.medium.com/how-to-implement-automatic-otp-verification-in-ios-7813c6116a1d)
    - UITextInputTraits
      - textContentType = oneTimeCode
    - Message should contain passcode or code keyword.
  - Environment variables and data externalization
    - Enviroment variables
      - Scoped to the View Hierarchy
      - Dynamic Updates: Changes to environment variables automatically propagate to all views that depend on them.
      - Built-in Environment Values: SwiftUI provides many built-in environment values like colorScheme, presentationMode, and locale
    - Global Variables
      - Use Singleton Pattern
      - Leverage Dependency Injection
      - Avoid Overuse
    - [SwiftUI Environment Variables and Global Variables: Best Practices and Examples](https://medium.com/@gongati/swiftui-environment-variables-and-global-variables-best-practices-and-examples-b2c31901ba67)
    - Data externalization
      - Cloud storage
      - File Servers
  - Network failure detection techniques
    - Tool
      - NWPathMonitor
      - Reachability
      - Network reqeust monitor
    - Optimization
      - 监听reqeuset delegate
      - timeoutIntervalForRequest 设置超时时长
      - 设置并发链接池
  - [OC vs Swift](https://medium.com/@hooy123456_58230/%E6%88%91%E5%8E%BB%E5%B9%B4%E9%9D%A2%E8%A9%A6%E6%99%82%E9%81%87%E5%88%B0%E7%9A%84-ios-%E7%9F%A5%E8%AD%98%E9%A1%8C-in-progress-87faa62f74dc)
    - Static vs dynamic forward
      - final / static
    - value pass / reference pass
      - COW
      - Inherit
      - mutating / inout
  - 掉帧 优化
    - 原因
      - 主线成被占用
      - 离屏渲染
    - 检测
      - CADisplayLink
      - instructment 分析
- Behavioral Questions
  - Summary:
    - Use the STAR method: Situation, Task, Action, Result. 
    - Be specific: Provide concrete examples rather than generalities. 
    - Be honest and authentic: Let your personality and passion for iOS development shine through. 
    - Relate your answers to the job requirements: Tailor your responses to demonstrate how your skills and experience align with the specific role and company. 
  - Collaboration and Teamwork
    - [Focus: Highlighting communication, active listening, and conflict resolution skills. ](https://medium.com/@sunee.ragu/behavioral-and-general-interview-questions-for-ios-developer-2022-fb2c6d7a9b1c)
    - Focus: Highlighting communication, active listening, and conflict resolution skills. 
  - Problem-Solving and Decision-Making
    - Question: "Tell me about a time you faced a significant technical challenge. How did you approach it, and what was the result?"
    - Demonstrating analytical skills, creative problem-solving, and the ability to learn from mistakes.
    - 分析，数据，方案和决策
  - Adaptability and Learning
    - Question: "Describe a time you had to adapt to a significant change in iOS development, such as a new iOS version or framework."
    - Highlighting the ability to learn new technologies, adapt to change, and stay current with industry trends.
    - 调研，模仿，验证。
  - Handling Pressure and Deadlines
    - Q: "Give me an example of a time you worked under pressure to meet a deadline."
    - Showcasing time management skills, ability to prioritize tasks, and ability to work effectively under pressure.
    - 沟通，分析，备选方案，风险控制。
  - Strengths and Weaknesses
    - Focus: Providing honest self-assessment and demonstrating self-awareness.
  - Motivation and Career Goals
    - Question: "What are you most proud of accomplishing?"
    - Focus: Highlighting achievements and demonstrating passion for iOS development.
- What are some good coding practices?
  - Code indentation
  - Meaningful naming
  - Comments that add context
  - Don’t repeat yourself. (DRY princple)
  - Low coupling and high cohesion
  - Consider your context


### access总结表：
| 方法访问级别 | 可访问范围 | 可被外部模块重写（仅当类为`open`） |
|--------------|------------|-----------------------------------|
| `open`       | 任何地方   | 是（要求类也是`open`）            |
| `public`     | 任何地方   | 否                                |
| `internal`   | 同一模块   | 否                                |
| `fileprivate`| 同一文件   | 否                                |
| `private`    | 类内或同一文件的扩展 | 否                         |


### 闭包

```swift
{[capturing list] (parameters) -> return type in
   statements
}
```

Closures can take one of three forms:
- Global functions are closures that have a name and don’t capture any values.
- Nested functions are closures that have a name and can capture values from their enclosing function.
- Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.

#### callAsFunction

- @dynamicCallable
- [Methods with Special Names](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/declarations/#Methods-with-Special-Names)

## swift 单元测试

### Swift testing

- @Test
- @Suite
- 异步
  - await
  - confirmation

```swift
@Suite("name") struct XX {
  @Test func test1() {}
}
```

### XCTest

- Create a new subclass of XCTestCase within a test target.
- Add one or more test methods to the test case.
- Add one or more test assertions to each test method.

```swift
class XX: XCTesrt {
  func test1() {

  }
}
```

## Flutter

## TLS/SSL 

### [Flow](https://www.cloudflare.com/zh-cn/learning/ssl/what-happens-in-a-tls-handshake/)

- 客户端 hello， 发送版本，随机数等。
- 服务器发送证书等。
- 客户端校验证书
  - 使用服务器端发送的证书找到跟证书，校验根签发机构是否信任
  - 使用根证书公钥解码证书hash，求h1
  - 使用发送的证书hash算法再次hash，求h2
  - 比较h1 == h2
  - 通过后，使用证书公钥加密 新的随机数发送到后台
- 后台解密，生成会话密钥(c1 + s1 + c2).

- 证书机构用私钥c0对证书hash值加密。hash内容包括公钥，域名，有效期等。（所以可以用公钥c1对其解密，获得hash值）。
- 客户端得到证书后，证书内容包括签发机构/公钥等明文信息。
  - 客户端根据签发机构，找到签发机构的根证书，用根证书上的公钥，对签名解密，得到hash值 A.
  - 客户端用收到的证书上的公钥等其他信息计算hash 值B
  - 比较 证书已经有的hash A 是否等于 计算出来的hash B.
  - 如果相等，则说明证书有效，该证书上的公钥可以用于后续的加密。
- TLS 1.2 用 rsa
- TLS 1.3
  - 密码套件
    - 密码套件是一组用于建立安全通信连接的算法。广泛使用的密码套件有多种，而且 TLS 握手的一个重要组成部分就是针对这一握手使用哪个密码套件达成一致意见。
  - 

#### SSL pinning in iOS

把服务器端证书锁定在app中，每一次按该证书进行校验。防止被劫持。

- [iOS cer pinning](https://medium.com/@greenSyntax/ssl-pinning-in-ios-f508b5860ead)

```ios code
 func urlSession(_ session: URLSession, didReceive challenge: URLAuthenticationChallenge, completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {
        
        if let trust = challenge.protectionSpace.serverTrust,
           SecTrustGetCertificateCount(trust) > 0 {
            if let certificate = SecTrustGetCertificateAtIndex(trust, 0) {
                let data = SecCertificateCopyData(certificate) as Data
                
                if certificates.contains(data) {
                    completionHandler(.useCredential, URLCredential(trust: trust))
                    return
                } else {
                    //TODO: Throw SSL Certificate Mismatch
                }
            }
            
        }
        completionHandler(.cancelAuthenticationChallenge, nil)
    }

private let certificates: [Data] = {
        let url = Bundle.main.url(forResource: "run.mocky.io", withExtension: "cer")!
        let data = try! Data(contentsOf: url)
        return [data]
}()
```

- [SSL Pinning via Public Key](https://medium.com/@otufekci/ios-ssl-pinning-with-public-key-8ebdc2d32a9f)


## 算法

- [穷举](https://houbb.github.io/2020/01/23/data-struct-learn-07-base-enum)

## LLDB (Low level debug)

- [Swift with LLDB](https://commitstudiogs.medium.com/debugging-swift-code-effectively-with-lldb-e5e52da180af)

## TO-DO List

### iOS knowleage

- Combin
- URLSession