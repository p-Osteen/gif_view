[![pub package](https://img.shields.io/pub/v/gif_view.svg)](https://pub.dev/packages/gif_view)

# GifView

Load [GIF](https://pt.wikipedia.org/wiki/GIF)|[APNG](https://pt.wikipedia.org/wiki/Animated_Portable_Network_Graphics) images and can set framerate

## Features

With `GifView` you can load GIF images of easy way and can configure frameRate.

- Load from `Assets`;
- Load from `Network`;
- Load from `Memory`;
- Set frame rate;
- Set `progress` while loading GIF

## Getting started

Add `gif_view` as a [dependency in your pubspec.yaml file](https://flutter.dev/using-packages/).

## Usage

### GIF from Asset

```dart
  GifView.asset(
    'assets/gif1.gif',
    height: 200,
    width: 200,
    frameRate: 30, 
  )
```


### GIF from Network

```dart
  GifView.network(
    'https://www.showmetech.com.br/wp-content/uploads/2015/09/happy-minion-gif.gif',
    height: 200,
    width: 200,
  )
```


### GIF from Memory

```dart
  GifView.memory(
    _bytes,
    height: 200,
    width: 200,
  )
```

## Attributes

| Name | Description | Default |
|------|-------------|---------|
| controller | Optional controller to manage GIF playback externally | - |
| frameRate | Duration between frames in milliseconds. If null, uses original GIF frame rate | - |
| height | The height of the GIF view widget | - |
| width | The width of the GIF view widget | - |
| fit | How to fit the image within its bounds (BoxFit enum) | BoxFit.contain |
| color | Color to blend with the image | - |
| colorBlendMode | Blend mode for color overlay | - |
| alignment | Alignment of the image within its bounds | `Alignment.center` |
| imageRepeat | How to repeat the image | `ImageRepeat.noRepeat` |
| centerSlice | Center slice for nine-patch scaling | - |
| matchTextDirection | Whether to match text direction | `false` |
| invertColors | Whether to invert image colors | `false` |
| filterQuality | Quality of image filtering | `FilterQuality.low` |
| isAntiAlias | Whether to use anti-aliasing | `false` |
| withOpacityAnimation | Whether to use fade-in animation | `true` |
| fadeDuration | Duration of fade-in animation | `Duration(milliseconds: 300)` |
| autoPlay | Whether to auto-start playback | `true` |
| loop | Whether to loop the animation | `true` |
| playInverted | Whether to play in reverse initially | `false` |
| onFinish | Callback when animation completes | - |
| onStart | Callback when animation starts | - |
| onFrame | Callback for each frame change | - |
| onLoaded | Callback when GIF is loaded with frame count | - |
| errorBuilder | Builder for error state widget | - |
| progressBuilder | Builder for loading state widget | - |
| scale | Scale factor for network/memory images | `1.0` |
| headers | HTTP headers for network requests | - | 


## Controller

The `GifController` provides programmatic control over GIF playback:

```dart
GifController controller = GifController();

// Playback control
controller.play({bool? inverted, int? initialFrame});  // Start/resume playback
controller.pause();                                    // Pause playback
controller.stop();                                     // Stop and reset to first frame

// Seeking
controller.seek(34);                                   // Seek to specific frame
controller.seekToProgress(0.5);                        // Seek to 50% of animation

// Status and progress
GifStatus status = controller.status;                  // Current playback status
double progress = controller.progress;                 // Current progress (0.0 to 1.0)

// Available statuses: loading, playing, stopped, paused, reversing, completed, error
```

## Controller Example

```dart
class MyPage extends StatefulWidget {
  const MyPage({super.key});

  @override
  State<MyPage> createState() => _MyPageState();
}

class _MyPageState extends State<MyPage> {
  late final GifController controller;

  @override
  void initState() {
    super.initState();
    controller = GifController();
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GifView.network(
        'https://www.showmetech.com.br/wp-content/uploads/2015/09/happy-minion-gif.gif',
        controller: controller,
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          if (controller.status == GifStatus.playing) {
            controller.pause();
          } else {
            controller.play();
          }
        },
        child: Icon(
          controller.status == GifStatus.playing
              ? Icons.pause
              : Icons.play_arrow,
        ),
      ),
    );
  }
}
```

## Cache Management

### Pre-fetching Images

GifView provides a static `preFetchImage` method to load and cache GIF images ahead of time for better performance:

```dart
// Pre-fetch different types of images
await GifView.preFetchImage(AssetImage('assets/my-gif.gif'));
await GifView.preFetchImage(NetworkImage('https://example.com/gif.gif'));
await GifView.preFetchImage(MemoryImage(bytes));
await GifView.preFetchImage(FileImage(File('path/to/gif.gif')));
```

### Clearing Cache

```dart
await GifView.clearCache();
```

### Custom Cache Provider

You can implement your own caching strategy by setting a custom cache provider:

```dart
class MyCustomCacheProvider implements GifCacheProvider {
  @override
  Future<void> set(String key, Uint8List data) async {
    // Custom cache implementation
  }

  @override
  Future<Uint8List?> get(String key) async {
    // Custom retrieval implementation
  }

  @override
  Future<void> clear() async {
    // Custom clear implementation
  }
}

// Set custom provider
GifView.setCacheProvider(MyCustomCacheProvider());

// Revert to default provider
GifView.setCacheProvider(null);
```

## Credits

This package is a fork/continuation of the original [gif_view](https://github.com/RafaelBarbosatec/gif_view) package by Rafael Barbosa. We acknowledge and thank the original author for their work on this Flutter GIF viewing library.
