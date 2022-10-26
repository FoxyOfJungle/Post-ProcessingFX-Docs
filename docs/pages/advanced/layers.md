
## Applying layered effects <!-- {docsify-ignore} -->

This guide shows you how to easily add layered effects using Post-Processing FX.

## Create a exclusive system for the layer <!-- {docsify-ignore} -->

You will create a system exactly the way you create it for full screen effects, the difference is that `you will not use ppfx_draw()`. Just use the `ppfx_layer_apply()` function.

In `Create Event` or `Room Start`:
```gml
// Create system
layer_effects_id = ppfx_create();

// Create profile with effects
var _profile_test = ppfx_profile_create("Cool Layer Effects", [
    new pp_sine_wave(true, 0.5, [0.005, 0.005], [30, 30]),
	new pp_sunshafts(true, [0.5, 0.5], 0.3, 0.5, 3, 1, 0.3, true, 1, 0.08, 0.8, false),
]);

// Load profile
ppfx_profile_load(layer_effects_id, _profile_test);
```


## Tell PPFX that it should be drawn on a layer <!-- {docsify-ignore} -->

For that, you must create a layer system with `ppfx_layer_create()`, then just use `ppfx_layer_apply()`.

In `Create Event` or `Room Start`:
```gml
layer_index = ppfx_layer_create();
ppfx_layer_apply(layer_effects_id, layer_index, layer_get_id("Background_Mid_1"), layer_get_id("Background_0"), false);
```

Note that the function asks for a range of layers, the top layer is the top layer in the room layers, the bottom layer is the bottom layer. Effects will be applied within this range.

![Layer Range](/./images/Layers_0.png)

> Note that it works for any layer type: Instances, Backgrounds, Asset layers...

> You can also apply it in just one layer, just use the same layer id.

> Please note: You CANNOT select a range to which the layer has already been in range by another system. This will give an error. If you want to use more effects in the same layer, just add more effects to the profile.

> Note that you should use the ppfx_layer_apply() function only once, for each system. That is, don't run this every frame.

If you want to change this range later, use ppfx_layer_set_range() function.

## Avoid memory leak <!-- {docsify-ignore} -->

Remember to delete the layer and post-processing systems when leaving the room, to avoid memory leak. To do so, use these functions in the Clean Uo event:

```gml
ppfx_destroy(layer_effects_id); // Free post-processing system from memory
ppfx_layer_destroy(layer_index); // Destroy created post-processing layer
```

## Result <!-- {docsify-ignore} -->

That's all, we have amazing effects just on the desired layers!

![Layer Range](/./images/Layers_Result.gif)
