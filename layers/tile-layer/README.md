# Tile Layer

The basis of any map is a `TileLayer`, which displays square raster images in a continuous grid, sourced from the Internet or a local file system.

flutter\_map supports [wms-usage.md](wms-usage.md "mention"), but most map tiles are accessed through Slippy Map/CARTO/XYZ URLs.

{% embed url="https://pub.dev/documentation/flutter_map/latest/flutter_map.plugin_api/TileLayer-class.html" %}

```dart
TileLayer(
  urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
  userAgentPackageName: 'dev.fleaflet.flutter_map.example',
  // Plenty of other options available!
),
```

{% hint style="warning" %}
You must comply with the appropriate restrictions and terms of service set by your tile server. Failure to do so may lead to any punishment, at the tile server's discretion.

This library and/or the creator(s) are not responsible for any violations you make using this package.

_The OpenStreetMap Tile Server (as used above) ToS can be_ [_found here_](https://operations.osmfoundation.org/policies/tiles)_. Other servers may have different terms._
{% endhint %}

## URL Template

{% hint style="success" %}
The `urlTemplate` parameter must be specified unless [`wmsOptions`](wms-usage.md) is specified.
{% endhint %}

The URL template is a string that contains placeholders, which, when filled in, create a URL/URI to a specific tile.

Specifically, flutter\_map uses the Slippy Map format, sometimes referred to as CARTO or Raster XYZ. Tiles are referred to by their zoom level, and position on the X & Y axis. For more information, read [explanation](../../explanation/ "mention").

These templates are usually documented by your tile server, and will always include the following placeholders:

* `{x}`: x axis coordinate
* `{y}`: y axis coordinate
* `{z}`: zoom level

Sometimes, they also include:

* `{s}`: subdomain\
  Now usually [considered redundant](https://github.com/openstreetmap/operations/issues/737) due to the usage of HTTP/2 & HTTP/3, this bypassed browsers' limitations on simultaneous HTTP connections\
  To use subdomains, also configure the `TileLayer.subdomains` parameter.
* `{r}` or `@2x`: retina mode\
  Request high-definition tiles from the tile server

{% hint style="warning" %}
`TileLayer.retinaMode` can be used to emulate retina tiles. Do not combine it with an `{r}` or `@2x` placeholder.

It does not change whether the `{r}` placeholder is filled or emptied (it is always filled).
{% endhint %}

Additional placeholders can also be added freely to the template, and are filled in with the specified values in `additionalOptions`.

## `userAgentPackageName`

{% hint style="success" %}
Although it is programatically optional, always specify the `userAgentPackageName` argument to avoid being blocked by your tile server.
{% endhint %}

This parameter should be passed the application's package name, such as 'com.example.app'. This is important to avoid blocking by tile servers due to high-levels of unidentified traffic. If no value is passed, it defaults to 'unknown'.

This is then formatted into a 'User-Agent' header, and appended to the `TileProvider`'s `headers` map, if it is not already present.

This is ignored on the web, where the 'User-Agent' header cannot be changed due to a limitation of Dart/browsers.

## Tile Providers

{% hint style="success" %}
If a large proportion of your users use the web platform, it is preferable to use `CancellableNetworkTileProvider`, instead of the default `NetworkTileProvider`.

See [#cancellablenetworktileprovider](tile-providers.md#cancellablenetworktileprovider "mention") for more information.
{% endhint %}

Need more control over how the URL template is interpreted and/or tiles are fetched? You'll need to change the `TileProvider`.

{% content-ref url="tile-providers.md" %}
[tile-providers.md](tile-providers.md)
{% endcontent-ref %}
