# The thinking of macos rpath

## What is the problem?

I have tried to build the `ffmpeg` by configuration with a `--prefix=./lib`. After successful built, the `ffmpeg` can't work which have an error message.

```text
  dyld[76564]: Library not loaded: ./lib/lib/libavdevice.61.dylib
  Referenced from: <0AAAE9DA-09B7-349A-990C-AD388AE54756> /Users/xx/work/c/ffmpeg/build/lib/bin/ffmpeg
  Reason: tried: './lib/lib/libavdevice.61.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS./lib/lib/libavdevice.61.dylib' (no such file), './lib/lib/libavdevice.61.dylib' (no such file)
zsh: abort      ./ffmpeg
```
What is wrong with this buiding?

## Investigation

```text
% otool -l ffmpeg | grep libav    
         name ./lib/lib/libavdevice.61.dylib (offset 24)
         name ./lib/lib/libavfilter.10.dylib (offset 24)
         name ./lib/lib/libavformat.61.dylib (offset 24)
         name ./lib/lib/libavcodec.61.dylib (offset 24)
         name ./lib/lib/libavutil.59.dylib (offset 24)
```

```text
Load command 63
          cmd LC_RPATH
      cmdsize 32
         path @rpath/../.. (offset 12)
```

## Solution

### Rootcause

The search path have't lib.

### Implementation

- Change the lib rpath

```
install_name_tool -change ./lib/lib/libswresample.5.dylib @rpath/libswresample.5.dylib ffmpeg
```

- Add find path with executed file

It is better that add a rpath by the loader path, which means the lib/executable load path. There is a document which about the detail.
[Understanding dyld @executable_path, @loader_path and @rpath](https://itwenty.me/posts/01-understanding-rpath/)

```
install_name_tool -add_rpath @loader_path/../lib ffmpeg
```

Let's explain the executable_path / loader_path / rpath in a short message.
  - executable_path, the executable file's directory.
  - loader_path, the directory not only the executable file but also the dynamic lib when it loaded.
  - rpath, A path that help the macos to find the lib saving direcotry.

- The result after changes.

```txt
% otool -l ffmpeg | grep libav 
         name @rpath/libavdevice.61.dylib (offset 24)
         name @rpath/libavfilter.10.dylib (offset 24)
         name @rpath/libavformat.61.dylib (offset 24)
         name @rpath/libavcodec.61.dylib (offset 24)
         name @rpath/libavutil.59.dylib (offset 24)
```

```txt
Load command 63
          cmd LC_RPATH
      cmdsize 24
         path @loader_path/../lib (offset 12)
```

It's working..