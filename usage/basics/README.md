# Base Widget

<table data-card-size="large" data-view="cards" data-full-width="true"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><pre class="language-dart"><code class="lang-dart">FlutterMap(
    mapController: MapController(),
    options: MapOptions(),
    children: [],
);
</code></pre></td><td><p><img src="../../.gitbook/assets/Okay Icon" alt="" data-size="line"> <mark style="color:green;"><strong>Prefer</strong></mark></p><p>...using the normal constructor</p></td><td><hr><p>This constructor provides access to the full functionality set of flutter_map.<br><br>Start by adding some <a data-mention href="broken-reference">Broken link</a> to <code>children</code>, then configure the map in <a data-mention href="../options/">options</a>.</p></td></tr><tr><td><pre class="language-dart"><code class="lang-dart">FlutterMap.simple(
    options: MapOptions(),
    urlTemplate: '',
    userAgentPackageName: '',
    attribution: RichAttributionWidget(),
);
</code></pre></td><td><p><img src="../../.gitbook/assets/Exclamation Icon" alt="" data-size="line"><mark style="color:orange;"><strong>Avoid</strong></mark></p><p>... using the <code>simple</code> constructor, unless the map doesn't require much functionality</p></td><td><hr><p>This constructor provides limited access to a subset of the available functionality, designed to get you started in record time.<br>See <a data-mention href="simple-constructor.md">simple-constructor.md</a> for more information.</p></td></tr></tbody></table>

## Placement Recommendations

It is recommended to make the map as large as possible, to allow it to display a lot of useful information easily.

As such, we recommend using a depth-based layout (eg. using `Stack`s) instead of a flat-based layout (eg. using `Column`s). The following 3rd party packages might help with creating a modern design:

* [https://pub.dev/packages/backdrop](https://pub.dev/packages/backdrop)
* [https://pub.dev/packages/sliding\_up\_panel](https://pub.dev/packages/sliding\_up\_panel)
* [https://pub.dev/packages/material\_floating\_search\_bar\_2](https://pub.dev/packages/material\_floating\_search\_bar\_2)

If you must restrict the widget's size, you won't find a `height` or `width` property. Instead, use a `SizedBox` or `Column`/`Row` & `Expanded`.

{% hint style="info" %}
The map widget will expand as much as possible.

To avoid errors about infinite/unspecified dimensions, ensure the map is contained within a constrained widget.
{% endhint %}

### Keep Alive

If the map is displayed lazily in something like a `PageView`, changing the page and unloading the map will cause it to reset to its [initial positioning](../options/#initial-positioning).

To prevent this, set `MapOptions.keepAlive` `true`, which will activate an internal `AutomaticKeepAliveClientMixin`. This will retain the internal state container in memory, even when it would otherwise be disposed.
