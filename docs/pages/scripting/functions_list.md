
## Post-Processing FX Functions List <!-- {docsify-ignore} -->

The following are all the functions that Post-Processing FX has to generate the effects and control them.  
Although the topics are separate, they are all linked in some way. Parameters inside brackets are optional.

Note: All content here is also available using Feather, from GameMaker, offline.

<br>



# PPFX_System()

`PPFX_System()` is essentially responsible for drawing the visual effects. It works both for full screen and for layers.
You can create as many systems as you need, depending on what you're going to do.
Examples:

<ul class="a">
<li>For a full-screen game, one system is enough;</li>
<li>To use in layers, you will need another system (do not use the same system as fullscreen), for each layer range;</li>
<li>For a split-screen game, you'll need 4 (or 2, depending on what you want) systems (unless you use the same color grading system for both 4 screens);</li>
<li>To use with Areas, you will need another system;</li>
</ul>

> By instantiating the system with "new" operator, you will get a struct, in which you will be able to control the system with the methods below.

> Systems need to be destroyed using .Destroy() when no longer used, to avoid memory leaks.


## .ProfileLoad()

```gml
.ProfileLoad(profile_index, merge);
```
This function loads a previously created profile.
Which means that the post-processing system will apply the settings of the effects contained in the profile, showing them consequently.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| profile_index | Struct | Profile id created with "new PPFX_Profile()". |  
| merge | Bool | If you want to merge the profile with existing effects in the system (without replacing). |  

#### Returns: [Undefined] - N/A

### Example:
```gml
main_profile = new PPFX_Profile("Main", effects);

ppfx_id.ProfileLoad(main_profile);
```
The code above creates a profile and loads it into the ppfx_id id system.

<br><br><br>




## .ProfileUnload()

```gml
.ProfileUnload();
```
This function removes any profile associated with this post-processing system, consequently disabling all effects.

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.ProfileUnload();
```
The code above causes the ppfx_id system to remove its profile.

<br><br><br>




## .Draw()

```gml
.Draw(surface, x, y, w, h, surface_width, surface_height);
```
Draw post-processing system on screen.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| surface | Surface | Render surface to copy from. (You can use application_surface). |  
| x | Real | The x position og where to draw the surface. |  
| y | Real | The y position og where to draw the surface. |  
| w | Real | The width at which to draw the surface. |  
| h | Real | The height at which to draw the surface. |  
| surface_width | Real | Width resolution of your game screen (Can use width of application_surface or viewport). |  
| surface_height | Real | Height resolution of your game screen (Can use height of application_surface or viewport). |  


#### Returns: [Undefined] - N/A

### Example:
```gml
var _pos = application_get_position();
var _view_width = surface_get_width(application_surface), _view_height = surface_get_height(application_surface);

ppfx_id.Draw(application_surface, _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1], _view_width, _view_height);
```
The above example draws the "ppfx_id" post-processing system on the screen. Inside Post-Draw event and at position 0, 0, using the window size and using the camera resolution.

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
ppfx_id.DrawInFullscreen(application_surface);
```
The above example draws the "ppfx_id" post-processing system on the screen.

<br><br><br>





## .Destroy()

```gml
.Destroy();
```
Destroy a previously created post-processing system, freeing from memory all the resources it created at runtime. Only use this if you will no longer be using a system.


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.Destroy();
```
The example above destroys the "ppfx_id" post-processing system.

<br><br><br>





## .Clean()

```gml
.Clean();
```
Clean post-processing system, without destroying it.
Useful for when toggling effects and want to make sure existing surfaces are destroyed.

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.Clean();
```
In the example above, the post-processing system is cleaned up, resetting all surfaces and so on.

<br><br><br>




## .SetEffectParameter()

```gml
.SetEffectParameter(effect_index, param, value);
```
Modify a single parameter (setting) of an effect.
Use this to modify an effect's attribute in real-time.

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect_index | Real | Use the enumerator, example: FX_EFFECT.BLOOM. |  
| param | Real | Parameter macro. Example: PP_BLOOM_COLOR. |  
| value | Real | Parameter value. Example: make_color_rgb_ppfx(255, 255, 255). |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetEffectParameter(FX_EFFECT.LENS_DISTORTION, PP_LENS_DISTORTION_AMOUNT, 0.5);
```
In the example above, we set the amount of distortion for the Lens Distortion effect to 0.5.

<br><br><br>





## .SetEffectParameters()

```gml
.SetEffectParameters(effect_index, param, value);
```
Modify many parameters (setting) of an effect. Use this to modify an effect's attribute in real-time.

> Note that this function uses a internal loop to access the array, which makes it slower.

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect_index | Real | Use the enumerator, example: FX_EFFECT.BLOOM. |  
| params_array | Real | Array with the effect parameters. Use the pre-defined macros, for example: [PP_BLOOM_COLOR, PP_BLOOM_INTENSITY]. |  
| values_array | Real | Array with parameter values, must be in the same order. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetEffectParameters(FX_EFFECT.BLOOM, [PP_BLOOM_INTENSITY, PP_BLOOM_THRESHOLD], [3, 0.7]);
```
In the example above, we specify the effect (which is an enum), the array of macros of the parameters we want to modify, and finally the desired array of value. They must need to be in the same order they were written.

<br><br><br>





## .SetEffectEnable()

```gml
.SetEffectEnable(effect_index, enable);
```
Toggle whether the post-processing system can render.
Please note that if enabled, it can render to the surface even if not drawing! Disabling this releases the system usage on the GPU.  

> Note that if the effect was active before and you turned it off later (using this function), its surface will still exist, so you need to use .Clean() in the post-processing system to clean up any unused surfaces. This is ONLY valid for effects that have individual stacks, such as: Bloom, Depth Of Field, FXAA, HQ4X, VHS, Palette Swap, Blurs, Chromatic Aberration, Sunshafts and Slow Motion.

![Draw Enable](/images/PPFXRenderEnable.gif)
*In the image above, the system, when deactivated, uses the original surface without the effects. So the surface of the disabled system stops updating.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect_index | Real/Enum | Use the enumerator, example: FX_EFFECT.BLOOM. |  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetEffectEnable(FX_EFFECT.BLOOM, false);
```
In the example above, we have deactivated the Bloom effect from the ppfx_id system.

<br><br><br>





## .SetDrawEnable()

```gml
.SetDrawEnable(enable);
```
Toggle whether the post-processing system can draw.  
Please note that if disabled, it may still be rendered if rendering is enabled (will continue to demand GPU).

![Draw Enable](/images/PPFXDrawEnable.gif)
*In the image above, another system still can to get its surface, even without drawing.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetDrawEnable(false);
```
In the example above, we disable system drawing.

<br><br><br>



## .SetRenderEnable()

```gml
.SetRenderEnable(enable);
```
Toggle whether the post-processing system can render.
Please note that if enabled, it can render to the surface even if not drawing! Disabling this releases the system usage on the GPU.  

![Draw Enable](/images/PPFXRenderEnable.gif)
*In the image above, the system, when deactivated, uses the original surface without the effects. So the surface of the disabled system stops updating.*

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetRenderEnable(false);
```
In the example above, we disable system rendering.

<br><br><br>





## .SetGlobalIntensity()

```gml
.SetGlobalIntensity(value);
```
Defines the overall draw intensity of the post-processing system.
The global intensity defines the interpolation between the original image and with the effects applied.

![Global Intensity](/images/GlobalIntensity.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| value | Struct | Intensity, 0 for none (default image), and 1 for full. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetGlobalIntensity(0.5);
```
In the example above, the global system strength ppfx_id is set to 0.5.

<br><br><br>





## .SetRenderResolution()

```gml
.SetRenderResolution(resolution);
```
This function modifies the final rendering resolution of the post-processing system, based on a percentage (0 to 1 - full).

![Render Resolution](/images/PPFXRenderResolution.gif)

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| resolution | Real | Value from 0 to 1 that is multiplied internally with the final resolution of the system's final rendering view. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetRenderResolution(0.5);
```
In the example above, we set the render resolution to half size.

<br><br><br>





## .IsDrawEnabled()

```gml
.IsDrawEnabled();
```
This function returns whether the system is drawing or not.

#### Returns: [Bool] - true: is drawing. false: not drawing.

### Example:
```gml
var _drawing = ppfx_id.IsDrawEnabled();
show_debug_message(_drawing);
```
In the example above, we return whether the system is drawing to a variable, and then show a message on the console.

<br><br><br>





## .IsRenderEnabled()

```gml
.IsRenderEnabled();
```
This function returns whether the system is rendering or not.

#### Returns: [Bool] - true: is rendering. false: not rendering.

### Example:
```gml
var _rendering = ppfx_id.IsRenderEnabled();
show_debug_message(_rendering);
```
In the example above, we return whether the system is rendering to a variable, and then show a message on the console.

<br><br><br>





## .IsEffectEnabled()

```gml
.IsEffectEnabled(effect_index);
```
This function returns whether an effect is enabled or not.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect_index | Real/Enum | Use the enumerator, example: FX_EFFECT.BLOOM. |  

#### Returns: [Bool] - true: is enabled. false: not enabled.

### Example:
```gml
var _enabled = ppfx_id.IsEffectEnabled(FX_EFFECT.BLOOM);
show_debug_message(_enabled);
```
In the example above, we return whether the effect is enabled to a variable, and then show a message on the console.

<br><br><br>





## .GetRenderSurface()

```gml
.GetRenderSurface();
```
Returns the post-processing system final rendering surface.

#### Returns: [Surface] - Surface id.

### Example:
```gml
ui_blur_id.DrawInFullscreen(ppfx_id.GetRenderSurface());
```
In the example above, a second system is drawn using the final surface of the previous system.

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
var _surface = ppfx_id.GetStackSurface(0);
```
In the example above, we are getting the surface from the "Base" stack (with effects like: Rotation, Zoom, etc).

<br><br><br>





## .GetEffectParameter()

```gml
.GetEffectParameter(effect_index, param);
```
Returns the specific stack rendering surface.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| effect_index | Real/Enum | Effect index. Use the enumerator, example: FX_EFFECT.BLOOM. |  
| param | Macro | Parameter macro. Example: PP_BLOOM_COLOR. |  

#### Returns: [Any] - Effect parameter value.

### Example:
```gml
var _intensity = ppfx_id.GetEffectParameter(FX_EFFECT.BLOOM, PP_BLOOM_INTENSITY);
```
In the example above, we get the intensity of the Bloom effect.

<br><br><br>






## .GetGlobalIntensity()

```gml
.GetGlobalIntensity();
```
Get the overall draw intensity of the post-processing system.
The global intensity defines the interpolation between the original image and with the effects applied.

#### Returns: [Real] - Global intensity, value from 0 to 1.

### Example:
```gml
var _intensity = ppfx_id.GetGlobalIntensity();
```
In the example above, we get the global intensity of the ppfx_id system.

<br><br><br>





## .GetRenderResolution()

```gml
.GetRenderResolution();
```
Returns the post-processing system rendering percentage (0 to 1).


#### Returns: [Real] - Render resolution, value from 0 to 1.

### Example:
```gml
var _resolution = ppfx_id.GetRenderResolution();
```
In the example above, we get the render resolution of the ppfx_id system.

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
ppfx_id.SetBakingLUT(lut_generator_baking.GenerateGrid(8, true));
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
var _sprite = ppfx_id.GetBakedLUT();
sprite_save(_sprite, 0, "LUT.png");
```
In the example above, we get the LUT sprite and save it to disk.

<br><br><br>





## ppfx_system_exists()

```gml
ppfx_system_exists(pp_index);
```
Check if post-processing system exists.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by "new PPFX_System()". |  

#### Returns: [Bool] - If system exists or not.

### Example:
```gml
if (ppfx_system_exists(ppfx_id)) {
    // do something
}
```
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
main_profile = new PPFX_Profile("Main", effects);
```
The code above creates a profile and associates its id with main_profile.

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
var _text = main_profile.Stringify(false, true);
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





# PPFX_LayerRenderer()

The Layer Renderer is responsible for using a post-processing system as a bridge to draw it in one or more layers (range).

> This system uses layer scripts internally, so please note that GameMaker is only able to use one script per layer. If you use another script on the layer, it will be replaced by the one in the layer renderer.

#### Returns: [Struct] - Layer Renderer id.

### Example:
```gml
// Range 1 (use effects for "Background_Mid_1" and "Background_Mid_0")
layer_index = new PPFX_LayerRenderer();
layer_index.Apply(ppfx_test_id, layer_get_id("Background_Mid_1"), layer_get_id("Background_Mid_0"));

// Range 2 (use effects for "Background_0" layer only)
layer_index2 = new PPFX_LayerRenderer();
layer_index2.Apply(ppfx_test2_id, layer_get_id("Background_0"), layer_get_id("Background_0")); 
```

In the example above, we created two layer renders, and associated each one with its system (ppfx_test_id and layer_index2).

> PLEASE NOTE: You need to create their own system for each Renderer. NEVER use the same ppfx system, this includes the same ppfx system used for FULLSCREEN.


<br><br><br>





## .Destroy()

```gml
.Destroy();
```
This function deletes a previously created post-processing layer, freeing memory.

#### Returns: [Undefined] - N/A

### Example:
```gml
layer_index.Destroy();
```

<br><br><br>





## .Apply()

```gml
.Apply(pp_index, top_layer_id, bottom_layer_id, draw_layer);
```
This function applies post-processing to one or more layers. You only need to call this ONCE in an object's Create Event (or Room Start).  
Make sure the top layer is above the bottom layer in order. If not, it may not render correctly.  
Please note: You CANNOT select a range to which the layer has already been in range by another system. This will give an unbalanced surface stack error. If you want to use more effects, just add more effects to the profile.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by "new PPFX_System()". |  
| top_layer_id | Layer ID | The top layer, in the room editor. For example: layer_get_id("Top_Layer") |  
| bottom_layer_id | Layer ID | The bottom layer, in the room editor. For example: layer_get_id("Bottom_Layer") |  
| draw_layer | Bool | If false, the layer will not draw and the layer content will still be rendered to the surface. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
// Create a ppfx System for the Layer Renderer.
ppfx_blur = new PPFX_System();
var _profile = new PPFX_Layer("Blur", [
	new FX_KawaseBlur(true),
]);
ppfx_blur.LoadProfile(_profile);

// Create Layer Renderer and use ppfx system to render effects to the layer.
layer_index = new PPFX_LayerRenderer();
layer_index.Apply(ppfx_blur, layer_get_id("Background_0"), layer_get_id("Background_0")); 
```
In the example above, a profile is created with a blur effect, then it is associated with a layer, applying the effects in a range.

<br><br><br>







## .GetSurface()

```gml
.GetSurface(with_effects);
```
This function gets the final surface used in the PPFX layer system.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| with_effects | Bool | If true, it will return the surface of the layer with the effects applied. False is without the effects. |  

#### Returns: [Surface] - Surface id

### Example:
```gml
if keyboard_check_pressed(vk_space) {
	var _surface = layer_renderer.GetSurface(layer_index, true);
	surface_save(_surface, "layer_surface.png");
}
```
In the example above, the layer content is captured to a .png image file, when pressing space.

<br><br><br>





## .IsRenderEnabled()

```gml
.IsRenderEnabled();
```
This function checks if the layer renderer is rendering.

#### Returns: [Bool] - true: is enabled. false: not enabled.

### Example:
```gml
var _rendering = layer_renderer.IsRenderEnabled();
```
In the example above, I return in a variable if the layer renderer is rendering.

<br><br><br>





## .IsReady()

```gml
.IsReady();
```
This function checks if the layer renderer is ready, which means that internally the system was created, as well as the surfaces, and it is possible to obtain its surface safely.

#### Returns: [Bool] - true: is enabled. false: not enabled.

### Example:
```gml
var _rendering = layer_renderer.IsReady();
```
In the example above, we return in a variable if the layer renderer is ready.

<br><br><br>





## .SetRange()

```gml
.SetRange(top_layer_id, bottom_layer_id);
```
This function defines a new range of layers from an existing Layer Renderer.

> When changing the range, the old layer scripts of the layers are reset.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| top_layer_id | Layer Id | The top layer, in the room editor. |  
| bottom_layer_id | Layer Id | The bottom layer, in the room editor. |  

#### Returns: [Surface] - Surface id

### Example:
```gml
layer_renderer.SetRange(layer_get_id("Background_Far_1"), layer_get_id("Background_Far_0"));
```
In the example above, we changed the Layer Renderer range to a new range of layers.

<br><br><br>





## .SetRenderEnable()

```gml
.SetRenderEnable(enable);
```
Toggle whether layer renderer can render on layer.
If disabled, nothing will be rendered to the surface. That is, the layer will be drawn without the effects. Disabling this releases the system usage on the GPU.  

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id.SetRenderEnable(false);
```
In the example above, we disable system rendering.

<br><br><br>





## ppfx_layer_renderer_exists()

```gml
ppfx_layer_renderer_exists(layer_renderer);
```
Checks if a previously created post-processing layer exists.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_layer_index | Struct | The returned variable by "new PPFX_LayerRenderer()". |  

#### Returns: [Bool] - N/A

### Example:
```gml
if (ppfx_layer_renderer_exists(layer_renderer)) {
	// do something
}
```
In the example above, we check if the layer renderer exists and run some code later.

<br><br><br>





# Areas

Area masks are areas of the screen on which you can draw only part of the post-processing. You can use sprites to crop the image, creating cool effects.

## area_draw_rect()

Draw part of the post-processing system surface in a rectangular area.

```gml
area_draw_rect(x, y, w, h, x_offset, y_offset, pp_index)
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| x | Real | The x coordinate of where to draw the sprite. |  
| y | Real | The y coordinate of where to draw the sprite. |  
| w | Real | The width of the area. |  
| h | Real | The height of the area. |  
| x_offset | Real | Defined by position X, minus this offset variable. Use 0 in the GUI dimension. |  
| y_offset | Real | Defined by position Y, minus this offset variable. Use 0 in the GUI dimension. |  
| pp_index | Struct | The returned variable by PPFX_System(). |  

<br><br><br>



## area_draw_sprite_mask()

Draw a normal sprite, but its texture is the post-processing texture.

```gml
area_draw_sprite_mask(sprite_mask, subimg, x, y, pp_index)
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| sprite_mask | Sprite | The sprite index to be used as a mask. It can be any color. It will only be used to "cut" the surface. |  
| subimg | Real | The subimg (frame) of the sprite to draw (image_index or -1 correlate to the current frame of animation in the object). |  
| x | Real | The x coordinate of where to draw the sprite. |  
| y | Real | The y coordinate of where to draw the sprite. |  
| pp_index | Struct | The returned variable by PPFX_System(). |  

<br><br><br>



## area_draw_sprite_ext_mask()

Draw a normal sprite extended, but its texture is the post-processing texture.

```gml
area_draw_sprite_ext_mask(sprite_mask, subimg, x, y, xscale, yscale, rot, color, alpha, pp_index)
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| sprite_mask | Sprite | The sprite index to be used as a mask. It can be any color. It will only be used to "cut" the surface. |  
| subimg | Real | The subimg (frame) of the sprite to draw (image_index or -1 correlate to the current frame of animation in the object). |  
| x | Real | The x coordinate of where to draw the sprite. |  
| y | Real | The y coordinate of where to draw the sprite. |  
| xscale | Real | The horizontal scaling of the sprite, as a multiplier: 1 = normal scaling, 0.5 is half etc... |  
| yscale | Real | The vertical scaling of the sprite as a multiplier: 1 = normal scaling, 0.5 is half etc... |  
| rot | Real | The rotation of the sprite. 0=right way up, 90=rotated 90 degrees counter-clockwise etc... |  
| color | Real | The color of the sprite. |  
| alpha | Real | The alpha of the sprite. |  
| pp_index | Struct | The returned variable by PPFX_System(). |  

<br><br><br>



## area_draw_sprite_stretched_mask()

Draw a normal sprite stretched, but its texture is the post-processing texture.

```gml
area_draw_sprite_stretched_mask(sprite_mask, subimg, x, y, w, h, pp_index)
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| sprite_mask | Real | The sprite index to be used as a mask. It can be any color. It will only be used to "cut" the surface. |  
| subimg | Real | The y coordinate of where to draw the sprite. |  
| x | Real | The x coordinate of where to draw the sprite. |  
| y | Real | The y coordinate of where to draw the sprite. |  
| w | Real | The width of the sprite. |  
| h | Real | The height of the sprite. |  
| pp_index | Struct | The returned variable by PPFX_System(). |  

<br><br><br>



## area_draw_sprite_stretched_ext_mask()

Draw a normal sprite stretched extended, but its texture is the post-processing texture.

```gml
area_draw_sprite_stretched_ext_mask(sprite_mask, subimg, x, y, w, h, color, alpha, pp_index)
```

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| sprite_mask | Real | The sprite index to be used as a mask. It can be any color. It will only be used to "cut" the surface. |  
| subimg | Real | The y coordinate of where to draw the sprite. |  
| x | Real | The x coordinate of where to draw the sprite. |  
| y | Real | The y coordinate of where to draw the sprite. |  
| x | Real | The width of the sprite. |  
| x | Real | The height of the sprite. |  
| color | Real | The color of the sprite. |  
| alpha | Real | The alpha of the sprite. |  

<br><br><br>


Give me a cookie :)
