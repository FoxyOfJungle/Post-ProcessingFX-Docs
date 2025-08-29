
## Controlling effects using code <!-- {docsify-ignore} -->

This guide shows you how to modify an effect's settings in-game, as well as enable or disable it.  
References: [Effects enum](./pages/scripting/effects_enum.md "Effects Enumerator List"), [Parameter list](./pages/scripting/param_list.md "Effects Parameter List")

</br>


### Changing a single effect parameter

To modify a parameter of some effect, just use the `.SetEffectParameter()` method.

Example: Modifying the Noise Grain intensity to 0.5:
```gml
renderer.SetEffectParameter(FF_NOISE_GRAIN, PP_NOISEGRAIN_INTENSITY, 0.5);
```
We specify the renderer, in this case: `renderer`, the effect name (which is a the `FF_` macro), the parameter name macro we want to modify and finally the desired value.  

<br><br>


### Changing several parameters

To modify many parameters of some effect, just use the `.SetEffectParameters()` function.

> Note that this function uses an internal loop to access the array, which makes it slower.

Example: Modifying some Bloom parameters:
```gml
renderer.SetEffectParameters(FF_BLOOM, [PP_BLOOM_INTENSITY, PP_BLOOM_THRESHOLD], [3, 0.7]);
```
We specify the renderer, the effect name, the array of parameters we want to modify, and finally the desired array of values. They must need to be in the same order they were written.  

<br><br>


### Enabling and Disabling effects

When creating an effect, the first parameter indicates whether it will initially be active or not, for example:
```gml
...
new FX_Bloom(true, ...),
...
```
However, if you want to enable or disable the effect in-game, just use the following method:
```gml
ppfx_id.SetEffectEnable(FF_BLOOM, false);
```
The example above disables the Bloom effect. Note that if you write `enabled` to -1, or leave it blank, this will toggle between enabled and disabled whenever the function is called.  

</br></br>

**IMPORTANT:** Note that you can only use these functions to disable, enable or modify effects that are already loaded in the renderer (which have been loaded from a profile).  

Since the effects are only referenced in the renderer, you can ALSO modify the effects directly as well:

```gml
bloom = new FX_Bloom(true);
bloom.knee = 1;

profile = new PPFX_Profile("Game", [bloom]);
renderer.ProfileLoad(profile);
```
