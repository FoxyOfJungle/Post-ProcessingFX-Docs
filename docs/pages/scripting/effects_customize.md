
## Controlling effects using code <!-- {docsify-ignore} -->

This guide shows you how to modify an effect's settings in-game, as well as enable or disable it.  
References: [Effects enum](./pages/scripting/effects_enum.md "Effects Enumerator List"), [Parameter list](./pages/scripting/param_list.md "Effects Parameter List")


### Changing a single effect parameter

To modify a parameter of some effect, just use the ppfx_effect_set_parameter() function.

Example: Modifying the Noise Grain intensity to 0.5:
```gml
ppfx_effect_set_parameter(ppfx_id, PP_EFFECT.NOISE_GRAIN, PP_NOISEGRAIN_INTENSITY, 0.5);
```
We specify the system id, the effect (which is an enum), the macro of the parameter we want to modify and finally the desired value.
<br><br>


### Changing several parameters

To modify many parameters of some effect, just use the ppfx_effect_set_parameters() function.
> Note that this function uses a internal loop to access the array, which makes it slower.

Example: Modifying some Bloom parameters:
```gml
ppfx_effect_set_parameters(ppfx_id, PP_EFFECT.BLOOM, [PP_BLOOM_INTENSITY, PP_BLOOM_THRESHOLD], [3, 0.7]);
```
We specify the system id, the effect (which is an enum), the array of macros of the parameters we want to modify, and finally the desired array of value. They must need to be in the same order they were written.
<br><br>


### Enabling and Disabling effects

When creating an effect, the first parameter indicates whether it will initially be active or not, for example:
```gml
...
new pp_bloom(true, ...),
...
```
However, if you want to enable or disable the effect in-game, just use the ppfx_effect_set_enable() function:
```gml
ppfx_effect_set_enable(pp_index, PP_EFFECT.BLOOM, false);
```
The example above disables the Bloom effect. Note that if you write "enabled" to -1, or leave it blank, this will toggle between enabled and disabled whenever the function is called.

> Important: Note that you can only disable or enable effects that are in the system (which have been loaded from a profile).
