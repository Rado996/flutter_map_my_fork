# Interaction & Control

There's two ways to interact with the map - that is to control it, as well as receive data from it - and it's current viewport, aka. 'camera'.

## via User Gestures

The first way is through user interaction, where they perform gestures (such as drags/pans), and the map reacts automatically to those gestures to change the camera view of the map.

These are usually restricted by [options.md](../options.md "mention"). It is possible to disable all input, either by disabling all gestures, or by wrapping the map with something like `IgnorePointer`.

## via Programmatic Means

### Control Camera

To manipulate the map, for example to change its viewport, use a `MapController`.

{% content-ref url="controller.md" %}
[controller.md](controller.md)
{% endcontent-ref %}

### Get Current Camera

In order to get the current viewport, retrieve a `MapCamera` object.

{% content-ref url="get-current-camera.md" %}
[get-current-camera.md](get-current-camera.md)
{% endcontent-ref %}

### Listen To Events

It's also possible to listen to events, including changes to the current `MapCamera` object.

{% content-ref url="listen-to-events.md" %}
[listen-to-events.md](listen-to-events.md)
{% endcontent-ref %}
