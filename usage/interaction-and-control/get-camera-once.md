# Get Camera Once

The `MapCamera` object describes the map's current viewport. There's two ways to retrieve it a single time.

<mark style="background-color:orange;">\<insert link></mark>

{% hint style="info" %}
The `MapCamera` object also provides access to some other helpful methods that depend on it, such as `pointToLatLng` & `latLngToPoint`.
{% endhint %}

## Usage Inside Of A `FlutterMap` Child

To get the camera from within the `BuildContext` of the `FlutterMap` widget, use `MapCamera.maybeOf(context)`.

If this is `null` or throws a `StateError`, try wrapping the concerned widget in a `Builder`, to ensure the `FlutterMap` widget is parenting the `BuildContext`. If this has no effect, use [#usage-outside-of-fluttermaps](get-camera-once.md#usage-outside-of-fluttermaps "mention") instead.

## Usage Outside Of `FlutterMap`s

To get the camera from outside the context of the `FlutterMap` widget, you'll need to setup a `MapController` first: see [controller.md](controller.md "mention")>[#usage-outside-of-fluttermaps](controller.md#usage-outside-of-fluttermaps "mention").

Then, use the `.camera` getter.
