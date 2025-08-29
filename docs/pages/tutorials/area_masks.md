
## Using Area Masks <!-- {docsify-ignore} -->

Area masks are areas of the screen on which you can draw only part of the post-processing. You can use sprites to crop the image, creating cool effects.

![Layer Range](./images/AreaMasks_0.png)

> This feature is intended for UI use only.


## Creating glass effect for UI <!-- {docsify-ignore} -->

In this example, we will simulate glassmorphism. Basically you will create a renderer in the same way as the other examples. Placing the desired effects and then loading the profile.  
We're going to use Kawase Blur, which is the lightest blur effect.

In `Create Event`:
```gml
uiBlurRenderer = new PPFX_Renderer();
uiBlurRenderer.SetDrawEnable(false); // disable auto-draw of this renderer - because we will draw it manually, using the area functions!
var profileBlur = new PPFX_Profile("Glass Blur", [
	new FX_KawaseBlur(true, 0.4),
]);
uiBlurRenderer.ProfileLoad(profileBlur);
```

</br>
Generally, you will already be using a post-processing renderer before, so you should use the main surface as a base to render the renderer used for blur:

In `Draw GUI Begin` event:
```gml
var _xx = 0;
var _yy = 0;
var _ww = display_get_gui_width();
var _wh = display_get_gui_height();

// render main renderer from "Main" (this is your fullscreen renderer)
renderer.Draw(application_surface, _xx, _yy, _ww, _wh);

// render "Glass Blur" using the main post-processing surface (the previous one)
uiBlurRenderer.Draw(renderer.GetRenderSurface(), _xx, _yy, _ww, _wh);
```
In the example above, we create a Blur effect, using the full-screen rendering "image" (surface) as a base.

</br>

## Drawing Simple Masks <!-- {docsify-ignore} -->

Still nothing will be displayed on the screen as we need to create clipping masks. For this we will use some functions. It is possible to use sprites or rectangles as a mask.

To draw a simple rectangular mask, use:

In `Draw GUI` event:
```gml
area_draw_rect(60, 60, 200, 200, 0, 0, uiBlurRenderer);
```
In the code above we used the blur system (Glass Blur).

So:

![Layer Range](./images/GlassBlur_0.png)

</br>

## Drawing Sprite Masks <!-- {docsify-ignore} -->

We've seen how to draw a basic rectangular mask. Now we'll see how to use your *own* sprite as a mask, giving you infinite customization possibilities!
The code is the same for the systems. Just now we will use the `area_draw_sprite_*` functions:

```gml
area_draw_sprite_ext_mask(sprMask, 0, 100, 100, 1, 1, -current_time*0.1, c_aqua, 1, uiBlurRenderer);
```
You will draw the sprite like any other, you will just use the id of the post-processing system.

So:

![Layer Range](./images/GlassBlur_1.gif)

Mask used:

![Layer Range](./images/ppfx_mask_icon.png)


</br>

### Rounded Rectangle Sprite Mask <!-- {docsify-ignore} -->

Usage:

```gml
area_draw_sprite_stretched_mask(sprMask, 0, device_mouse_x_to_gui(0), device_mouse_y_to_gui(0), 300, 200, uiBlurRenderer);
```

So:

![Mask](./images/GlassBlur_2.png)

Mask used:

![Mask](./images/RoundedSquareMask.png)

> Tip: You can enable the Nine Slices option on the sprite for a perfect glass!

![Mask](./images/GlassBlur_3.png)


This is it! :D


