# Creating New Tile Providers

One common requirement is a custom `TileProvider`, and potentially a custom `ImageProvider` inside. This will allow your plugin to intercept all tile requests made by a map, and take your own action(s), before finally returning a tile.

{% hint style="info" %}
Check the [list.md](../list.md "mention") for providers that already implement the behaviour you wish to replicate.
{% endhint %}

Some useful steps are below, but there are more properties of a `TileProvider` that can be overridden.

## Extending `TileProvider`

To create your own usable `TileProvider`, the first step is making a class that `extends` the abstract class. It is not recommended to `implement` the base, as this will require overriding methods unnecessarily for most custom providers.

```dart
class CustomTileProvider extends TileProvider {
    CustomTileProvider({
        // Suitably initialise your own custom properties
        super.headers, // Allow a `Map` of custom HTTP headers
    })
}
```

### Overriding `getImage`

The majority of custom `TileProviders` will want to implement their own method to retrieve tiles. This is done by overriding the `getImage` method, and usually specifying your own `ImageProvider`, for example:

```dart
    @override
    ImageProvider getImage(TileCoordinates coords, TileLayer options) =>
        CustomImageProvider(
            options: options,
            coords: coords,
            headers: {
                ...headers,
                'User-Agent': headers['User-Agent'] == null
                    ? '<pluginName> for flutter_map (unknown)'
                    : '<pluginName> for ${headers['User-Agent']}',
            },
        );
```

{% hint style="info" %}
Notice that the 'User-Agent' header has been changed. This is recommended, as it helps further differentiate your plugin's traffic from vanilla 'flutter\_map' traffic.
{% endhint %}

### Overriding `getTileUrl`

Some custom `TileProviders` may want to change the way URLs for tiles are generated. Note that this is quite advanced usage.

```dart
    @override
    String getTileUrl(TileCoordinates coordinates, TileLayer options) {
        // Custom generation
    }
```
