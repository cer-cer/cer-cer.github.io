# Render flow


```txt
To composite the tree, create a ui.SceneBuilder object using RendererBinding.createSceneBuilder,<br> pass it to the root Layer object's addToScene method, and then call ui.SceneBuilder.build to obtain a ui.Scene.<br> A ui.Scene can then be painted using ui.FlutterView.render.
```

```plantuml
@startuml
package "dart:ui" {
    class RendererBinding {
        PictureRecorder createPictureRecorder()
        Canvas createCanvas(PictureRecorder recorder)
        SceneBuilder createSceneBuilder()
    }

    abstract PictureRecorder {
        Picture endRecording()
    }

    class Canvas {
        factory Canvas(PictureRecorder recorder, [Rect? cullRect])
        // encapsulation the draw method
    }

    class SceneBuilder {
        void addPicture(Offset offset, Picture picture, ...)
        Scene build()
    }
    class Scene
    class Picture

    class FlutterView {
        void render(Scene scene, {Size? size,})
    }
}

package "dart:rendering" {
    abstract Layer{
        void addToScene( SceneBuilder builder )
    }

    class PictureLayer {
        Picture? get picture
    }

    PictureLayer -|> Layer
}

FlutterView -> Scene
Scene "1" -> "*" Layer

Layer "1" -> "1" Picture

@enduml
```