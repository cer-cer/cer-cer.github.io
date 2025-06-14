# Flutter App launch

All the code refence from flutter version 3.29.3, dart 3.7.2.

## RunApp

`[runApp] which bootstraps a widget tree and renders it into a default [FlutterView]`

```dart
void runApp(Widget app) {
  final WidgetsBinding binding = WidgetsFlutterBinding.ensureInitialized();
  _runWidget(binding.wrapWithDefaultView(app), binding, 'runApp');
}
```

### WidgetsFlutterBinding.ensureInitialized

```plantuml
@startuml
abstract BindingBase {
    initInstances() // delay to implementation
    initServiceExtensions()
}
class WidgetsFlutterBinding
class WidgetsBinding

WidgetsFlutterBinding -up-|> BindingBase
WidgetsFlutterBinding -left-o WidgetsBinding
@enduml
```

The method `initInstances` will be called in class `WidgetsFlutterBinding` 's constructor. which will set the `WidgetsBinding`'s instance.

### _runWidget

Method `_runWidget` will build the element tree.

```dart
void _runWidget(Widget app, WidgetsBinding binding, String debugEntryPoint) {
  binding
    ..scheduleAttachRootWidget(app)
    ..scheduleWarmUpFrame();
}
```

```plantuml
@startuml

|WidgetBinding|
start

:attachRootWidget;
note
Wrap the `app` with `RootWidget`
end note
:attachToBuildOwner;

|RootWidget|
:attach();

if (element == null) then(Y)
:mount;
else (N)
:markNeedsBuild;
endif

|WidgetBinding|
:SchedulerBinding.ensureVisualUpdate;
end

@enduml
```


### WidgetsFlutterBinding.wrapWithDefaultView

Generate the `View` widget which contain a `FlutterView` will be drawn. The `View` also includes 

```dart
Widget wrapWithDefaultView(Widget rootWidget) {
    return View(
      view: platformDispatcher.implicitView!,
      deprecatedDoNotUseWillBeRemovedWithoutNoticePipelineOwner: pipelineOwner,
      deprecatedDoNotUseWillBeRemovedWithoutNoticeRenderView: renderView,
      child: rootWidget,
    );
  }
```

The `View` 's purpose is building the render tree.

```plantuml
@startuml
class StatefulWidget
class StatelessWidget
class RenderObjectWidget
class InheritedWidget
class FlutterView

class View
class RawView
class _RawViewInternal
class _ViewScope

View -up-|> StatefulWidget
RawView -up-|> StatelessWidget
_RawViewInternal -right-|> RenderObjectWidget
_ViewScope -up-|> InheritedWidget

View -* RawView
RawView -down-* _RawViewInternal
RawView -down-* _ViewScope
_RawViewInternal -left-* FlutterView
_ViewScope -* FlutterView

@enduml
```

## Build child element tree

After attach to root widget, the build owner will start to `mount` child which will build a element tree.

This is the flow of mount belong to `RootElement`.

```plantuml
@startuml

|RootWidget|
:attach;
if (element == null) then(y)
:Create element;
|RootElement|

:mount;
note right
The mount method has flow of _rebuild
end note
|Element|
:mount;
note right
{{
    :set parent/slot;
    :set build owner;
    :register global key to buildOwner;

    :update inherit element;
    :attrach to notification tree;
}}
end note

|RootElement|

:_rebuild;

|Element|
:updateChild;
note right
purpose:
Generator child element accroding the child widget.

{{
    if (newWidget == null) then(y)
        :deActive element;
    else (n)
        if (child == null) then(y)
            :inflateWidget;
        else (n)
            if (same element class) then(y)
                if (same new widget) then(y)
                    :update the child just by assign;
                else (n)
                    :call element.Update(child);

                endif
            else (n)
                :inflateWidget;
            endif
        endif
    endif
}}
end note

:inflateWidget;

note right
Since there is no child element, so it always call inflateWidget to create a new child element.
This is the subflow of `inflateWidget`.

{{
    if (widget's key is globlaKey) then(y)
        :get element from buildOwner;
    else (n)
        :child = newWidget.createElement();
        :child.mount();
    endif
}}
loop to call child's mount
end note

else (n)
|Element|
:markNeedsBuild;
note right
Add this element to buildScope's dirty list
endnote
endif

|RootWidget|

@enduml
```

The render tree will aslo be built when element changed.

After that, it will ask to refresh to screen.