
## Creating Your Own Effects <!-- {docsify-ignore} -->

Just like external effects, Post-Processing FX allows you to create your own effects and add them to the renderer for later use.  

Before creating an effect, consider whether you really need it, as PPFX already has several effects.  

So, basically you only need a template, which is a constructor:

```gml
/// @desc My custom effect.
function FX_Effect(_enabled, _parameter1, _parameter2) : __PPF_FXSuperClass() constructor {
    effectName = "effect"; // used to reference the effect
	stackOrder = -50; // the rendering order
    isExternalEffect = true;

    // parameters
    enabled = _enabled;
    parameter1 = _parameter1;
    parameter2 = _parameter2;

    // shader uniforms
    static u_parameter1 = shader_get_uniform(shEffect, "u_parameter1");
    static u_parameter2 = shader_get_uniform(shEffect, "u_parameter2");

    // the drawing function
    static Draw = function(_renderer, _surfaceWidth, _surfaceHeight, _time) {
        if (!enabled) exit;
        
        // render
		_renderer.__stackSetSurface(_surfaceWidth, _surfaceHeight, effectName);
			draw_clear_alpha(c_black, 0);
			gpu_push_state();
			gpu_set_blendmode_ext(bm_one, bm_inv_src_alpha);
			
            // apply shader
			shader_set(shEffect);
            shader_set_uniform_f(u_parameter1, parameter1);
            shader_set_uniform_f(u_parameter2, parameter2);
            draw_surface_stretched(_renderer.__stackSurfaces[_renderer.__stackIndex-1], 0, 0, _surfaceWidth, _surfaceHeight);
			shader_reset();
			
			gpu_pop_state();
		surface_reset_target();
    }

    // for cleaning stuff from memory (no need to clean the stack surface)
    static Clean = function() {
        
    }

    // for profile exporting
    static ExportData = function() {
		return {
			name : instanceof(self),
			params : [enabled, parameter1, parameter2],
		};
	}

    // for Debug UI usage
    static GetEditorData = function(editor) {
        with(editor) {
            inspector_struct
			.AddElement(new __ppf_ui_menu("EFFECT", false, 2))
			.AddElement(new __ppf_ui_checkbox("Effect Enabled", 0, gui_ef_get_enabled(FX_EFFECT_EXT.EFFECT), function(checked) {return gui_ef_set_enabled(checked, FX_EFFECT_EXT.EFFECT);}))
			.AddElement(new __ppf_ui_slider("Intensity", 0, false, gui_ef_get_param_simple(FX_EFFECT_EXT.EFFECT, PP_EFFECT_INTENSITY), 0, 1, true, function(output) {gui_ef_set_param_simple(output, FX_EFFECT_EXT.EFFECT, PP_EFFECT_INTENSITY);}))
        }
        return 3; // number of elements
    }
}
```
The only mandatory function is `Draw()`, the others don't even need to exist (they are overriden).

> You need to set `isExternalEffect` to `true`.

> It is mandatory to inherit `__PPF_FXSuperClass`.

> Note: it is not necessary to pre-multiply the alpha in the shader.


### About `ExportData()`: 

**It's optional**. It is used when you use the export effects option in the Debug UI. 

Note that some data types are treated differently, and these must be added to an array as follows:
```gml
params : [enabled, ["texture", paperTexture], value],
```
The first value of the array is the `type`, and the second is the `data`.

Type list:

<ul class="a">
    <li>"vec2": An array of [x, y];</li>
    <li>"vec3" An array of [x, y, z];</li>
    <li>"color" A color array. The range is [1, 1, 1] -> white, which is like [255, 255, 255]; This will be interpreted by the shader;</li>
    <li>"texture" A pointer indicating that the parameter is a texture, which will return `undefined` (because textures can't be saved).</li>
</ul>

</br>

### About `GetEditorData()`:

**It's optional**. It is used to create the UI where you can edit the parameters in real time. 

You need to take a look at `__ppf_DebugUI` to know how the UI system works.

</br>

Then you can use the effect like any other. You can also base yourself on existing effects.
