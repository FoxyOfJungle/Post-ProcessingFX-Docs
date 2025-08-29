
## Applying layered effects <!-- {docsify-ignore} -->

This guide shows you how to easily add layered effects using Post-Processing FX.


## Tell PPFX that it should be drawn on a layer <!-- {docsify-ignore} -->

You just need to call `.LayerApply()` (see Feather docs).

Example: Apply a blur to a range of layers:


In `Create Event` or `Room Start`:
```gml
layerRenderer = new PPFX_Renderer();
layerRenderer.ProfileLoad(new PPFX_Profile("Cool Blur", [new FX_GaussianBlur(true, 0.1)]));
layerRenderer.LayerApply(layer_get_id("Background_Mid_1"), layer_get_id("Background_0")); 
```
</br>


Note that the function asks for a range of layers, the top layer is the top layer in the room layers, the bottom layer is the bottom layer. Effects will be applied within this range.

![Layer Range](/./images/Layers_0.png)

> You can also apply it in just one layer, just don't write anything to the seconds parameter (it is equal to the first parameter).

> Note that it works for any layer type: Instances, Backgrounds, Asset layers...

> Please note: You CANNOT use a range to which the layer has already been in range by another renderer. This will give an error. If you want to use more effects in the same layer, just add more effects to the profile.

> Note that you should use the .Apply() method only once, for each renderer. That is, don't run this every frame. It might be awkward to call .Apply() on Create Event, but that's right.


If you want to change this range later, use `.LayerSetRange()` method.


## Things you need to know <!-- {docsify-ignore} -->

When using this feature:

1 - Depth buffer IS supported.

2 - Cameras created using matrix functions ARE supported, but only orthographic projection.

3 - Blendmodes are not supported in the Draw event of objects, using `gpu_set_blendmode()`. I don't know why. 

4 - Using `surface_set_target()` is not supported in Draw event. If you really need this, use it in the Step event and just draw the surface in some Draw event.


## Result <!-- {docsify-ignore} -->

That's all, we have amazing effects just on the desired layers!

![Layer Range](/./images/Layers_Result.gif)
