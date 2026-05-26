
## Creating shockwaves <!-- {docsify-ignore} -->

This guide will show you how to create shockwave effects using Post-Processing FX.

</br>


## Add Shockwaves effect <!-- {docsify-ignore} -->

If you already have a fullscreen `PPFX_Renderer`, simply add the `FX_Shockwaves` effect to it. It will be responsible for distorting the screen using a texture.  

Below we will create the PPFX renderer with the Shockwaves effect loaded from a profile.  

**Create Event:**
```gml
// Create Post-processing renderer id to be used by the other functions
renderer = new PPFX_Renderer();

// Create profile with some effects
profile = new PPFX_Profile("Shockwaves", [
	new FX_Shockwaves(true, 0.1, 0.2, sprite_get_texture(__ppf_sprPrismLutGP, 0)),
]);

// load profile
renderer.ProfileLoad(profile);
```
Note that we are using the built-in spectral LUT sprite, but you can use your own sprite; just create a 3x1 sprite where each pixel represents the RGB channels from left to right.

![Image](/./images/ShockwavesSpectralLUT.png)

</br>



## Create NormalMap Renderer system <!-- {docsify-ignore} -->

Normal maps are sprites that contain colors (vectors) responsible for distorting the screen. Red distorts horizontally, green distorts vertically. The neutral color is purple RGB: 128, 128, 255. 

Post-Processing FX already comes with some built-in ones that you can use.  

![Image](/./images/ShockwavesNormalMaps.png)

For now, the effect doesn't do anything on its own, as it hasn't yet received the distorted texture to distort the screen.  

That's why we need a `PPFX_NormalmapRenderer`. Its purpose is to draw shockwaves (sprites) inside a surface and send that surface to the effect.  

Still on **Create Event:**
```gml
// Init shockwaves distortion surface
shockwavesRenderer = new PPFX_NormalmapRenderer(renderer);
shockwavesRenderer.SetHighPrecisionEnable(true);
```
You reference the renderer to which you want it to send the surface with normal maps.

By default, Post-Processing FX will use a built-in object containing a sprite. You can create new child objects from it and draw them however you like.

![Image](/./images/ShockwaveObject.png)

You can, of course, define your object directly via code:

```gml
shockwavesRenderer.SetObject(objNormalMap);
```

</br>


## Renderize normal maps to the internal surface <!-- {docsify-ignore} -->

Now, in order for the normal maps to actually be rendered within the surface, you need to call the `.Render(camera)` function in the **Draw End** event. It's important that it happens at this event to avoid a 1-frame lag.

**Draw End Event:**
```gml
// renderize shockwaves surface
shockwavesRenderer.Render(view_get_camera(view_current));
```

</br>

## Creating shockwaves <!-- {docsify-ignore} -->

Now for the fun part: creating the shockwaves.  

You can simply create the objects in the room using `instance_create_depth/layer()` or the built-in `shockwave_instance_create()` helper function.

```gml
// Spawn shockwaves at the mouse position
if (mouse_check_button_pressed(mb_left)) {
	shockwave_instance_create(mouse_x, mouse_y, "Instances", 0, 2, 0.01, __ppf_objShockwave);
}
```

</br>


## Result <!-- {docsify-ignore} -->

That's all, we have cool shockwaves! :D

![Image](/./images/ShockwavesResult.gif)
