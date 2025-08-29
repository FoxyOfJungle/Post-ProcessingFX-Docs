
## Using multiple rendering <!-- {docsify-ignore} -->

This guide shows you how to use multiple rendering (like a stack).

Imagine stacks as layers on top of another. That's basically it, use the final image from a post-processing renderer as input to another renderer.

![Layer Range](/./images/MultipleStacks_0.png)

</br>

## Creating two different PPFX_Renderer <!-- {docsify-ignore} -->

We will use the first renderer as the final image (input), for the second renderer to draw over it.

### Stack 1

Create Event:
```gml
// Create the first post-processing renderer
ppfxStack1 = new PPFX_Renderer();

// Create a profile with effects, for the renderer
var _profile1 = new PPFX_Profile("Cool Effect", [
    new FX_Vignette(true),
]);

// Load profile
ppfxStack1.ProfileLoad(_profile1);
```

For better performance, disable the first stack auto-draw with the `.SetDrawEnable()` function:

```gml
ppfxStack1.SetDrawEnable(false);
```



### Stack 2

Create Event:
```gml
// Create the second post-processing renderer
ppfxStack2 = new PPFX_Renderer();

// Create a profile with effects, for the renderer
var _profile2 = new PPFX_Profile("Cool Effect", [
    new FX_VHS(true),
]);

// Load profile
ppfxStack2.ProfileLoad(_profile2);
```


### Draw Everything

Now that we've created the stacks, we need to draw them.
In `Post-Draw` (recommended) or `Draw GUI Begin` event:

```gml
ppfxStack1.DrawInFullscreen(application_surface);
ppfxStack2.DrawInFullscreen(ppfxStack1.GetRenderSurface());
```

Note that the last stack (ppfxStack2) is what is being displayed in the the screen. It gets the surface of the first stack (ppfxStack1), which contains the vignette.


## Result <!-- {docsify-ignore} -->

So, we draw the VHS effect on top of the Vignette:

![Layer Range](/./images/MultipleStacks_1.gif)

> NOTE: You can change the order of effects using `.SetOrder()` for each effect, if this is your goal.

> This is just an example. We do not recommend using many renderers at the same time as it will affect performance. If you want multiple effects, use them all in the same profile.

> Note that you can use as many effects as you like in both profiles.
