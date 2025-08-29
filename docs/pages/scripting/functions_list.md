
## Post-Processing FX Functions List <!-- {docsify-ignore} -->

The following are all the functions that Post-Processing FX has to generate the effects and control them.  
Although the topics are separate, they are all linked in some way. Parameters inside brackets are optional.

Note: All content here is also available using Feather, from GameMaker, offline.

<br>



# PPFX_Renderer()

`PPFX_Renderer()` is essentially responsible for drawing the visual effects. It works both for full screen and for layers. You can create as many renderers as you need, depending on what you're going to do.
Examples:

<ul class="a">
<li>For a full-screen game, one renderer is enough;</li>
<li>To use in layers, you will need another renderer (do not use the same renderer as fullscreen), for each layer range;</li>
<li>For a split-screen game, you'll need 4 (or 2, depending on what you want) renderers (unless you use the same color grading renderer for both 4 screens);</li>
<li>To use with Areas, you will need another renderer;</li>
</ul>

> By instantiating the renderer with "new" operator, you will get a struct, in which you will be able to control the renderer with the methods below.

> Renderers need to be destroyed using `.Destroy()` when no longer used, to avoid memory leaks.


## .ProfileLoad()

```gml
.ProfileLoad(profile, merge, filter);
```
This function loads a previously created profile. Effects REFERENCE is copied, which means that if you modify the effect struct, this will be reflected in the renderer as well.  
After loading the profile, the renderer will reorder the effects based on their stackOrder and draw them as defined.  
Your Profile must NOT have repeated effects, otherwise they will be replaced. If you want to repeat effects, use another PPFX_Renderer where it receives input from another.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| profile | Struct | Profile struct created with new PPFX_Profile(). |  
| merge | Bool | If you want to merge the profile with existing effects in the renderer (without replacing everything). |  
| filter | Undefined, Macro(String), Array<Macro(String)> | If defined, will add only the selected effect. Use the "FF_" effect macro directly, or an array of macros. Example: [FF_BLOOM, FF_DOF] will only add those two effects among several in the profile. The effect should exist in the profile to be filtered (otherwise nothing happens). |  

#### Returns: [Undefined] - N/A

### Example:
```gml
profile = new PPFX_Profile("Main", effects);

renderer.ProfileLoad(profile);
```
The code above creates a profile and loads it into the renderer id system.

<br><br><br>




## .ProfileUnload()

```gml
.ProfileUnload();
```
This function removes any profile associated with this post-processing system, consequently disabling all effects.

#### Returns: [Undefined] - N/A

### Example:
```gml
renderer.ProfileUnload();
```
The code above causes the renderer system to remove its profile.

<br><br><br>





## .Draw()

```gml
.Draw(surface, x, y, w, h);
```
Draw Post-processing's final surface on screen. This function works like draw_surface_stretched(), but with the effects applied. The resolution of the internal surfaces will be the same as the input surface.  

This is similar to `draw_surface_stretched()`.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| surface | Surface | The input surface to copy from. You can use `application_surface` or your own. |  
| x | Real | The x position og where to draw the surface. |  
| y | Real | The y position og where to draw the surface. |  
| w | Real | The width at which to draw the surface. |  
| h | Real | The height at which to draw the surface. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
var _pos = application_get_position();

renderer.Draw(application_surface, _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1]);
```
The above example draws the "renderer" post-processing system on the screen. Inside Post-Draw event and at position 0, 0, using the window size and using the camera resolution.

<br><br><br>





## .DrawInFullscreen()

```gml
.DrawInFullscreen(surface);
```
Similar to `.Draw`, but it handles the sizes and positions for you, for ease.  
This function automatically detects the draw event you are drawing (Post-Draw or Draw GUI Begin).  
It uses the size of the referenced surface for internal rendering resolution (example: application_surface size).  
For the width and height size (scaled rendering size): If drawing in Post-Draw, is the size of the window (frame buffer). If in Draw GUI Begin, the size of the GUI.  

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| surface | Surface | Render surface to copy from. (You can use application_surface). |  


#### Returns: [Undefined] - N/A

### Example:
```gml
renderer.DrawInFullscreen(application_surface);
```
The above example draws the "renderer" post-processing system on the screen.

<br><br><br>





## .Destroy()

```gml
.Destroy();
```
Destroy a previously created post-processing system, freeing from memory all the resources it created at runtime. Only use this if you will no longer be using a system.


#### Returns: [Undefined] - N/A

### Example:
```gml
renderer.Destroy();
```
The example above destroys the "renderer" post-processing system.

<br><br><br>




## .SetEffectParameter()

```gml
.SetEffectParameter(effect, param, value);
```
Modify a single parameter (setting) of an effect. Use this to modify an effect's attribute in real-time.  

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect | Macro(String) | Effect name. Use the macro starting with FF_, example: FF_BLOOM. Or, the effectName: "bloom". |  
| param | Macro(String) | Parameter name macro. Example: PP_BLOOM_COLOR. |  
| value | Any | Parameter value. Example: `make_color_rgb_ppfx(255, 255, 255)`. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetEffectParameter(FF_LENS_DISTORTION, PP_LENS_DISTORTION_AMOUNT, 0.5);
```
In the example above, we set the amount of distortion for the Lens Distortion effect to 0.5.

<br><br><br>





## .SetEffectParameters()

```gml
.SetEffectParameters(effect, param, value);
```
Modify many parameters (setting) of an effect. Use this to modify an effect's attribute in real-time.

> Note that this function uses a internal loop to access the array, which makes it slower.

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect | Macro(String) | Effect name. Use the macro starting with FF_, example: FF_BLOOM. Or, the effectName: "bloom". |  
| paramsArray | Array<Macro(String)> | Array with the effect parameters. Use the pre-defined macros, for example: [PP_BLOOM_COLOR, PP_BLOOM_INTENSITY]. |  
| valuesArray | Array<Macro(String)>  | Array with parameter values, must be in the same order. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetEffectParameters(FF_BLOOM, [PP_BLOOM_INTENSITY, PP_BLOOM_THRESHOLD], [3, 0.7]);
```
In the example above, we specify the effect (which is an enum), the array of macros of the parameters we want to modify, and finally the desired array of value. They must need to be in the same order they were written.

<br><br><br>





## .SetEffectEnable()

```gml
.SetEffectEnable(effect, enable);
```
Toggle whether the post-processing effect can render.
Please note that if enabled, it can render to the surface even if not drawing! Disabling this releases the effect usage on the GPU.  

> Note that if the effect was active before and you turned it off later (using this function), its surface will still exist, so you need to use .Clean() in the post-processing effect to clean up any unused surfaces. This is ONLY valid for effects that have individual stacks, such as: Bloom, Depth Of Field, FXAA, HQ4X, VHS, Palette Swap, Blurs, Chromatic Aberration, Sunshafts and Slow Motion.

![Draw Enable](/images/PPFXRenderEnable.gif)
*In the image above, the effect, when deactivated, uses the original surface without the effects. So the surface of the disabled effect stops updating.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect | Real/Enum | Use the enumerator, example: FF_BLOOM. |  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetEffectEnable(FF_BLOOM, false);
```
In the example above, we have deactivated the Bloom effect from the renderer effect.

<br><br><br>





## .SetDrawEnable()

```gml
.SetDrawEnable(enable);
```
Toggle whether the post-processing renderer can draw.  
Please note that if disabled, it may still be rendered if rendering is enabled (will continue to demand GPU).

![Draw Enable](/images/PPFXDrawEnable.gif)
*In the image above, another renderer still can to get its surface, even without drawing.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetDrawEnable(false);
```
In the example above, we disable renderer drawing.

<br><br><br>




## .SetHDREnable()

```gml
.SetHDREnable(enable);
```
Enable or disable HDR from this renderer. Default is false. Generally recommended to have (if needed), for best visuals.  
This allows for better color depth, contrast and brightness (especially useful for the Bloom and Sunshafts/Godrays/LongExposure effect).  
This can affect VRAM usage and not every hardware supports this (although, most of the 2015 GPUs above should work).  

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Defines if HDR is enabled. Use -1 to toggle. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetHDREnable(true);
```
In the example above, we disable renderer drawing.

<br><br><br>




## .SetRenderEnable()

```gml
.SetRenderEnable(enable);
```
Toggle whether the post-processing renderer can render.
Please note that if enabled, it can render to the surface even if not drawing! Disabling this releases the system usage on the GPU.  

![Draw Enable](/images/PPFXRenderEnable.gif)
*In the image above, the renderer, when deactivated, uses the original surface without the effects. So the surface of the disabled renderer stops updating.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetRenderEnable(false);
```
In the example above, we disable renderer rendering.

<br><br><br>




## .SetRenderResolution()

```gml
.SetRenderResolution(resolution);
```
This function modifies the final rendering resolution of the post-processing renderer, based on a percentage (0 to 1 - full).

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| resolution | Real | Value from 0 to 1 that is multiplied internally with the final resolution of the renderer's final rendering view. |  

#### Returns: [Self]

### Example:
```gml
renderer.SetRenderResolution(0.5);
```
In the example above, we set the render resolution to half size.

<br><br><br>




## .LayerApply()

```gml
.LayerApply(topLayer, bottomLayer);
```

This function applies post-processing to one or more layers. You only need to call this ONCE in an object's "Create" or "Room Start" Event. Do NOT draw the Post-processing renderer manually, if you use this. This function already draws the Post-Processing on the layer, using layer scripts.  

Make sure the Top Layer is above the Bottom Layer in order. If not, it may not render correctly!  
Please note: You CANNOT select a range to which the layer has already been in range by another renderer. This will give an unbalanced surface stack error. If you want to use more effects, just add more effects to the profile.  
When applying PPFX_Renderer to layers, the layers defined in range are drawn within the same surface, which means that if you have another layer between them, the depth in consideration will be the depth of the first layer.  

Let's say you have these layers:
- Grass1;
- Grass2;
- Brick;
- Grass4;
- Grass5;

This will work well:
- Apply the renderer from "Grass1" to "Grass2".

This will cause depth issues:
- Apply the renderer from "Grass1" to "Grass5", causing the Brick layer to be above the others.
The solution is to create a new PPFX_Renderer() for each range of layers.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| topLayer | Id.Layer | The top layer, in the room editor. |  
| bottomLayer | Id.Layer | The bottom layer, in the room editor. By default, this is equal to topLayer, useful if you want to render everything in just one layer. |  


#### Returns: [Self]

### Example:
```gml
layerRenderer = new PPFX_Renderer();
layerRenderer.ProfileLoad(new PPFX_Profile("Blur", [new FX_GaussianBlur(true, 0.1)]));
layerRenderer.LayerApply(layer_get_id("Background_0"));
```
Blur effect applied to a single layer.

<br><br><br>





## .LayerSetRange()

```gml
.LayerSetRange(topLayer, bottomLayer);
```

This function defines a new range of layers, if you are applying post-processing to a layer (using .LayerApply()).  
Make sure the top layer is above the bottom layer in order. If not, it may not render correctly!  
Please note: You CANNOT select a range to which the layer has already been in range by another renderer. This will give an unbalanced surface stack error. If you want to use more effects, just add more effects to the profile.  

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| topLayer | Id.Layer | The top layer, in the room editor. |  
| bottomLayer | Id.Layer | The bottom layer, in the room editor. By default, this is equal to topLayer, useful if you want to render everything in just one layer. |  


#### Returns: [Self]

### Example:
```gml
layerRenderer.LayerSetRange(layer_get_id("BackgroundTop"), layer_get_id("BackgroundBottom"));
```
Blur effect applied to a range of layers.

<br><br><br>





## .SetDeltaTime()

```gml
.SetDeltaTime(deltaTime);
```

Sets the delta time variable to be used in the renderer (optional).

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| deltaTime | Real | The delta time. 1 is the default delta time. |  


#### Returns: [Self]

### Example:
```gml
renderer.SetDeltaTime(deltaTime);
```

<br><br><br>



## .SetTimer()

```gml
.SetTimer(value);
```

Sets the time that the renderer will reset its timer. Useful for mobile devices, for low precision reasons.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| value | Real | The timer, in seconds. Use -1 for unlimited. |  


#### Returns: [Self]

### Example:
```gml
renderer.SetTimer(3600);
```
The renderer timer will reset in 1 hour.

<br><br><br>




## .DisableAllEffects()

```gml
.DisableAllEffects();
```
This function disables all loaded effects immediately.

#### Returns: [Bool]

### Example:
```gml
renderer.DisableAllEffects();
```

<br><br><br>




## .IsDrawEnabled()

```gml
.IsDrawEnabled();
```
This function returns whether the renderer is drawing or not.

#### Returns: [Bool] - true: is drawing. false: not drawing.

### Example:
```gml
var _drawing = renderer.IsDrawEnabled();
show_debug_message(_drawing);
```
In the example above, we return whether the renderer is drawing to a variable, and then show a message on the console.

<br><br><br>




## .IsRenderEnabled()

```gml
.IsRenderEnabled();
```
This function returns whether the renderer is rendering or not.

#### Returns: [Bool] - true: is rendering. false: not rendering.

### Example:
```gml
var _rendering = renderer.IsRenderEnabled();
show_debug_message(_rendering);
```
In the example above, we return whether the renderer is rendering to a variable, and then show a message on the console.

<br><br><br>



## .IsLayered()

```gml
.IsLayered();
```
Returns true if the renderer is rendering on layers.

#### Returns: [Bool]

### Example:
```gml
var _isLayered = renderer.IsLayered();
show_debug_message(_isLayered);
```

<br><br><br>




## .IsEffectEnabled()

```gml
.IsEffectEnabled(effect);
```
This function returns whether an effect is enabled or not.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect | Macro(String) | Effect name. Use the macro starting with FF_, example: FF_BLOOM. Or, the effectName: "bloom". |  

#### Returns: [Bool] - true: is enabled. false: not enabled.

### Example:
```gml
var _enabled = renderer.IsEffectEnabled(FF_BLOOM);
show_debug_message(_enabled);
```
In the example above, we return whether the effect is enabled to a variable, and then show a message on the console.

<br><br><br>



## .GetRenderSurface()

```gml
.GetRenderSurface();
```
Returns the post-processing renderer final rendering surface.

#### Returns: [Surface] - Surface id.

### Example:
```gml
blurRenderer.DrawInFullscreen(renderer.GetRenderSurface());
```
In the example above, a second renderer is drawn using the final surface of the previous renderer.

<br><br><br>




## .GetStackSurface()

```gml
.GetStackSurface(index);
```
Returns the specific stack rendering surface.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| index | Real/Enum | The stack index. |  

#### Returns: [Surface] - Surface id.

### Example:
```gml
var _surface = renderer.GetStackSurface(0);
```
In the example above, we are getting the surface from the "Base" stack (with effects like: Rotation, Zoom, etc).

<br><br><br>




## .GetEffectParameter()

```gml
.GetEffectParameter(effect, param);
```
Returns the specific stack rendering surface.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect | Macro(String) | Effect name. Use the macro starting with FF_, example: FF_BLOOM. Or, the effectName: "bloom".|  
| param | Macro | Parameter macro. Example: PP_BLOOM_COLOR. |  

#### Returns: [Any] - Effect parameter value.

### Example:
```gml
var _intensity = renderer.GetEffectParameter(FF_BLOOM, PP_BLOOM_INTENSITY);
```
In the example above, we get the intensity of the Bloom effect.

<br><br><br>




## .GetRenderResolution()

```gml
.GetRenderResolution();
```
Returns the post-processing renderer rendering percentage.


#### Returns: [Real] - Render resolution.

### Example:
```gml
var _resolution = renderer.GetRenderResolution();
```
In the example above, we get the render resolution of the renderer renderer.

<br><br><br>





## .SetBakingLUT()

```gml
.SetBakingLUT(sprite);
```
Set a sprite (neutral LUT image) to bake all color grading stack into it.

> Note that this is a secret function and therefore does not appear in Intellisense.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| sprite | Sprite | The neutral linear LUT to be used to bake. You can use a LUT returned by PPFX_LUTGenerator(). Use -1 to remove it. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
lut_generator_baking = new PPFX_LUTGenerator();
renderer.SetBakingLUT(lut_generator_baking.GenerateGrid(8, true));
```
In the example above, we created a neutral LUT sprite and sent it to PPFX.

<br><br><br>

 



## .GetBakedLUT()

```gml
.GetBakedLUT();
```
Bake all color grading stack into a LUT image.


#### Returns: [Sprite] - The LUT sprite.

### Example:
```gml
var _sprite = renderer.GetBakedLUT();
sprite_save(_sprite, 0, "LUT.png");
```
In the example above, we get the LUT sprite and save it to disk.

<br><br><br>





# PPFX_Profile()

The profile is like a container, which contains the effects you want the system to render. They alone don't do anything, it's just a data structure. You load them into the post-processing system. Each post-processing system can only have 1 profile loaded into it, at the same time (you can also merge profiles). You can have infinite profiles with the combinations you want, in any order.

The profile name can be anything you like. This is just for debugging purposes.

If for example the game Menu has different effects than Level 1, you will create a different profile for both, and then load them properly.

> If you are using effects in fullscreen, we recommend using just one PPFX system and multiple profiles, for better flexibility. (You can use as many as you like too).

> Profiles do not need to be destroyed, as they are structs.

```gml
PPFX_Profile(name, effects_array);
```
Creates a profile with predefined effects.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| name | String | Profile name. It only serves as a cosmetic and nothing else. |  
| effects_array | Array | Array with all effects structs. |  

#### Returns: [Struct] - Profile id

### Example:
```gml
var effects = [
    new FX_SineWave(true),
];
profile = new PPFX_Profile("Main", effects);
```
The code above creates a profile and associates its id with profile.

<br><br><br>





## .SetName()

```gml
.SetName(name);
```
Set the name of the profile, created with "new PPFX_Profile()".
This feature is just cosmetic and nothing more.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| name | String | New profile name. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
profile.SetName("Menu");
```

<br><br><br>





## .GetName()

```gml
GetName();
```
Get the name of the profile, created with "new PPFX_Profile()".

#### Returns: [String] - Profile name

### Example:
```gml
var _name = profile.GetName();
```
In the example above, we returned the name of the profile. Example: "Menu".

<br><br><br><br>





## .Stringify()

```gml
.Stringify(round_numbers, enabled_only);
```
This function parses and exports the profile in GML, for easy use.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| round_numbers | Bool | Sets whether to round numbers (removing decimals). |  
| enabled_only | Bool | Defines if will export only enabled effects. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
var _text = profile.Stringify(false, true);
clipboard_set_text(_text);
```
In the example above, we get the code in GML with all the parameters of the effects.
Something like this:
```
game_profile = new PPFX_Profile("Game", [
	new FX_Rotation(true, 10),
]);
```
> Please note: there are some things that cannot be exported, such as textures, as you will have to define your own. So "undefined" will be used instead.

<br><br><br>






# Areas

Uses the final image texture from the post-processing renderer as the texture for what to draw next. You MUST call ppfx_area_mask_end() when you are done drawing masks.  

If for example you draw a sprite after calling this function, the alpha channel of the sprite will be taken into account, but using the PPFX texture.  

You can draw multiple things after calling this function, using the same renderer texture.  

You can call this function multiple times for the same renderer.


## ppfx_area_mask_begin()

Draw part of the post-processing system surface in a rectangular area.

```gml
ppfx_area_mask_begin(renderer);
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| renderer | Struct | The returned variable by PPFX_Renderer(). |  

Example:

```gml
ppfx_area_mask_begin(blurRenderer);
draw_rectangle(20, 20, 20+256, 20+256, false);
draw_sprite_stretched_ext(sprAreaMask2, 0, 20, 300, 256, 100, c_lime, 1);
draw_sprite_ext(sprPPFXIcon, 0, 400, 400, 0.25, 0.25, -current_time*0.1, c_white, 1);

ppfx_area_mask_begin(orangeRenderer);
draw_rectangle(300, 20, 300+256, 20+256, false);

ppfx_area_mask_begin(grayscaleRenderer);
draw_sprite_stretched_ext(sprAreaMask, 0, 620, 20, 256, 256, c_white, 1);

ppfx_area_mask_end();
```


<br><br><br>



## ppfx_area_mask_end()

This should be called after all ppfx_area_mask_begin() calls, just once. This is basically a shader_reset().

```gml
ppfx_area_mask_end();
```

<br><br><br>






Give me a cookie :)
