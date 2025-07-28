# Flutter common wdiget

- [Widget catalog](https://docs.flutter.dev/ui/widgets)
- [Drag and drop UI elements in Flutter with Draggable and DragTarget](https://blog.logrocket.com/drag-and-drop-ui-elements-in-flutter-with-draggable-and-dragtarget/)
- [FLUTTER DRAGGABLE & DRAGTARGET](https://medium.com/@surya.sh/flutter-draggable-dragtarget-9b47a99f874a)

## Base Function class

- BoxConstraints

盒子约束

  - `BoxConstraints.tight`

```dart
/// Creates box constraints that is respected only by the given size.
  BoxConstraints.tight(Size size)
    : minWidth = size.width,
      maxWidth = size.width,
      minHeight = size.height,
      maxHeight = size.height;
```

  - `const BoxConstraints.tightFor({double? width, double? height})`

```dart
  /// Creates box constraints that require the given width or height.
  ///
  /// See also:
  ///
  ///  * [BoxConstraints.tightForFinite], which is similar but instead of
  ///    being tight if the value is non-null, is tight if the value is not
  ///    infinite.
  const BoxConstraints.tightFor({double? width, double? height})
    : minWidth = width ?? 0.0,
      maxWidth = width ?? double.infinity,
      minHeight = height ?? 0.0,
      maxHeight = height ?? double.infinity;
```

  - `const BoxConstraints.tightForFinite`

`tight` 系列函数： 如果设置了大小，则必须精确到这个尺寸。如果没有设置，则最大值 `double.infinity`

  - `BoxConstraints.loose(Size size)`

```dart
/// Creates box constraints that forbid sizes larger than the given size.
  BoxConstraints.loose(Size size)
    : minWidth = 0.0,
      maxWidth = size.width,
      minHeight = 0.0,
      maxHeight = size.height;
```

`loose` 宽松限制，最大值为指定值。

  - `const BoxConstraints.expand({double? width, double? height})`

```dart
/// Creates box constraints that expand to fill another box constraints.
  ///
  /// If width or height is given, the constraints will require exactly the
  /// given value in the given dimension.
  const BoxConstraints.expand({double? width, double? height})
    : minWidth = width ?? double.infinity,
      maxWidth = width ?? double.infinity,
      minHeight = height ?? double.infinity,
      maxHeight = height ?? double.infinity;
```



1. Container

容器类，组合了其他widget，以便于使用。

- StatelessWidget
- 组合widget，它组合了以下控件
  - LimitedBox  `当没有child时，限制大小为0.`
  - Align `对齐`
  - ColoredBox `颜色`
  - Padding 
  - DecoratedBox
  - ConstrainedBox
  - Transform
  - ClipPath

2. SizedBox

设置子控件大小

- SingleChildRenderObjectWidget
- RenderConstrainedBox

若设置大小，则精确到该尺寸；若不设置，则使用 `BoxConstraints.tightFor` 用最大值。

```dart
BoxConstraints get _additionalConstraints {
    return BoxConstraints.tightFor(width: width, height: height);
  }
```

3. ConstrainedBox

- SingleChildRenderObjectWidget
- RenderConstrainedBox

实现的方案和`SizedBox` 一样，但有使用者直接设置约束。

4. LimitedBox

- SingleChildRenderObjectWidget
- RenderLimitedBox

若parent传递的约束有边界，则使用parent的。若无，则使用设置的limited value。
```dart
BoxConstraints _limitConstraints(BoxConstraints constraints) {
    return BoxConstraints(
      minWidth: constraints.minWidth,
      maxWidth:
          constraints.hasBoundedWidth ? constraints.maxWidth : constraints.constrainWidth(maxWidth),
      minHeight: constraints.minHeight,
      maxHeight:
          constraints.hasBoundedHeight
              ? constraints.maxHeight
              : constraints.constrainHeight(maxHeight),
    );
  }
```

