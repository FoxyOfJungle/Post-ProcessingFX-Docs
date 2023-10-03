
## Getting started with Post-Processing FX <!-- {docsify-ignore} -->

This page explains how to easily add post-processing effects to your game.
This quick-start guide requires a basic knowledge of GML and GameMaker. (No shader knowledge required).  
Estimated setup time: 10 min or less.

## Create a Manager object <!-- {docsify-ignore} -->

In order to display the effects of Post-Processing, you first need to create an object that will control post-processing in general.  
It is recommended that this object be one of the first objects to be created in the game.

> Tip: It can be an object called obj_post_processing. For example:

![Bulb + PPFX](/../images/QuickStart_0.png)

### Persistent

Make sure object persistence is enabled, so Post-Processing will not be destroyed when changing rooms.

![Bulb + PPFX](/../images/QuickStart_1.png)

### Create Event

Add a **Create Event** to the object if it doesn't exist

![Bulb + PPFX](/../images/QuickStart_2.gif)


## Disable default drawing <!-- {docsify-ignore} -->

In order for Post-Processing FX to have full control of the final drawing of the game, you need to disable <a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Reference%2FDrawing%2FSurfaces%2Fapplication_surface.htm" target="_blank">application_surface</a>.</br>
For that, use the following function just once. This will work through the rest of the game.

```gml
application_surface_draw_enable(false);
```

## Creating a PPFX system <!-- {docsify-ignore} -->

The system is responsible for rendering the effects somewhere (either full screen or in layers). It contains the important internal variables for the effects to work. As Post-Processing FX is modular, it allows you to create multiple systems.  

We will use the system as a basis for the other post-processing functionality, including changing effects behaviours.

To create a system, use the PPFX_System() function constructor and bind it to a variable:
```gml
ppfx_id = new PPFX_System();
```
In the example above, we set the variable name to "ppfx_id", this can be any other name.

> You must use the prefix/operator "<a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Overview%2FLanguage_Features%2Fnew.htm" target="_blank">new</a>" to instantiate the object.


### Adding Effects

Effects need to be added to an 1D array. Look at the following example:

```gml
var effects = [
    new FX_Colorize(true, color_get_hue(c_orange), 200, 240),
    new FX_SineWave(true, 1),
];
```
In the code above, the variable "effects" receives an array. Each index (position) of the array will receive the struct of the effect. The order you add the effects doesn't matter, they all already have their own order internally.  

The effects have all parameters with default values, with the exception of the first one (enabled). This is to speed up the setup for visualization of the effects in the game.
That is, these arguments are optional, for example:

![Optional Parameters](/../images/QuickStart_2-1.png)

You can middle click on the effect function to see the default parameter values.
Note that effects start with the prefix "FX_", to make finding all effects faster in GameMaker's intellisense (code completion).

> In GameMaker, you can skip parameters if you do something like: new FX_Bloom(true,,, 4). This will make the effect use the predefined value.


## Profiles <!-- {docsify-ignore} -->

You will hear about "Profiles", they are basically a set of effects, which tell the system which effects to use to display on the screen. They alone don't do anything, it's just a data structure. You can have infinite profiles with the combinations you want.

![Optional Parameters](/../images/ProfilesEverywhere.jpg)

In the previous explanation you created the "effects" array, now you just need to create a profile and associate the effects to a profile:

```gml
main_profile = new PPFX_Profile("Main", effects);
```
In the example above, we store the id "main_profile" of the profile, which contains our struct with effects settings. The profile name can be anything you like, we named it "Main" - This is just for debug purposes.

Now for the effects to be applied to the system, we need to load the profile:

```gml
ppfx_id.ProfileLoad(main_profile);
```

If for example the game Menu has different effects than Level 1, you will create a different profile for both, and then load them properly.

> We recommend using just one PPFX system and multiple profiles for full-screen effects, for better flexibility. (You can use as many as you want, though).


## Final Creation Code

So, our final creation code looked like this:
```gml
// Disable default game rendering. Call it only once.
application_surface_draw_enable(false);

// Create ppfx system.
ppfx_id = new PPFX_System();

// Create profile with all effects.
var effects = [
    new FX_Colorize(true, color_get_hue(c_orange), 200, 240),
    new FX_SineWave(true, 1),
];
main_profile = new PPFX_Profile("Main", effects);

// Load profile, so all effects will be used.
ppfx_id.ProfileLoad(main_profile);
```
That's all you need to initialize everything.

> TIP: You can use a switch(room) and create and load the profiles in the Room Start event, for different rooms. You can use .ProfileUnload() to remove any profile from the system.


## Drawing <!-- {docsify-ignore} -->

To render the Post-Processing on the game screen, all you need is to call the `DrawInFullscreen()` or `Draw()` function/method. Draw() lets you define positions and sizes manually (useful for split-screen).

We strongly recommend using the `Post-Draw event` only (although you can use the *Draw GUI Begin* event as well - in case of problems with resolution in GX.Games, specifically). Because this event is independent of cameras, views and others. This way we have full control of the game drawing.

Reference: <a href="https://manual-en.yoyogames.com/#t=The_Asset_Editors%2FObject_Properties%2FDraw_Events.htm">Draw Events</a>.

> We strongly recommend you activate the "Clear Display Buffer" option in the room settings, to prevent ghosting or blending issues.


## Final Draw Code

The code below draws the game screen with post-processing applied in fullscreen.  

```gml
// Draw post-processing in full screen
ppfx_id.DrawInFullscreen(application_surface);
```

This function automatically detects the draw event you are drawing (Post-Draw or Draw GUI Begin).  

It uses the size of the referenced surface for internal rendering resolution (example: application_surface size).  

If you are using `.Draw()`: for the size parameters, width ("w") and height ("h"), it's the scaled rendering size: If drawing in Post-Draw, is the size of the window (frame buffer). If in Draw GUI Begin, the size of the GUI.

> It is very important to mention that the larger the surface/application_surface size, the more it will demand from the GPU.  


> It is worth remembering that `Post-Draw event is independent of cameras and views`.  
It is based on the size of the display buffer (*window* size), and is placed *before* the Draw GUI events. Which means the effects won't be applied on the game UI as GameMaker doesn't render the GUI event on the application_surface. (There are some ways around this and there is an example in the project just about that).  

> Despite being independent of view ports, you can use their position and size as a base, to use in split-screen games, for example (when using .Draw() method).

> Be aware that: You should only draw once for each PPFX system. (You can have multiple systems too).



</br></br>
If you use the `.Draw()` method, the following image will better illustrate how it works:

![Optional Parameters](/../images/QuickStart_2-2.png)
*Game: Super Mario World 2 - Yoshi's Island. Copyright (C) 1995, Nintendo. Used for illustration only.*


## Conclusion

With that it is enough to apply the effects on the screen:

![PPFX](/../images/QuickStart_3.png)

*"SquarX" game. Copyright (C) 2023, Kazan Games.*

