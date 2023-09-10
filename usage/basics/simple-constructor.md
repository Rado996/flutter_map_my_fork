# Simple Constructor

{% hint style="info" %}
The `simple` constructor is designed to get you started quickly, without having to worry about more technical aspects of the map.

However, due to this, it provides very little customizability and functionality, so it's only recommended to use this constructor for mockups or for where only an extremely simple map is required.
{% endhint %}

```dart
FlutterMap.simple(
    options: MapOptions(),
    urlTemplate: '',
    userAgentPackageName: '',
    attribution: RichAttributionWidget(),
);
```

To set up your simple map:

1. [`urlTemplate`](https://pub.dev/documentation/flutter\_map/latest/flutter\_map.plugin\_api/TileLayer/urlTemplate.html): Provide a standard slippy map URL template, with `{x}`, `{y}`, and `{z}` placeholders (variable subdomain support is not included with the `simple` constructor).
2. [`userAgentPackageName`](https://pub.dev/documentation/flutter\_map/latest/flutter\_map.plugin\_api/TileLayer/tileProvider.html): Provide the application's correct package name, such as 'com.example.app', to allow the tile server to identify your application.
3. `attribution` ([attribution-layer.md](../../layers/attribution-layer.md "mention")): Optionally, provide either a `RichAttributionWidget` or `SimpleAttributionWidget` to give appropriate credit to your tile server.
4. `options` ([options](../options/ "mention")): Finally, configure the rules for your map in the normal way
