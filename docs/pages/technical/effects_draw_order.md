
## Effects Draw Order <!-- {docsify-ignore} -->

Effects have an internal rendering order, for better final image processing results. You can modify the default stack rendering order in the script: **"__ppf_effects" > Enums**.

### Default drawing order

Following is the default order in which effects are drawn/applied to the image:
Top = First | Bottom = Last

```gml
* BASE (Rotation, Zoom, Shake, Lens Distortion, Pixelization, Swirl, Panorama, Interference, Shockwaves, DisplaceMaps),
BLOOM,
SUNSHAFTS,
LONG_EXPOSURE,
* COLOR_GRADING (White Balance, Exposure, Tone Mapping, Contrast, Brightness, Color Curves, LUT etc),
PALETTE_SWAP,
DEPTH_OF_FIELD,
MOTION_BLUR,
BLUR_RADIAL,
TEXTURE_OVERLAY,
BLUR_KAWASE,
BLUR_GAUSSIAN,
GLITCH,
VHS,
ASCII,
CHROMATIC_ABERRATION,
HQ4X,
* FINAL (Mist, Speedlines, Dithering, Scanlines, Noise Grain, Fade, CinemaBars, Vignette, Border, etc),
SHARPEN,
FXAA,
COMPARE,
```

Something important to know: Effects are classified into two types:

<ul class="a">
    <li>Individual: These effects are independent and can have their order changed (ex: Bloom, Depth Of Field, Blurs);</li>
    <li>Shared: These effects share the same stack and cannot have their >individual< order changed (ex: LUT, Hue Shift, Color Tint, Scanlines) .</li>
</ul>


Effects with `*` use a shared stack, which means that a single surface is used for all the effects. These effects have the same order and cannot be changed individually. This is done for performance. The order applied is already what gives best results.  

Individual effects can have their order changed freely, with `.SetOrder(intNumber)`.
Example:
```gml
new FX_DepthOfField(true).SetOrder(10),
```

Note that each enum value is an integer, so if you want to reorder the effects, you need to use this as a base. You can even reference the enum too:
```gml
new FX_DepthOfField(true).SetOrder(PPFX_STACK.BASE-1),
```
Therefore, the effect will be the first to be drawn before everyone else.

> NOTE: It is extremely important to know that if the independent effects have the same orders, the post-processing system/renderer will NOT know how to define which will be rendered first, so the order will be random.

> External effects (add-ons) are classified as individual effects.
