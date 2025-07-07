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
  - VIPER
    - View - presenter - interactor - entity
    - presenter - route

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

- [Automatic memory leak detection on iOS](https://engineering.fb.com/2016/04/13/ios/automatic-memory-leak-detection-on-ios/)
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
  - @StateObject 管理生命周期，并向下传递 @ObservableObject 对象。@ObservedObject 不管理生命周期，从其他地方获取 `ObservableObject` 对象。 `ObservableObject` 持有 `Published`修饰的property.
  

`Of the four you will find that @ObservedObject is both the most useful and the most commonly used, so if you’re not sure which to use start there.`

- [resultBuilder](https://www.hackingwithswift.com/swift/5.4/result-builders) [ViewBuilder 研究（上）—— 掌握 Result builders](https://fatbobman.com/zh/posts/viewbuilder1/)
  - static method buildBlock
  - ViewBuilder 使用者

-[Scrollable](https://www.swiftyplace.com/blog/how-to-use-swiftui-scrollview)

## Swift 面试题

- [iOS Interview Series](https://medium.com/@nimjea/ios-interview-series-part-1-core-swift-and-ios-concepts-you-must-know-1c7d73ef3d63)
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
      - 全局闭包： 没有捕获
      - 闭包表达式
      - 尾随闭包
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
    - Local storage 
      - Keychain for Secure Storage
    - Secure Network Communication-HTTPS
      - SSL cert pinning
      - SSL public key pinning
    - Jailbreak Detection
    - Detect and Disable taking ScreenRecords
      - UIScreen.capturedDidChangeNotification
    - App Switcher Snapshot Controls
      - .overlay(Color.black.ignoresSafeArea()).
    - Logging Practices
      - While logging is vital for debugging, leaving sensitive information in logs can be a security risk.
    - Regular Updates and Patching
  - [Implement OTP (one time password) autofill](https://shankarmadeshvaran.medium.com/how-to-implement-automatic-otp-verification-in-ios-7813c6116a1d)
    - UITextInputTraits
      - textContentType = oneTimeCode
    - Message should contain passcode or code keyword.


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

Closures can take one of three forms

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

## Flutter performance

- [17 个提高性能的 Flutter 最佳实践](https://inficial.medium.com/flutter-best-practices-for-improve-performance-7e21e14efebb)
  - Avoid rebuilding all the widgets repetitively
  - And it would be a good idea to add const.
  - Use nil instead const Container
  - User itemExtent in ListView for long lists.
  - Use for/while instead of foreach/map
  - Precache your images and icons
  - Consider using the RepaintBoundary widget
  - Use builder named constructors if possible
- [Flutter Under the Hood: Owners](https://medium.com/@mbixjkee1392/flutter-under-the-hood-owners-2ec741d45bea)


### Flutter layout
Render Objects are responsible for this. Three rules make it possible:

- Constraints go down by the tree from the parents to the children.
- Sizes go up by the tree from the children to the parents.
- Parents set the position of the children.

### Flutter Overflow

- [method](paintOverflowIndicator)
  - RenderConstraintsTransformBox
  - RenderFlex
    - Flex will report overflow if the items totoal size bigger than real bound.
  - RenderStack
    - 

### Flutter animation

- [Scrolling Animation in Flutter](https://medium.com/flutter-community/scrolling-animation-in-flutter-6a6718b8e34f)



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

#### SSL Pinning in Flutter

- [SSL Pinning in Flutter Apps](https://medium.com/@mbixjkee1392/ssl-pinning-in-flutter-apps-254e01e57965)
- [Securing Your Flutter App By Adding SSL Pinning](https://dwirandyh.medium.com/securing-your-flutter-app-by-adding-ssl-pinning-474722e38518)

```dart
Future<SecurityContext> get globalContext async {
  final sslCert = await rootBundle.load('assets/certificate.pem');
  SecurityContext securityContext = SecurityContext(withTrustedRoots: false);
  securityContext.setTrustedCertificatesBytes(sslCert.buffer.asInt8List());
  return securityContext;
}

Future<http.Client> getSSLPinningClient() async {
  HttpClient client = HttpClient(context: await globalContext);
  client.badCertificateCallback =
      (X509Certificate cert, String host, int port) => false;
  IOClient ioClient = IOClient(client);
  return ioClient;
}
```

## TO-DO List

### iOS knowleage

- Combin
- URLSession