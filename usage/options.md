# Options

To dictate & restrict what the map can and should do, regardless of its contents, it needs some guidance!

It provides options that can be categorized into three main parts:

* [Initial positioning](options.md#initial-positioning)\
  Defines the location of the map when it is first loaded
* [Permanent rules](options.md#permanent-rules)\
  Defines restrictions that last throughout the map's lifetime
* [Event handling](interaction-and-control/listen-to-events.md)\
  Defines methods that are called on specific map events

{% embed url="https://pub.dev/documentation/flutter_map/latest/flutter_map/MapOptions-class.html" %}

## Initial Positioning

{% hint style="info" %}
Changing these properties after the map has already been built for the first time will have no effect: they only apply on initialisation.

To control the map programatically, use a `MapController`: [controller.md](interaction-and-control/controller.md "mention").
{% endhint %}

One part of `MapOptions` responsibilities is to define how the map should be positioned when first loaded. There's two ways to do this (that are incompatible):

* `initialCenter` (`LatLng`) & `initialZoom`
* `initialCameraFit`
  * by bounds (circumscribed[^1]): `CameraFit.bounds`
  * by bounds (inscribed[^2]): `CameraFit.insideBounds`
  * by coordinates (circumscribed[^3]): `CameraFit.coordinates`

It is possible to also set the map's `initialRotation` in degrees, if you don't want it North (0°) facing initially.

If rotation is enabled/allowed, if using `initialCameraFit`, prefer defining it by coordinates for a more intended/tight fit.

## Permanent Rules

One part of `MapOptions` responsibilities is to define the restrictions and limitations of the map and what users can/cannot do with it.

You should check [all the available options](https://pub.dev/documentation/flutter\_map/latest/flutter\_map/MapOptions-class.html), but these are recommended for most maps:

* `maxZoom` (and `minZoom`)
* `cameraConstraint`
  * camera bounds inside bounds: `CameraConstraint.bounds`
  * camera center inside bounds: `CameraConstraint.center`
  * _unconstrained (default): `CameraConstraint.unconstrained`_
* `interactionOptions`\
  Limits what methods the user can interact with the map through (for example, preventing rotation)
* `keepAlive`\
  If `FlutterMap` is located inside a `PageView`, `ListView` or another complex lazy layout, set this `true` to prevent the map from resetting to the [#initial-positioning](options.md#initial-positioning "mention") on rebuild

{% hint style="success" %}
Instead of `maxZoom` (or in addition to), consider setting `maxNativeZoom` per `TileLayer` instead, to allow tiles to scale (and lose quality) on the final zoom level, instead of setting a hard limit.
{% endhint %}

[^1]: Bounds inside camera

[^2]: Camera inside bounds

[^3]: Coordinates inside camera, as tightly as possible
