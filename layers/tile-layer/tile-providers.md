# Tile Providers

The `tileProvider` parameter in `TileLayer` takes a `TileProvider` object specifying a [tile provider](../../explanation/#tile-providers) to use for that layer.

This has a default of `NetworkTileProvider` which gets tiles from the internet through a dedicated image provider.

There's two situations in which you'll need to change the tile provider:

* Sourcing tiles from the filesystem or asset store: [#local-tile-providers](tile-providers.md#local-tile-providers "mention")
* Using a [plugin](../../plugins/list.md) that instructs you to do so ([creating-new-tile-providers.md](../../plugins/making-a-plugin/creating-new-tile-providers.md "mention"))

## Network Tile Providers

These tile providers use the `urlTemplate` to get the appropriate tile from the a network, usually the World Wide Web.

### `NetworkTileProvider`

This is the default tile provider, and does nothing particularly special. It takes two arguments, but you'll usually never need to specify them:

* `httpClient`: `BaseClient`\
  By default, a `RetryClient` backed by a standard `Client` is used
* `headers`: `Map<String, String>`\
  By default, only headers sent by the platform are included with each request, plus an overridden (where possible) 'User-Agent' header based on the [#useragentpackagename](./#useragentpackagename "mention") property

### [`CancellableNetworkTileProvider`](https://github.com/fleaflet/flutter\_map\_cancellable\_tile\_provider)

{% hint style="info" %}
This requires the '[flutter\_map\_cancellable\_tile\_provider](https://github.com/fleaflet/flutter\_map\_cancellable\_tile\_provider)' plugin to be installed.

This plugin is part of the official flutter\_map organisation, and maintained by this library's maintainers.
{% endhint %}

If a large proportion of your users use the web platform, it is preferable to use this tile provider instead of the default `NetworkTileProvider`.

It could:

* Reduce tile loading durations
* Reduce costly tile requests to tile servers\*[^1]
* Reduce (cellular) data consumption

This provider uses '[dio](https://pub.dev/packages/dio)' to support cancelling/aborting unnecessary HTTP requests in-flight. Tiles that are removed/pruned before they are fully loaded do not need to complete loading, and therefore do not need to complete the HTTP interaction. Closing the connection in this way frees it up for other tile requests, and avoids downloading unused data.

There's no reason not use this tile provider, unless you'd rather avoid having 'dio' as a dependency. Once HTTP request abortion is [added to Dart's 'native' 'http' package (requested and PR opened)](https://github.com/dart-lang/http/issues/424), `NetworkTileProvider` will be updated to take advantage of it, replacing this provider.

## Local Tile Providers

These tile providers use the `urlTemplate` to get the appropriate tile from the asset store of the application, or from a file on the users device, respectively.

{% hint style="warning" %}
Specifying any `fallbackUrl` (even if it is not used) in the `TileLayer` will reduce the performance of these providers.

It will cause [23% slower asset tile requests](https://github.com/fleaflet/flutter\_map/issues/1436#issuecomment-1569663004) with `AssetTileProvider`,  and will cause main thread blocking when requesting tiles from `FileTileProvider`.
{% endhint %}

### `AssetTileProvider`

This tile providers uses the `templateUrl` to get the appropriate tile from the asset store of the application.

### `FileTileProvider`

This tile providers uses the `templateUrl` to get the appropriate tile from the a path/directory/file on the user's device - either internal application storage or external storage.

{% hint style="warning" %}
On the web, `FileTileProvider()` will throw an `UnsupportedError` when a tile request is attempted, due to the lack of the web platform's access to the local filesystem.

If you know you are running on the web platform, use a [`NetworkTileProvider`](tile-providers.md#network-tile-provider) or a custom tile provider.
{% endhint %}

## Offline Mapping

{% content-ref url="../../tile-servers/offline-mapping.md" %}
[offline-mapping.md](../../tile-servers/offline-mapping.md)
{% endcontent-ref %}

[^1]: Dependent on whether the tile server clocks a request at the start of the response/download or at the end
