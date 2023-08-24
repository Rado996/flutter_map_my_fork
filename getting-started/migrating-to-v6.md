# Migrating To v6

{% hint style="info" %}
This update has renewed one of the oldest surviving sections of 'flutter\_map', fixed bugs, and added features!

This is significant progress in our aim to renew the project and bring it up to date. In the long run, this will bring it inline with up-to-date Flutter good practises and techniques, improve its performance and stability, and reduce the maintenance burden.
{% endhint %}

There are major breaking changes for all users. Please refer to the sections below for information on how to migrate your project, as well as in-code documentation and deprecation messages if your migration is not listed below.

{% embed url="https://github.com/fleaflet/flutter_map/blob/master/CHANGELOG.md" %}
Full Changelog
{% endembed %}

<details>

<summary><code>MapController</code> should not be used directly to read information about the map's position</summary>

`MapController` now only controls the map's position. The map's position is now described by `MapCamera`.

There are multiple possibilities for migration:

1. If inside the `FlutterMap` context, prefer using `MapCamera.of(context)`
2. Otherwise, use `MapController` in the same way, but use the `.camera` getter to retrieve the `MapCamera`.

See [interaction-and-control](../usage/interaction-and-control/ "mention") for more information.

</details>

<details>

<summary><code>CustomPoint</code> no longer exists</summary>

[Extension methods](https://dart.dev/language/extension-methods) are now used to add the required functionality to the standard 'dart:math' `Point` object.

To migrate, most cases should just need to replace all occurrences of `CustomPoint` with `Point`.

</details>

<details>

<summary><code>TileLayer.backgroundColor</code> has been replaced by <code>MapOptions.backgroundColor</code></summary>

This will simplify the developer experience when using multiple overlaid `TileLayer`s, as `Colors.transparent` will no longer need to be specified. There is no reason that multiple `TileLayer`s would each need to have a different (non-transparent) background colors, as the layers beneath would be invisible and therefore pointless.

Therefore, `TileLayer`s now have transparent backgrounds, and the new `MapOptions.backgroundColor` property sets the background color of the entire map.

</details>

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

<details>

<summary>Removed <code>nonRotatedChildren</code></summary>

The approach to 'mobile' and 'static' layers has been changed. Mobile layers now wrap themselves in a `MobileLayerTransformer` which uses the inherited state, instead of `FlutterMap` applying the affects directly to them. Static layers should now ensure they use `Align` and/or `SizedBox.expand`.

This has been done to simplify setup, and allow for placing static layers between mobile layers.

</details>
