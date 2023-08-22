# Control Camera

To control the map (such as moving it to a new position and zoom level), you'll need a `MapController`. The controller does not provide access to the current viewport/camera: that is the responsibility of `MapCamera`.

{% embed url="https://pub.dev/documentation/flutter_map/latest/flutter_map/MapController-class.html" %}

## Usage Inside Of A `FlutterMap` Child

To control the map from within the context of a `FlutterMap` widget, use `MapController.of(context)`.

{% hint style="info" %}
Calling this method in a `build` method will cause the widget to automatically rebuild when the `MapController` changes. See [#hooking-into-inherited-state](../../plugins/making-a-plugin/creating-new-layers.md#hooking-into-inherited-state "mention") for more information.
{% endhint %}

If this is `null` or throws a `StateError`, try wrapping the concerned widget in a `Builder`, to ensure the `FlutterMap` widget is parenting the `BuildContext`. If this has no effect, use [#usage-outside-of-fluttermap](controller.md#usage-outside-of-fluttermap "mention") instead.

## Usage Outside Of `FlutterMap`

### Initialisation

To use a `MapController`, it must initialised like any other object and then passed to the `FlutterMap`. This attaches them until the map is disposed.

```dart
final mapController = MapController();

@override
Widget build(BuildContext context) =>
    FlutterMap(
        mapController: mapController,
        // ...
    );
```

{% hint style="warning" %}
Avoid disconnecting the map from the controller, as it can cause problems. If you need to change the map's contents:

* Change its `children` (layers) individually
* Re-initialise a new `MapController`, and keep it in an external state system

If you still get issues, and `FlutterMap` is located inside a `PageView`, `ListView` or another complex lazy layout, try setting `keepAlive` `true` in `MapOptions`: [#permanent-rules](../options.md#permanent-rules "mention").
{% endhint %}

### Usage Inside `initState()`

Sometimes, it is necessary `MapController` in `initState()` before the map has been built, for example to attach an event listener ([listen-to-events.md](listen-to-events.md "mention")). This is not directly possible, as the map must be built for the controller to be attached.

Instead, use the `MapOptions.onMapReady` callback. The initialised `MapController` can be used freely within it.

```dart
final mapController = MapController();

@override
Widget build(BuildContext context) =>
    FlutterMap(
        mapController: mapController,
        options: MapOptions(
            onMapReady: () {
                mapController.mapEventStream.listen((evt) {});
                // And any other `MapController` dependent non-movement methods
            },
        ),
    );
```

{% hint style="warning" %}
`MapController` methods that change the position of the map should not be used instantly in `onMapReady` - see [issue #1507](https://github.com/fleaflet/flutter\_map/issues/1507).

Using them as a reaction to a map event is still fine.
{% endhint %}

## Animated Movements

Whilst animated movements through `MapController`s aren't built-in, the [community maintained plugin `flutter_map_animations`](https://github.com/TesteurManiak/flutter\_map\_animations) provides this, and much more!

The example application also includes a page demonstrating a custom animated map movement without the plugin.
