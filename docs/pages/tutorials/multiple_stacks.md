
## Using multiple rendering <!-- {docsify-ignore} -->

This guide shows you how to use multiple rendering (like a stack system).

Imagine stacks as layers on top of another. That's basically it, use the final image from a post-processing renderer as input to another system.

![Layer Range](/./images/MultipleStacks_0.png)

</br>

## Creating two different systems <!-- {docsify-ignore} -->

We will use the first renderer as the final image (input), for the second renderer to draw over it.

### Stack 1

Create Event:
```gml
// Create the first post-processing system
ppfx_stack1 = new PPFX_System();

// Create a profile with effects, for the system
var _profile1 = new PPFX_Profile("Cool Effect", [
    new FX_Vignette(true),
]);

// Load profile
ppfx_stack1.ProfileLoad(_profile1);
```

For better performance, disable the first stack auto-draw with the `.SetDrawEnable()` function:

```gml
ppfx_stack1.SetDrawEnable(false);
```



### Stack 2

Create Event:
```gml
// Create the second post-processing system
ppfx_stack2 = new PPFX_System();

// Create a profile with effects, for the system
var _profile2 = new PPFX_Profile("Cool Effect", [
    new FX_VHS(true),
]);

// Load profile
ppfx_stack2.ProfileLoad(_profile2);
```


### Draw Everything

Now that we've created the stacks, we need to draw them.
In `Post-Draw` or `Draw GUI Begin` event:

```gml
ppfx_stack1.DrawInFullscreen(application_surface);
ppfx_stack2.DrawInFullscreen(ppfx_stack1.GetRenderSurface());
```

Note that the last stack (ppfx_stack2) is what is being displayed in the the screen. It gets the surface of the first stack (ppfx_stack1), which contains the vignette.


## Result <!-- {docsify-ignore} -->

So, we draw the VHS effect on top of the Vignette:

![Layer Range](/./images/MultipleStacks_1.gif)

> NOTE: You can change the order of effects using .SetOrder(), if this is your goal.

> This is just an example. We do not recommend using many stacks/systems at the same time as it will affect performance. If you want multiple effects, use them all in the same profile.

> Note that you can use as many effects as you like in both profiles.
