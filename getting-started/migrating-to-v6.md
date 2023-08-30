# Migrating To v6

{% hint style="info" %}
This update has renewed two of the oldest surviving sections of 'flutter\_map' (state/`MapController` and `TileProvider`s), fixed bugs, and added features!

This is significant progress in our aim to renew the project and bring it up to date. In the long run, this will bring it inline with up-to-date Flutter good practises and techniques, improve its performance and stability, and reduce the maintenance burden.
{% endhint %}

There are major breaking changes for all users, as well as some things users should check and possibly change.

Some changes have deprecations and messages, some do not. Please refer to the sections below for information on how to migrate your project, as well as in-code documentation and deprecation messages, if your migration is not listed below.

Some changes are omitted if they are deemed unlikely to affect implementations.

{% embed url="https://github.com/fleaflet/flutter_map/blob/master/CHANGELOG.md" %}
Full Changelog
{% endembed %}

## Important Non-Migration Recommendations

{% hint style="success" %}
If you're developing an app for the web, there's an exciting new performance boost available, that must be installed manually! See [#cancellablenetworktileprovider](../layers/tile-layer/tile-providers.md#cancellablenetworktileprovider "mention") for more information.
{% endhint %}

{% hint style="warning" %}
If you're using subdomains with OpenStreetMap's tile server, use the non-subdomained `urlTemplate` instead. See [https://github.com/fleaflet/flutter\_map/issues/1631](https://github.com/fleaflet/flutter\_map/issues/1631) and [https://github.com/openstreetmap/operations/issues/737](https://github.com/openstreetmap/operations/issues/737) for more information.
{% endhint %}

{% hint style="warning" %}
If you're compiling a web app, always use the CanvasKit renderer instead of the HTML renderer. See [#web](installation.md#web "mention") for more information.
{% endhint %}

## General/Misc

<details>

<summary><code>MapController</code> should not be used directly to read information about the map's position</summary>

`MapController` now only controls the map's position/viewport/camera. The map's position is now described by `MapCamera`.

There are multiple possibilities for migration:

1. If inside the `FlutterMap` context, prefer using `MapCamera.of(context)`
2. Otherwise, use `MapController` in the same way, but use the `.camera` getter to retrieve the `MapCamera`.

See [interaction-and-control](../usage/interaction-and-control/ "mention") for more information.

</details>

<details>

<summary><code>CustomPoint</code> has been replaced by extension methods on <code>Point</code></summary>

[Extension methods](https://dart.dev/language/extension-methods) are now used to add the required functionality to the standard 'dart:math' `Point` object.

To migrate, most cases should just need to replace all occurrences of `CustomPoint` with `Point`.

</details>

<details>

<summary><code>TileLayer.backgroundColor</code> has been replaced by <code>MapOptions.backgroundColor</code></summary>

This will simplify the developer experience when using multiple overlaid `TileLayer`s, as `Colors.transparent` will no longer need to be specified. There is no reason that multiple `TileLayer`s would each need to have a different (non-transparent) background colors, as the layers beneath would be invisible and therefore pointless.

Therefore, `TileLayer`s now have transparent backgrounds, and the new `MapOptions.backgroundColor` property sets the background color of the entire map.

To migrate, move any background colour specified on the bottom-most `TileLayer` to `MapOptions`.

</details>

<details>

<summary><code>nonRotatedChildren</code> has been removed</summary>

The approach to 'mobile' and 'static' layers has been changed. Mobile layers now wrap themselves in a `MobileLayerTransformer` which uses the inherited state, instead of `FlutterMap` applying the affects directly to them. Static layers should now ensure they use `Align` and/or `SizedBox.expand`.

This has been done to simplify setup, and allow for placing static layers between mobile layers.

</details>

<details>

<summary><code>AnchorAlign</code> has been replaced by native <code>Alignment</code></summary>

`AnchorAlign` has been removed without deprecation, because `Alignment`/`AlignmentGeometry` can provide the same alignment positions/values.

To migrate, replace occurences of `AnchorAlign` with the respective `Aligment`. Note that only the pre-provided `Alignment`s should be used.

</details>

## Map Options

<details>

<summary><code>MapOptions.</code> <code>center</code>, <code>bounds</code>, <code>zoom</code>, and <code>rotation</code> have been replaced with <code>initialCenter</code>, <code>initialCameraFit</code>, <code>initialZoom</code>, and <code>initialRotation</code></summary>

These have been renamed for clarity, as well as to better fit the change into using a documented 'camera' and increasing customizability.

To migrate, rename the properties, and also check the in-code documentation and new objects for information.

</details>

<details>

<summary><code>MapOptions.maxBounds</code> has been replaced with <code>MapOptions.cameraConstraint</code></summary>

This is part of to better fit the change into using a documented 'camera' and increasing customizability.

To migrate, rename the properties, and also check the in-code documentation and new objects for information.

</details>

## Tile Providers & `templateFunction`

<details>

<summary>Implementations should switch to extensions</summary>

It is not recommended to implement `TileProvider`, as there are now two methods of which only one should be implemented (`getImage` & `getImageWithCancelLoadingSupport`), as well as other members that should not usually be overriden.

To migrate, use `extends` instead of `implements`.

_Further panes will refer to implementations that use `extends` as 'extensions' for clarity, not to be confused with extension methods._

</details>

<details>

<summary>Extensions should not provide a constant default value for <code>headers</code> in the constructor</summary>

`TileLayer` behaviour has been modified so that the 'User-Agent' header can be set without copying all user-specified `headers`. It is now inserted into the `Map`, so it must be immutable/non-constant.

Note that the `headers` property is also now `final`.

To migrate, remove the default value for `super.headers`: it is not necessary.

</details>

<details>

<summary>Extensions overriding <code>getTileUrl</code> should consider overriding other methods instead</summary>

The logic previously handled by `getTileUrl`, `invertY`, and `getSubdomain` has been refactored into `generateReplacementMap`, `populateTemplatePlaceholders`, and `getTileUrl`.

To migrate, consider overriding another of those methods, if it is more suitable. This will reduce the amount of code duplicated in your library from flutter\_map's implementation.

</details>

<details>

<summary>Extensions implementing <code>getImage</code> should consider overriding <code>getImageWithCancelLoadingSupport</code> instead </summary>

The framework necessary to support tile providers that can abort in-flight HTTP requests and other processing is now available. For more information about the advantages of cancelling unnecessary tile requests when they are pruned before being fully loaded, see [#cancellablenetworktileprovider](../layers/tile-layer/tile-providers.md#cancellablenetworktileprovider "mention").

If it is not possible to cancel the loading of a tile, or there is no advantage gained by doing so, you can ignore this.

To migrate, override `supportsCancelLoading` to `true`, implement `getImageWithCancelLoadingSupport` as appropriate, and remove the implementation of `getImage`.

</details>

<details>

<summary><code>TileLayer.templateFunction</code> has been replaced by <code>TileProvider.populateTemplatePlaceholders</code></summary>

`TileProvider.templateFunction` has been deprecated. It is now preferrable to create a custom `TileProvider` extension, and override the `populateTemplatePlaceholders` method. This has been done to reduce the scope of `TileLayer`.

To migrate, see [creating-new-tile-providers.md](../plugins/making-a-plugin/creating-new-tile-providers.md "mention").

</details>
