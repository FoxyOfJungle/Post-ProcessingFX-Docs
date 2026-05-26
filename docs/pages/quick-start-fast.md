
## Getting started with Post-Processing FX <!-- {docsify-ignore} -->

This page explains how to easily add post-processing effects to your game.
This quick-start guide requires a basic knowledge of GML and GameMaker. (No shader knowledge required).  

This is the FAST version of the original Getting Started guide. To learn how Post-Processing FX works, please follow the full tutorial.


## 1. Importing PPFX package <!-- {docsify-ignore} -->

See [installation](./pages/installation.md) guide.


## 2. Create a Manager object <!-- {docsify-ignore} -->

In order to display the effects of Post-Processing, you first need to create an object that will control post-processing in general, and add it to the room.  

That's all you need to draw the effects in fullscreen:

**Create Event:**
```gml
renderer = new PPFX_Renderer();
profile = new PPFX_Profile("Default", [
    new FX_Colorize(true, c_red),
    // All effects have optional parameters, so you can omit or customize them at any time.
]);
renderer.ProfileLoad(profile);
```

The profile contains the effects you want to render. You can create different profiles and load them whenever you want.  

**Draw End Event:**

```gml
renderer.Render(application_surface); // Apply effects from this surface
renderer.DrawSelf(); // Draw post-processing surface inside input surface (application_surface)
```

What the code does is basically: it gets the [application_surface](https://manual.gamemaker.io/lts/en/GameMaker_Language/GML_Reference/Drawing/Surfaces/application_surface.htm), applies effects to it, and draws on it. You can also do this to your own custom surface! :D

If your game is split-screen, since Draw End render for each viewport, you might want to use [surface_get_target()](https://manual.gamemaker.io/lts/en/GameMaker_Language/GML_Reference/Drawing/Surfaces/surface_get_target.htm) instead of `application_surface`, requiring a `PPFX_Renderer` for each viewport. If your game is single-player, this is not necessary.
