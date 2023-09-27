
## Creating Your Own Effects <!-- {docsify-ignore} -->

Just like external effects, Post-Processing FX allows you to create your own effects and add them to the system for later use.

You only need a template, which is a constructor:

```gml
/// @desc My custom effect.
function FX_Effect(enabled, parameter1, parameter2) : __ppf_fx_super_class() constructor {
    effect_name = "effect";
	stack_order = -50; // the rendering order

    // parameters
    settings = {
		enabled : enabled,
		parameter1 : parameter1,
		parameter2 : parameter2,
    };

    // shader uniforms
    uni_parameter1 = shader_get_uniform(sh_effect, "u_parameter1");
    uni_parameter2 = shader_get_uniform(sh_effect, "u_parameter2");

    // the drawing function
    static Draw = function(renderer, surface_width, surface_height, time, global_intensity) {
        if (!settings.enabled) exit;

        // create stack surface
		renderer.__create_stack_surface(surface_width, surface_height, effect_name);

        // render
		surface_set_target(renderer.__stack_surface[renderer.__stack_index]);
			draw_clear_alpha(c_black, 0);
			gpu_push_state();
			gpu_set_blendmode_ext(bm_one, bm_inv_src_alpha);
			
            // apply shader
			shader_set(sh_effect);
            shader_set_uniform_f(uni_parameter1, settings.parameter1);
            shader_set_uniform_f(uni_parameter2, settings.parameter2);
            draw_surface_stretched(renderer.__stack_surface[renderer.__stack_index-1], 0, 0, surface_width, surface_height);
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
			params : [settings.enabled, settings.parameter1, settings.parameter2],
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
The only mandatory function is Draw(), the others don't even need to exist (they are overriden).

> It is mandatory to inherit __ppf_fx_super_class.

> Note: it is not necessary to pre-multiply the alpha in the shader.


### About `ExportData()`: 

**It's optional**. It is used when you use the export effects option in the Debug UI. 

Note that some data types are treated differently, and these must be added to an array as follows:
```gml
params : [settings.enabled, ["texture", settings.paper_tex], settings.value],
```
The first value of the array is the `type`, and the second is the `data`.

Type list:

<ul class="a">
    <li>"vec2": An array of [x, y];</li>
    <li>"vec3" An array of [x, y, z];</li>
    <li>"color" A color array. The range is [1, 1, 1] -> white, which is like [255, 255, 255]; This will be interpreted by the shader;</li>
    <li>"texture" A pointer indicating that the parameter is a texture, which will return undefined.</li>
</ul>

</br>

### About `GetEditorData()`:

**It's optional**. It is used to create the UI where you can edit the parameters in real time. 

You need to take a look at `__ppf_debug_ui` to know how the UI system works.

</br>

Then you can use the effect like any other.
