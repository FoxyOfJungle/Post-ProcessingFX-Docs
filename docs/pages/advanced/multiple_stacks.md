
## Using multiple stacks <!-- {docsify-ignore} -->

This guide shows you how to use multiple stacks.

Imagine stacks as layers on top of another. That's basically it, applying effects on top of the underneath post-processing.

![Layer Range](/./images/MultipleStacks_0.png)

This is particularly useful when you want to use the same effect for different purposes. Example: using `Displacement Maps` effect for shockwaves and underwater. It is possible to obtain better results using two systems.

## Creating two different systems <!-- {docsify-ignore} -->

You must create different systems, where each one will act as a stack.

### Stack 1: Shockwaves

Create Event:
```gml
// create post-processing system id to be used by the other functions
ppfx_stack1 = ppfx_create();

// create profile with displacemap
var _profile = ppfx_profile_create("Shockwaves", [
	new pp_displacemap(true, 0.1, 1, 0, 0),
]);

// load profile
ppfx_profile_load(ppfx_stack1, _profile);

// init shockwaves distortion surface
shockwaves_sys_id = shockwave_create();
```

Draw Event:

```gml
// renderize shockwaves surface
shockwave_render(ppfx_stack1, shockwaves_sys_id, [__obj_ppf_shockwave], view_camera[0]);
```

For better performance, disable the first stack auto-draw with the `ppfx_set_draw_enable()` function:

```gml
ppfx_set_draw_enable(ppfx_stack1, false);
```


### Stack 2: Underwater

Create Event:
```gml
// create post-processing system id to be used by the other functions
ppfx_stack2 = ppfx_create();

// create profile with some effects
var _profile = ppfx_profile_create("Underwater", [
	new pp_displacemap(true, 0.1, 1, 0.1, 0, sprite_get_texture(__spr_ppf_water_normal, 0)),
	new pp_colorize(true, c_blue, 0.5, 0),
	new pp_contrast(true, 1.1),
]);

// load profile
ppfx_profile_load(ppfx_stack2, _profile);
```


### Draw Everything

Now that we've created the stacks, we need to draw them.
In `Post-Draw` event:

```gml
var _pos = application_get_position();
var _view_width = surface_get_width(application_surface), _view_height = surface_get_height(application_surface);

ppfx_draw(application_surface, _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1], _view_width, _view_height, ppfx_stack1);
ppfx_draw(ppfx_get_render_surface(ppfx_stack1), _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1], _view_width, _view_height, ppfx_stack2);
```

Note that the last stack (ppfx_stack2) is what is being displayed in the the screen. It gets the surface of the first stack (ppfx_stack1), which contains the shockwaves.


## Result <!-- {docsify-ignore} -->

So we have water and shockwaves together! You can make an amazing game underwater!

![Layer Range](/./images/MultipleStacks_1.gif)












