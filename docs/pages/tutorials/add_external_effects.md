
## Adding External Effects <!-- {docsify-ignore} -->

Post-Processing FX allows you to add external effects (add-ons) that you can create and use yourself, or use what we produce.

![Layer Range](./images/ExternalEffect_Sketch.png)
<a href="https://foxyofjungle.itch.io/sketch-cartoon-shader" target="_blank">Sketch Shader</a> add-on.

### Import the .yymps Package

To be able to use the external effect, import the .yymps package into your game. Use the `Tools > Import Local Package` menu in GameMaker.


### Use it

Now just use the effect in the profile just like others:
```gml
profile = new PPFX_Profile("Game", [
    new FX_Sketch(true),
])
```


### Technical details:

To reference the effect, it is through the `effectName` of the effect. It's the same to use the macro that starts with `FF_`.  

And the parameters are the name of the variables that are defined in the effect.  

The parameters accompany the effect, and you can use them normally. Example: `PP_SKETCH_WHITE_AMOUNT`  

So:

```gml
renderer.SetEffectParameter(FF_SKETCH, PP_SKETCH_WHITE_AMOUNT, 1);
```
