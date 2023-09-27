
## Applying layered effects <!-- {docsify-ignore} -->

This guide shows you how to easily add layered effects using Post-Processing FX.



## Create a exclusive system for the layer <!-- {docsify-ignore} -->

You will create a system exactly the way you create it for full screen effects, the difference is that `you will not use Draw() or DrawInFullscreen()`.

In `Create Event` or `Room Start`:
```gml
// Create a exclusive post-processing system for the layer
layer_effects_id = new PPFX_System();

// Create a profile with effects, for the system
var _layer_profile = new PPFX_Profile("Cool Effect", [
	new FX_SineWave(true, 0.5, [0.005, 0.005], [30, 30]),
	new FX_SunShafts(true, [0.5, 0.5], 0.3, 0.5, 3, 1, 0.3, true),
]);

// Load profile
layer_effects_id.ProfileLoad(_layer_profile);
```
</br>


## Tell PPFX that it should be drawn on a layer <!-- {docsify-ignore} -->

For that, you must create a layer renderer with `PPFX_LayerRenderer()`, then just use `.Apply()` method.

In `Create Event` or `Room Start`:
```gml
layer_renderer = new PPFX_LayerRenderer();
layer_renderer.Apply(layer_effects_id, layer_get_id("Background_Mid_1"), layer_get_id("Background_0")); 
```
`layer_effects_id` is our post-processing system created before, with `new PPFX_System()`.  
</br>


Note that the function asks for a range of layers, the top layer is the top layer in the room layers, the bottom layer is the bottom layer. Effects will be applied within this range.

![Layer Range](/./images/Layers_0.png)

> You can also apply it in just one layer, just use the same layer id in the Top Layer and Bottom Layer.

> Note that it works for any layer type: Instances, Backgrounds, Asset layers...

> Please note: You CANNOT use a range to which the layer has already been in range by another renderer. This will give an error. If you want to use more effects in the same layer, just add more effects to the profile.

> Note that you should use the .Apply() method only once, for each system. That is, don't run this every frame. It might be awkward to call .Apply() on Create Event, but that's right.


If you want to change this range later, use `.SetRange()` method, in the Layer Renderer.

## Avoid memory leak <!-- {docsify-ignore} -->

Remember to delete the layer and post-processing systems when leaving the room, to avoid memory leak. To do so, use these functions in the Clean Uo event:

```gml
layer_effects_id.Destroy(); // Free post-processing system from memory
layer_renderer.Destroy(); // Destroy layer renderer
```

## Limitations <!-- {docsify-ignore} -->

When using the PPFX_LayerRenderer() to draw effects, there are some limitations:

1 - Blendmodes are not supported in the Draw event of objects, using `gpu_set_blendmode()`.  

2 - Using `surface_set_target()` is not supported in Draw event. If you really need this, use it in the Step event and just draw the surface in some Draw event.

## Result <!-- {docsify-ignore} -->

That's all, we have amazing effects just on the desired layers!

![Layer Range](/./images/Layers_Result.gif)
