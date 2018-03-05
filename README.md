# Leaflet.Control.Layers.ComboBaseLayer
A layer control for Leaflet that breaks base layer selection across several radio groups.

## Usage

Download the JavaScript file and add it to your map's HTML header:

```html
<script 
  type="text/javascript"
  src="leaflet.control.layers.combobaselayer.js"></script>
```

Then instantiate it as you would a regular layer control:

```js
var my_combo_control = L.control.layers.comboBaseLayer(
  my_base_layers, my_overlays, { position: 'topleft' }).addTo(map);
```

You should name your base layers delimited by underscores, like `Option 1_Option 2_Option 3`. At present, the plugin only supports 3 options—no more or less—and they must be delimited by underscores (which can't be escaped). For example:

```js
var my_baselayers = {
  'Reported cereal satisfaction_Daytime_2017': L.tileLayer(...),
  'Reported cereal satisfaction_Nighttime_2017': L.tileLayer(...),
  'Reported cereal satisfaction_Daytime_2018': L.tileLayer(...),
  'Reported cereal satisfaction_Nighttime_2018': L.tileLayer(...),
  'Some other thing_Nighttime_2018': L.tileLayer(...)
}
```

At present, base layers are not disabled according to whether the presently selected options are compatible, so it is possible to select a combination that does not lead to a valid layer (this fails silently). This logic can be added via:

```js
L.Control.Layers.include({

  _checkDisabledLayers: function () {
  }

}
```js
    
(But I'd like to add some sensible defaults, once I can decide on some!)

Although I can't necessarily support your use of this plugin, feel free to [submit an issue](https://github.com/rensa/Leaflet.Control.Layers.ComboBaseLayer) or [get in touch](https://twitter.com/rensa_co).
