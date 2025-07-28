# Flutter TMP


## Performance

- [17 个提高性能的 Flutter 最佳实践](https://inficial.medium.com/flutter-best-practices-for-improve-performance-7e21e14efebb)
  - Avoid rebuilding all the widgets repetitively
  - And it would be a good idea to add const.
  - Use nil instead empty Container
  - User itemExtent in ListView for long lists.
  - Use for/while instead of foreach/map
  - Precache your images and icons
  - Consider using the RepaintBoundary widget
  - Use builder named constructors if possible
- [Flutter Under the Hood: Owners](https://medium.com/@mbixjkee1392/flutter-under-the-hood-owners-2ec741d45bea)


## Flutter layout

Render Objects are responsible for layout. Three rules make it possible:

- Constraints go down by the tree from the parents to the children.
- Sizes go up by the tree from the children to the parents.
- Parents set the position of the children.

## Flutter Overflow

- [method](paintOverflowIndicator)
  - RenderConstraintsTransformBox
  - RenderFlex
    - Flex will report overflow if the items totoal size bigger than real bound.
  - RenderStack
    - 

## Flutter animation

- [Scrolling Animation in Flutter](https://medium.com/flutter-community/scrolling-animation-in-flutter-6a6718b8e34f)

## Interview

- [flutter-interview-questions](https://github.com/thisissandipp/flutter-interview-questions?tab=readme-ov-file#what-is-the-difference-between-widgetsapp-and-materialapp-in-flutter)

- State
  - Wideget state
    1. createState
    2. mounted
    3. initState
    4. didChangeDependce
    5. parentWidget update -> didUpdateWidget
    6. hotload -> reassemble
    7. build()
    8. deactive (remove from subtree)
    9.  dispose (release)
  - App state
    1. detach
    2. resume
    3. inactive (forground but can't accept input)
    4. hidden (background, all view are hidden)
    5. pause 
- await
  - until the async operator is finished, the await operation always interrupts the process flow.
- Null-aware
  - optional. 
- BuildContext
  - Which used to identify or locate the widget in the widget tree. which is an element.
- Hot reload vs Hot restart
  - hot reload, the vm compile the changes code and reload the changes.
  - Hot restart, the vm compile all the changes and restart the dart app. The preserved states of dart app will be destoryed.
- Streams
  - Signal subscriber stream
  - Brodcast subscriber stream
  - StreamBuilder
- Keys
  - Keys are used in Flutter as identifiers for widgets, elements, and semantic nodes.
  - Within the widget tree, keys are responsible for preserving the state of modified widgets.
- runApp() vs main()
  - main(), the dart app's code entry
  - runApp(), attach the root widget and start the builder.
- Mixin vs extension
  - Dart minxin. for the code reuse with no related class.
    - field
    - method
    - constructor method(N)
  - extension, add new method to the exist class.
    - set/get
    - method
    - constructor method(N)
    - field (N)
- [Tree Shaking](https://medium.com/@samra.sajjad0001/flutter-tree-shaking-optimizing-your-app-for-performance-9a2d82b43eb1)
  - Tree shaking in Flutter works by analyzing the dependencies of each widget and only including the widgets that are actually used in the final build. This means that if a widget or library is not used, it will not be included in the final build, reducing the size and improving the performance of the application.
  - Enable `tree shake` in `pubspec` file.
  - Use Dart’s Strong Mode, special sdk version.
  - Minimize Imports
  - Remove unuse code
  - [barrel export](https://medium.com/@ugamakelechi501/barrel-files-in-dart-and-flutter-a-guide-to-simplifying-imports-9b245dbe516a)
- [Understanding import, export, part, and part of in Dart](https://medium.com/@irfandev/understanding-import-export-part-and-part-of-in-dart-c6c01c561682)
  - `part of` define current file belong to a library.
  - `part` describe that some files belong to current file (library)
  - `import` xxx `show` method. make only method visible.
- Typedef of dart
  - Use to define a method alian.
- What is isolate in Flutter
  - an executor of flutter.
  - Zone, an executor enviorment.
- Factory constructor
  - return an object of class/ subclass / no relation class.
- Key
  - GlobalKey
  - LocalKey
    - Keys must be unique amongst the Elements with the same parent. 
- ValueNotifier
  - ValueListenableBuilder
- What is a MediaQuery in Flutter
  - a MediaQuery is a widget that provides information about the device's screen size, orientation, and other display-related characteristics.
- AnimatedSwitcher
  - Use to switcher widget by transition.
  - Use the key for builder to identi the new widget.
- InheritedWidget
  - InheritedModel
  - InheritedNotifier
- Listenable, 这个体系没有依赖framework，所以的builder需要一个参数listenable。
  - ListenableBuilder
  - ChangeNotifier
  - ValueListenable
    - ValueNotifier
- Notification
  - NotificationListener
- localization
  - vscode plugin: Flutter intl
  - Add `Flutter_localizations`
  - Add location with meterial app
- How do you implement a custom transition between screens in flutter
- JIT VS AOT
  - JIT: just in time
    - JIT compiles code during runtime. Used in development (e.g., hot reload) but has slower performance.
  - AOT: aheader of time
    - AOT compiles code before runtime (during build). Used in production for faster performance and optimized binary size.
- Object vs Dynmaic vs var
  - Object the base class of dart.
  - Dynmaic, Allows any type, similar to Object, but without the need for casting.
  - var, type inferred when value assgin.
- rending pipline
  - builder wdiget/element tree.
  - render object tree
  - layout with render object
    - layout boundry.
  - painting. call render object's paint method
    - repaint boundry
    - building layer tree.
  - compositing, compositing the layer.
  - rasterization, flutter engine rasterzating. (GPU)
  - GPU render

## Secure Code

### SSL Pinning in Flutter

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