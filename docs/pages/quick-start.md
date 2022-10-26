
## Getting started with Post-Processing FX <!-- {docsify-ignore} -->

This page explains how to easily add post-processing effects to your game.
This quick-start guide requires a basic knowledge of GML and GameMaker.  
Estimated setup time: 10 min or less.

## Create a Manager object <!-- {docsify-ignore} -->

In order to display the effects of Post-Processing, you first need to create an object that will control post-processing in general.  
It is recommended that this object be one of the first objects to be created in the game.

> Tip: It can be an object called obj_post_processing. For example:

![Bulb + PPFX](/../images/QuickStart_0.png)

### Persistent

Make sure object persistence is enabled, so Post-Processing will be able to draw throughout the game, without being destroyed when changing rooms.

![Bulb + PPFX](/../images/QuickStart_1.png)

### Create Event

Add a **Create Event** to the object if it doesn't exist

![Bulb + PPFX](/../images/QuickStart_2.gif)


## Disable default drawing <!-- {docsify-ignore} -->

In order for Post-Processing FX to have full control of the final drawing of the game, you need to disable <a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Reference%2FDrawing%2FSurfaces%2Fapplication_surface.htm" target="_blank">application_surface</a>. For that, just use ppfx_application_render_init() just once, during the entire game.

```gml
ppfx_application_render_init();
```

## Creating a PPFX system <!-- {docsify-ignore} -->

A system is an instance of the post-processing constructor object. In other words, it is a struct that is created, which contains the important internal variables for the effects to work. As Post-Processing FX is modular, it allows you to create multiple systems.  

We will use the system as a basis for the other functions of Post-Processing FX.

To create a system, use the ppfx_create() function and bind it to a variable:
```gml
ppfx_id = ppfx_create();
```
In the example above, we set the variable name to "ppfx_id", this can be any other name.


### Adding Effects

Effects need to be added to an 1D array. Look at the following example:

```gml
var effects = [
	new pp_colorize(true, c_orange),
	new pp_sine_wave(true, 1),
];
```
In the code above, the variable "effects" receives an array. Each index (position) of the array will receive the struct of the effect. The order you add the effects doesn't matter, they all already have their own order internally.  

You must use the prefix "<a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Overview%2FLanguage_Features%2Fnew.htm" target="_blank">new</a>" to instantiate the effect object.

The effects have all parameters with default values, with the exception of the first one (enabled). This is to speed up the setup for visualization of the effects in the game.
That is, these arguments are optional, for example:

![Optional Parameters](/../images/QuickStart_2-1.png)

You can middle click on the effect function to see the default parameter values.
Note that effects start with the prefix "pp_", to make finding all effects faster in GameMaker's intellisense (code completion).


## Profiles <!-- {docsify-ignore} -->

You will hear about "Profiles", they are basically a set of effects, which tell the asset which effects to use to display on the screen. You can have infinite profiles with the combinations you want.

In the previous explanation you created the "effects" array, now you just need to create a profile and associate the effects to a profile:

```gml
main_profile = ppfx_profile_create("Main", effects);
```
In the example above, we store the id "main_profile" of the profile, which contains our effects. The profile name can be anything you like, we named it "Main".

Now for the effects to be applied to the system, we need to load the profile:

```gml
ppfx_profile_load(ppfx_id, main_profile);
```

If for example the game Menu has different effects than Level 1, you will create a different profile for both, and then load them properly.

> We recommend using just one PPFX system and multiple profiles, for better flexibility.
(You can use as many as you like and render one on top of the other (stacking), but it's a more advanced
topic).

> Tip: You can use a macro to refer to the post-processing object:  
#macro PPFX obj_post_processing  
Usage:  
ppfx_profile_load(PPFX.ppfx_id, menu_1_profile); // An example loading a profile to the Menu.



## Final Creation Code

So, our final creation code looked like this:
```gml
// Call it only once
ppfx_application_render_init();

// Create ppfx system
ppfx_id = ppfx_create();

// Create profile with all effects
var effects = [
	new pp_colorize(true, c_orange),
	new pp_sine_wave(true, 1),
];
main_profile = ppfx_profile_create("Main", effects);

// Load profile, so all effects will be used
ppfx_profile_load(ppfx_id, main_profile);
```
That's all you need to initialize everything.

## Drawing <!-- {docsify-ignore} -->

To render the Post-Processing on the game screen, all you need is to call the ppfx_draw() function.

We strongly recommend using the `Post-Draw event` only (although you can use the *Draw GUI Begin* event as well). Because this event is independent of cameras, views and others. This way we have full control of the game drawing.

Reference: <a href="https://manual-en.yoyogames.com/#t=The_Asset_Editors%2FObject_Properties%2FDraw_Events.htm">Draw Events</a>.

## Final Draw Code

```gml
// Get exact position and size of original application_surface
var _pos = application_get_position();
var _view_width = surface_get_width(application_surface), _view_height = surface_get_height(application_surface);

// Draw post-processing on screen
ppfx_draw(application_surface, _pos[0], _pos[1], _pos[2]-_pos[0], _pos[3]-_pos[1], _view_width, _view_height, ppfx_id);
```

The code above draws the game screen with post-processing applied on the `Post-Draw event`.  
It is worth remembering that `Post-Draw event is independent of cameras and views`.  
It is based on the size of the display buffer (*window* size), and is placed *before* the Draw GUI events.  

> Despite being independent of view ports, you can use their position and size as a base, to use in split-screen games, for example.

When using the ppfx_draw() function, be aware that:  
<ul class="a">
<li>You should only use it once for each PPFX system. (You can have multiple systems too).</li>
<li>"x" and "y" are the positions (from Window, GUI, View port... depends on where it will be drawn).</li>
<li>"w" and "h" is the stretched size of the game view.</li>
<li>"view_w" and "view_h" is the size of your game view (Can be viewport or application_surface size, for example).</li>
</ul>
If being drawn in "Post-Draw" event, Use the <a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Reference%2FDrawing%2FSurfaces%2Fapplication_get_position.htm" target="_blank">application_get_position()</a>. If it's "Draw GUI Begin", the <a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Reference%2FCameras_And_Display%2Fdisplay_get_gui_width.htm" target="_blank">GUI size</a> and 0, 0 position.

It is very important to mention that the larger the view size, the more it will demand from the GPU.  

</br></br>
The following image will better illustrate how it works:

![Optional Parameters](/../images/QuickStart_2-2.png)
*Game: Super Mario World 2 - Yoshi's Island. Copyright (C) 1995, Nintendo. Used for illustration only.*


## Conclusion

With that it is enough to apply the effects on the screen:

![PPFX](/../images/QuickStart_3.png)
