# Leaflet.Control.Layers.ComboBaseLayer
A layer control for Leaflet that breaks base layer selection across several radio groups.

## Usage

Download the JavaScript file and add it to your map's HTML header:

```html
<script 
  type="text/javascript"
  src="leaflet.control.layers.combobaselayer.js"></script>
```

Then instantiate it as you would a regular layer control. Like the default Layer control, ComboBaseLayer uses your layer names to construct the menus—but it expects layers to have structured names made up of delimited fragments. For example:

```js
var my_baselayers = {
  'cereal_satisf|day|2017': L.tileLayer(...),
  'cereal_satisf|night|2017': L.tileLayer(...),
  'cereal_satisf|day|2018': L.tileLayer(...),
  'cereal_satisf|night|2018': L.tileLayer(...),
  'toast_satisf|day|2017': L.tileLayer(...),
  'toast_satisf|night|2017': L.tileLayer(...),
  'toast_satisf|day|2018': L.tileLayer(...)
}
```

ComboBaseLayer can't detect the number of fragments (and hence menus) or the delimiter used. You need to tell it using the `menu_count` (default 3) and `menu_delimiter` (default `'|'`) options:

```js
var my_combo_control = L.control.layers.comboBaseLayer(
  my_base_layers,
  my_overlays,
  {
    position: 'topleft',
    menu_count: 3,
    menu_delimiter: '_'
  }).addTo(map);
```

The plugin will automatically populate menus according to the unique options given in the named layers, and it will manage layer attachment according to the selected options.

ComboBaseLayer can additionally add a 'prettier' description to the menu item using the `matches` option:

```js
var my_combo_control = L.control.layers.comboBaseLayer(
  my_base_layers,
  my_overlays,
  {
    matches: {
      cereal_satisf: 'Reported satisfaction with cereal',
      toast_satisf: 'Reported satisfaction with toast',
      day: 'during the day',
      night: 'during the night'
    }
  }).addTo(map);
```

If a menu item doesn't match anything provided, the description will be the same as the layer name (which might look a little odd). You can use `display: none` with various CSS selectors to control which parts of the menu items appear (for example, if you want to use layer named internally but only show the longer descriptions to users): 

| CSS Selector                                         | Menu item element   |
|------------------------------------------------------|---------------------|
| form.leaflet-control-layers-list input[type="radio"] | Radio button        |
| form.leaflet-control-layers-list span                | Name                |
| form.leaflet-control-layers-list p                   | Description         |

## Limitations

At present, base layers are not disabled according to whether the presently selected options are compatible, so it is possible to select a combination that does not lead to a valid layer (this fails silently). In the above example, a user could select `toast_satisf`, `night` and `2018`, even though no such layer has been defined. This will presently fail silently: any previously selected layer will be removed, but no new layer will be added.

The logic to determine whether certain menu selections are incompatible with others can be added via:

```js
L.Control.Layers.include({
  _checkDisabledLayers: function () {

    /* this function should iterate through this._layerControlInputs, setting the
     .disabled property for each */

  }
}
```
    
(I'd like to add some sensible defaults, once I can decide on some!)

Please feel free to [submit an issue](https://github.com/rensa/Leaflet.Control.Layers.ComboBaseLayer) if something's not working right, or [get in touch](https://twitter.com/rensa_co)! Suggestions, PRs and complaints are all welcome 😅
