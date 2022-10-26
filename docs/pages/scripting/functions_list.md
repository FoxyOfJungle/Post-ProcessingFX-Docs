
## PPFX Functions List <!-- {docsify-ignore} -->

The following are all the functions that Post-Processing FX has to generate the effects and control them.  
Although the topics are separate, they are all linked in some way. Parameters inside brackets are optional.
<br>


# Application Render

> The following functions handle the initial preparation of the game rendering. Use these functions to disable application_surface so that PPFX has full control of rendering.

## ppfx_application_render_init()

```gml
ppfx_application_render_init();
```
This function disables the default drawing of application_surface.  
It only needs to be used once, which can be on an object in the first room of the game, or even in a script.

#### Returns: [Undefined] >> N/A

<br><br><br>



## ppfx_application_render_free()

```gml
ppfx_application_render_free();
```
This function activates the default application_surface again, and causes the Post-Processing FX to no longer be displayed. If you're looking to destroy a post-processing system, use ppfx_destroy() instead.

#### Returns: [Undefined] - N/A

<br><br><br>



## ppfx_application_render_set_enable()


```gml
ppfx_application_render_set_enable([enabled]);
```
Toggle whether or not to enable the default drawing of application_surface.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| [enabled] | Real | Will be either true (enabled, the default value) or false (disabled).<br>The drawing will toggle if nothing or -1 is entered. |  

#### Returns: [Undefined] - N/A

<br><br><br><br>





# PPFX System

> The following functions deal with the post-processing system itself.

## ppfx_create()

```gml
ppfx_create();
```
Create post-processing system id to be used by the other functions.

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_id = ppfx_create();
```
The example above creates a system and references it in a variable.

<br><br><br>



## ppfx_destroy()

```gml
ppfx_destroy(pp_index);
```
Destroy a previously created post-processing system, freeing from memory all the resources it created at runtime. Only use this if you will no longer be using a system.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  

#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_destroy(ppfx_id);
```
The example above destroys the "ppfx_id" post-processing system.

<br><br><br>



## ppfx_draw()

```gml
ppfx_draw(surface, x, y, w, h, view_w, view_h, pp_index);
```
Draw post-processing system on screen

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| surface | Surface | Render surface to copy from. (You can use application_surface). |  
| x | Real | Horizontal X position of rendering. |  
| y | Real | Vertical Y position of rendering. |  
| w | Real | Width of the stretched game view. |  
| h | Real | Height of the stretched game view. |  
| view_w | Real | Width of your game view (Can use view port or window/gui width, for example). |  
| view_h | Real | Height of your game view (Can use view port or window/gui height, for example). |  
| pp_index | Struct | The returned variable by ppfx_create(). |  

#### Returns: [Undefined] - N/A

### Example:
```gml
var _pos = application_get_position();
var _view_width = surface_get_width(application_surface), _view_height = surface_get_height(application_surface);

ppfx_draw(application_surface, _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1], _view_width, _view_height, ppfx_id);
```
The above example draws the "ppfx_id" post-processing system on the screen. Inside Post-Draw event and at position 0, 0, using the window size and using the camera resolution.

<br><br><br>



## ppfx_exists()

```gml
ppfx_exists(pp_index);
```
Check if post-processing system exists.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  


#### Returns: [Bool] - If system exists or not

### Example:
```gml
var _exists = ppfx_exists(pp_index);
```

<br><br><br>



## ppfx_set_global_intensity()

```gml
ppfx_set_global_intensity(pp_index, value);
```
Defines the overall draw intensity of the post-processing system.
The global intensity defines the interpolation between the original image and with the effects applied.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  
| value | Struct | Intensity, 0 for none (default image), and 1 for full. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_set_global_intensity(ppfx_id, 0.5);
```
In the example above, the global system strength ppfx_id is set to 0.5.

<br><br><br>



## ppfx_set_draw_enable()

```gml
ppfx_set_draw_enable(pp_index, enable);
```
Toggle whether the post-processing system can draw.  
Please note that if disabled, it may still be rendered if rendering is enabled (will continue to demand GPU).


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_set_draw_enable(ppfx_id, false);
```
In the example above, we disable system drawing.

<br><br><br>



## ppfx_set_render_enable()

```gml
ppfx_set_render_enable(pp_index, enable);
```
Toggle whether the post-processing system can render.
Please note that if enabled, it can render to the surface even if not drawing! Disabling this releases the system usage on the GPU.


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  
| enable | Bool/Real | Will be either true (enabled, the default value) or false (disabled). The drawing will toggle if nothing or -1 is entered. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_set_render_enable(ppfx_id, false);
```
In the example above, we disable system rendering.

<br><br><br>



## ppfx_set_render_resolution()

```gml
ppfx_set_render_resolution(pp_index, resolution);
```
This function modifies the final rendering resolution of the post-processing system, based on a percentage (0 to 1).


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  
| resolution | Real | Value from 0 to 1 that is multiplied internally with the final resolution of the system's final rendering view. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_set_render_resolution(ppfx_id, 0.5);
```
In the example above, we set the render resolution to half size.

<br><br><br>



## ppfx_get_global_intensity()

```gml
ppfx_get_global_intensity(pp_index, resolution);
```
Get the overall draw intensity of the post-processing system.
The global intensity defines the interpolation between the original image and with the effects applied.


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  


#### Returns: [Real] - Global intensity, value from 0 to 1

### Example:
```gml
var _intensity = ppfx_get_global_intensity(ppfx_id);
```
In the example above, we get the global intensity of the ppfx_id system.

<br><br><br>



## ppfx_get_render_resolution()

```gml
ppfx_get_render_resolution(pp_index);
```
Returns the post-processing system rendering percentage (0 to 1).


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  


#### Returns: [Real] - Render resolution, value from 0 to 1

### Example:
```gml
var _resolution = ppfx_get_render_resolution(ppfx_id);
```
In the example above, we get the render resolution of the ppfx_id system.

<br><br><br>



## ppfx_get_render_surface()

```gml
ppfx_get_render_surface(pp_index);
```
Returns the post-processing system final rendering surface.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  


#### Returns: [Surface] - Surface id

### Example:
```gml
ppfx_draw(ppfx_get_render_surface(ppfx_id), _xx, _yy, _ww, _wh, _vw, _vh, ui_blur_id);
```
In the example above, a second system is drawn using the final surface of the previous system.

<br><br><br>



## ppfx_is_draw_enabled()

```gml
ppfx_is_draw_enabled(pp_index);
```
Returns the post-processing system rendering percentage (0 to 1).


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  


#### Returns: [Bool] - If system drawing is enabled or not

### Example:
```gml
var _resolution = ppfx_get_global_intensity(ppfx_id);
```
In the example above, we get the render resolution of the ppfx_id system.

<br><br><br>




## ppfx_is_render_enabled()

```gml
ppfx_is_render_enabled(pp_index);
```
Returns true if post-processing system rendering is enabled, and false if not.


| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create() |  


#### Returns: [Bool] - If system drawing is enabled or not

### Example:
```gml
var _resolution = ppfx_get_global_intensity(ppfx_id);
```
In the example above, we get the render resolution of the ppfx_id system.

<br><br><br><br>



# Profiles

> Profiles

## ppfx_profile_create()

```gml
ppfx_profile_create(name, effects_array);
```
Create a profile with predefined effects.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| name | String | Profile name. It only serves as a cosmetic and nothing else. |  
| effects_array | Array | Array with all effects structs. |  


#### Returns: [Struct] - Profile id

### Example:
```gml
var effects = [
    new pp_sine_wave(true, 1),
];
main_profile = ppfx_profile_create("Main", effects);
```
The code above creates a profile and associates its id with main_profile.

<br><br><br>



## ppfx_profile_load()

```gml
ppfx_profile_load(pp_index, profile_index);
```
This function loads a previously created profile.
Which means that the post-processing system will apply the settings of the effects contained in the profile, showing them consequently.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| profile_index | Struct | Profile id created with ppfx_profile_create(). |  


#### Returns: [Undefined] - N/A

### Example:
```gml
main_profile = ppfx_profile_create("Main", effects);

ppfx_profile_load(ppfx_id, main_profile);
```
The code above creates a profile and loads it into the ppfx_id id system.

<br><br><br>



## ppfx_profile_set_name()

```gml
ppfx_profile_set_name(profile_index);
```
Set the name of the profile, created with ppfx_profile_create().
This feature is just cosmetic and nothing more.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| profile_index | Struct | Profile id created with ppfx_profile_create(). |  
| name | String | New profile name. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_profile_set_name(ppfx_id, "Menu");
```

<br><br><br>



## ppfx_profile_get_name()

```gml
ppfx_profile_get_name(profile_index);
```
Get the name of the profile, created with ppfx_profile_create().

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| profile_index | Struct | Profile id created with ppfx_profile_create(). |  


#### Returns: [String] - Profile name

### Example:
```gml
var _name = ppfx_profile_get_name(ppfx_id);
```

<br><br><br><br>




# Effects

> The following functions control the behavior of effects.  
Important: Note that you can only disable or enable effects that are in the system (which have been loaded from a profile).

## ppfx_effect_set_enable()

```gml
ppfx_effect_set_enable(pp_index, effect_index, enable);
```
This function toggles the effect rendering.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| effect_index | Real | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  
| enable | Bool | Will be either true (enabled) or false (disabled). The rendering will toggle if nothing or -1 is entered. |  



#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_effect_set_enable(pp_index, PP_EFFECT.BLOOM, false);
```
In the example above, Bloom's effect is disabled.

<br><br><br>



## ppfx_effect_set_parameter()

```gml
ppfx_effect_set_parameter(pp_index, effect_index, param, value);
```
Modify a single parameter (setting) of an effect.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| effect_index | Enum | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  
| param | String | Parameter macro. Example: PP_BLOOM_COLOR. |  
| value | Any | Parameter value. Example: make_color_rgb_ppfx(255, 255, 255). |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_effect_set_parameter(ppfx_id, PP_EFFECT.ZOOM, PP_ZOOM_AMOUNT, 1.4);
```
In the example above, we are modifying the zoom amount to 1.4, using PP_ZOOM_AMOUNT.  

<br><br><br>



## ppfx_effect_set_parameters()

```gml
ppfx_effect_set_parameters(pp_index, effect_index, params_array, values_array);
```
Modify various parameters (settings) of an effect.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| effect_index | Enum | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  
| params_array | Array | Array with the effect parameters. Use the pre-defined macros, for example: PP_BLOOM_COLOR. |  
| values_array | Array | Array with parameter values, must be in the same order. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_effect_set_parameters(ppfx_id, PP_EFFECT.BLOOM, [PP_BLOOM_INTENSITY, PP_BLOOM_THRESHOLD], [3, 0.7]);
```
In the example above, we set the intensity and threshold to 3 and 0.7 respectively.

<br><br><br>



## ppfx_effect_get_parameter()

```gml
ppfx_effect_get_parameter(pp_index, effect_index, param, value);
```
Get a single parameter (setting) value of an effect.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| effect_index | Enum | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  
| param | String | Parameter macro. Example: PP_BLOOM_COLOR. |  


#### Returns: [Any] - Returned value

### Example:
```gml
var _value = ppfx_effect_get_parameter(ppfx_id, PP_EFFECT.BLOOM, PP_BLOOM_INTENSITY);
```
In the example above, I get the intensity value from Bloom.

<br><br><br>



## ppfx_effect_is_enabled()

```gml
ppfx_effect_is_enabled(pp_index, effect_index);
```
Returns true if effect rendering is enabled, and false if not.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| effect_index | Enum | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  


#### Returns: [Bool] - If is enabled

### Example:
```gml
var _is_bloom_enabled = ppfx_effect_is_enabled(ppfx_id, PP_EFFECT.BLOOM);
```

<br><br><br><br>




# Layers

> Layers

## ppfx_layer_create()

```gml
ppfx_layer_create();
```
Create post-processing layer id to be used by the other functions.


#### Returns: [Undefined] - N/A

### Example:
```gml
layer_index = ppfx_layer_create();
```

<br><br><br>



## ppfx_layer_destroy()

```gml
ppfx_layer_destroy(pp_layer_index);
```
This function deletes a previously created post-processing layer, freeing memory.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_layer_index | Struct | The returned variable by ppfx_layer_create() |  


#### Returns: [Undefined] - N/A

### Example:
```gml
ppfx_layer_destroy(layer_index);
```

<br><br><br>



## ppfx_layer_apply()

```gml
ppfx_layer_apply(pp_index, pp_layer_index, top_layer_id, bottom_layer_id, room_size_based, draw_layer);
```
This function applies post-processing to one or more layers. You only need to call this ONCE in an object's Create Event (or Room Start).  
Make sure the top layer is above the bottom layer in order. If not, it may not render correctly.  
Please note: You CANNOT select a range to which the layer has already been in range by another system. This will give an unbalanced surface stack error. If you want to use more effects, just add more effects to the profile.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_index | Struct | The returned variable by ppfx_create(). |  
| pp_layer_index | Struct | The returned variable by ppfx_layer_create() |  
| top_layer_id | Layer ID | Effect index. The top layer, in the room editor. |  
| bottom_layer_id | Layer ID | Effect index. Use the enumerator, example: PP_EFFECT.BLOOM. |  
| room_size_based | Bool | room_size_based If true, the system will use room size to render effects (requires more GPU resources). While off, uses camera position and size. |  
| draw_layer | Bool | If false, the layer will not draw and the layer content will still be rendered to the surface. |  


#### Returns: [Undefined] - N/A

### Example:
```gml
layer_effects = ppfx_create();
var _profile = ppfx_profile_create("Cool Effect", [
	new pp_colorize(true, make_color_rgb(200, 34, 60)),
]);
ppfx_profile_load(layer_effects, _profile);

layer_index = ppfx_layer_create();
ppfx_layer_apply(layer_effects, layer_index, layer_get_id("Background_0"), layer_get_id("Background_0"), false); 
```
In the example above, a profile is created with a colorize effect, then it is associated with a layer, applying the effects in a range.

<br><br><br>



## ppfx_layer_exists()

```gml
ppfx_layer_exists(profile_index);
```
Checks if a previously created post-processing layer exists.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_layer_index | Struct | The returned variable by ppfx_layer_create() |  


#### Returns: [Bool] - N/A

### Example:
```gml
ppfx_layer_destroy(layer_index);
```

<br><br><br>



## ppfx_layer_get_surface()

```gml
ppfx_layer_get_surface(pp_layer_index, with_effects);
```
This function gets the final surface used in the PPFX layer system.

| Name | Type | Description |  
|-----------|:-----------:|-----------:|  
| pp_layer_index | Struct | The returned variable by ppfx_layer_create(). |  
| with_effects | Bool | If true, it will return the surface of the layer with the effects applied. False is without the effects. |  


#### Returns: [Surface] - Surface id

### Example:
```gml
if keyboard_check_pressed(vk_space) {
	var _surface = ppfx_layer_get_surface(layer_index, true);
	surface_save(_surface, "layer_surface.png");
}
```
In the example above, the layer content is captured to a .png image file, when pressing space.

<br><br><br>

## ppfx_layer_is_ready()

## ppfx_layer_set_range()

## ppfx_layer_set_render_enable()






# Areas

> Areas

## area_draw_rect()

## area_draw_sprite_mask()

## area_draw_sprite_ext_mask()

## area_draw_sprite_stretched_mask()

## area_draw_sprite_stretched_ext_mask()

















abbbbbbbbbbbbbbb





