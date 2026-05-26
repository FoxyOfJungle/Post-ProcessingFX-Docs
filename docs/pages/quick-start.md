
## Getting started with Post-Processing FX <!-- {docsify-ignore} -->

This page explains how to easily add post-processing effects to your game.
This quick-start guide requires a basic knowledge of GML and GameMaker. (No shader knowledge required).  

Estimated setup time: 10 min or less.  

First of all, the best way to learn how to use a library is not to try to understand how it works internally. You just need to understand how to use the offered functions. This is part of the SOLID concept. Everything has been carefully thought out to make it easier for the end user.  

> You can also see the project working in the .yyz example GM project.

> There is complete Feather documentation as well.


## Briefing <!-- {docsify-ignore} -->

This guide is divided into the following parts:  

1 - Importing Post-Processing FX package into your game;  
2 - Creating a manager object for the Post-Processing and add events with codes;  
3 - Adding effects;  
4 - Profiles;  
5 - Conclusion;  

</br>


## 1. Importing PPFX package <!-- {docsify-ignore} -->

See [installation](./pages/installation.md) guide.



## 2. Create a Manager object <!-- {docsify-ignore} -->

In order to display the effects of Post-Processing, you first need to create an object that will control post-processing in general.  

> Tip: It can be an object called objPostProcessing. For example:

![Quick Start](/../images/QuickStart_0.png)


### Create Event

Add a **Create Event** to the object if it doesn't exist

![Quick Start](/../images/QuickStart_2.gif)



### Creating a PPFX_Renderer()

The renderer is responsible for rendering the effects to a surface (either full screen or in layers). It contains the important internal variables for the effects to work. As Post-Processing FX is modular, it allows you to create multiple renderers.  

We will use the renderer as a basis for the other post-processing functionality, including changing effects behaviours.

To create a renderer, use the `PPFX_Renderer()` function constructor and bind it to a variable:
```gml
renderer = new PPFX_Renderer();
```
In the example above, we set the variable name to "renderer", this can be any other name.

> You must use the prefix/operator "<a href="https://manual-en.yoyogames.com/#t=GameMaker_Language%2FGML_Overview%2FLanguage_Features%2Fnew.htm" target="_blank">new</a>" to instantiate the object.

</br>


## 3. Adding Effects <!-- {docsify-ignore} -->

Effects need to be added to an array. Look at the following example:

```gml
var effects = [
    new FX_Colorize(true, c_orange),
    new FX_SineWave(true, 1),
];
```
In the code above, the variable "effects" receives an array. Each index (position) of the array will receive the struct of the effect. The order you add the effects doesn't matter, they all already have their own order internally (you can change it later, if you wish).  

The effects have all parameters with default values, with the exception of the first one (`enabled`). This is to speed up the setup for visualization of the effects in the game.
That is, these arguments are optional, for example:

![Optional Parameters](/../images/QuickStart_2-1.png)

You can middle click on the effect function to see the default parameter values.
Note that effects start with the prefix "`FX_`", to make finding all effects faster in GameMaker's intellisense (code completion).

> In GameMaker, you can skip parameters if you do something like: `new FX_Bloom(true,,,, 4)`. This will make the effect use the predefined value.

</br>


## 4. Profiles <!-- {docsify-ignore} -->

You will hear about "Profiles", they are basically a set of effects, which tell the renderer which effects to use to display on the screen. They alone don't do anything, it's just a data structure. You can have infinite profiles with the combinations you want.

![Optional Parameters](/../images/ProfilesEverywhere.jpg)

In the previous explanation you created the "effects" array, now you just need to create a profile and associate the effects to a profile:

```gml
mainProfile = new PPFX_Profile("Main", effects);
```
In the example above, we store the id "mainProfile" of the profile, which contains our struct with effects settings. The profile name can be anything you like, we named it "Main" - This is just for debug purposes.

Now for the effects to be applied to the renderer, we need to load the profile:

```gml
renderer.ProfileLoad(mainProfile);
```

If for example the game Menu has different effects than Level 1, you will create a different profile for both, and then load them properly.

> We recommend using just one `PPFX_Renderer()` and multiple `PPFX_Profile`s for full-screen effects, for better flexibility. (You can use as many as you want, though).

You can apply the `PPFX_Renderer` to layers using `.LayerApply()` in the Create Event (read documentation for more info).

</br>


## Final Creation Code <!-- {docsify-ignore} -->

So, our final creation code looks like this:

**Create Event:**  
```gml
// Create a post-processing renderer.
renderer = new PPFX_Renderer();

// Create profile with all effects.
var effects = [
    new FX_Colorize(true, c_orange),
    new FX_SineWave(true, 1),
];
mainProfile = new PPFX_Profile("Main", effects);

// Load profile, so all effects will be used.
renderer.ProfileLoad(mainProfile);
```
That's all you need to initialize everything.

> TIP: You can use a `switch(room)` and create and load the profiles in the **Room Start** event, for different rooms. Calling .ProfileLoad() funciton can override existing effects or add new ones, depending on your preference.

</br>


## Rendering & Drawing <!-- {docsify-ignore} -->

Post-Processing FX can be used to take a surface, add effects to it, and then draw the surface with effects on the screen.

To make the renderer apply effects to the input surface (the game screen, for example), all you need is to call the `.Render(inputSurface)` function/method.  

And to draw the already rendered surface with effects, just call `.DrawSelf()`.  
The `.Draw()` lets you define positions and sizes manually, this function works essentially the same as `draw_surface_stretched()`, but with the effects applied to the input surface.  

For fullscreen effects, we strongly recommend using the **Draw End** event to Render and **Post-Draw** event to Draw. If your game is single-player, you can call everything in the **Post-Draw** without any problems.  

You can use other events if you only want to use PPFX on your custom surface, without problems.  

Reference: <a href="https://manual-en.yoyogames.com/#t=The_Asset_Editors%2FObject_Properties%2FDraw_Events.htm">Draw Events</a>.

> If you applied the renderer to layers using the `.LayerApply()` function, it's not necessary to Render and Draw! as this function already causes the renderer to render and draw on the layer using layer scripts.

> We strongly recommend you activate the "Clear Display Buffer" option in the room settings, to prevent ghosting or blending issues.



## Final Draw Code <!-- {docsify-ignore} -->

The code below will render and draw the game screen with post-processing applied in fullscreen.  

**Post-Draw Event:**  
```gml
// Draw post-processing in full screen
renderer.Render(application_surface);
renderer.DrawSelf();
```

It uses the size of the referenced surface for internal rendering resolution (example: application_surface size).   

> It is very important to mention that the larger the surface/application_surface size, the more it will demand from the GPU.  

> It is worth remembering that `Post-Draw event is independent of cameras and viewports`.  
It is based on the size of the display buffer (*window* size), and is placed *before* the Draw GUI events. Which means the effects won't be applied on the game UI as GameMaker doesn't render the GUI event on the `application_surface`. (There are some ways around this and there is an example in the project just about that).  

> If your game is split-screen, since Draw End render for each viewport, you might want to use [surface_get_target()](https://manual.gamemaker.io/lts/en/GameMaker_Language/GML_Reference/Drawing/Surfaces/surface_get_target.htm) instead of `application_surface`, requiring a `PPFX_Renderer` for each viewport. If your game is single-player, this is not necessary.


If you use the `.Draw()` method, the following image will better illustrate how it works (basically a `draw_surface_stretched`):

![Optional Parameters](/../images/QuickStart_2-2.png)
*Game: Super Mario World 2 - Yoshi's Island. Copyright (C) 1995, Nintendo. Used for illustration only.*


## 5. Conclusion <!-- {docsify-ignore} -->

With that it is enough to apply the effects on the screen:

![PPFX](/../images/QuickStart_3.png)

*"SquarX" game. Copyright (C) 2023, Kazan Games.*

