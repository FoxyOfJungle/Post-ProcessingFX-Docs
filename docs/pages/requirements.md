
This page contains information on system requirements and compatibility of the Post-Processing FX.

## Compatibility <!-- {docsify-ignore} -->

Post-Processing FX is compatible with the following platforms currently supported by GameMaker:  

* Windows, Mac, Ubuntu, Android, iOS, WASM (GX.Games), Playstation 4 & 5, Xbox One, Xbox Series X | S and Nintendo Switch.

It uses only native GML code, in addition to shaders, without using any extra dependencies.  

Post-Processing FX works in split-screen (in many screens as needed) and with custom surfaces.  

> We tried to support HTML5 up to version 4.0.3, but it is unsustainable now. I recommend using WASM (Web Assembly/GX.Games export) instead.


## GameMaker Version <!-- {docsify-ignore} -->

**Post-Processing FX v5+** is compatible with the following versions of GameMaker:
* GameMaker v2024.6.2+ | Runtime v2024.6.1+

> PPFX v5 does not work on versions prior to this one, due to a major update related to depth buffer.

**Post-Processing FX v4** is compatible with the following versions of GameMaker:
* GameMaker v2023.2+ | Runtime v2023.2+

**Post-Processing FX v2.3** is compatible with the following versions of GameMaker:
* GameMaker v2022.11 LTS+ | Runtime v2022.11+

Do not use runtime v2023.6, because there is a shader bug that affects PPFX. v2023.8 is perfectly compatible, but currently 2024.13 is the recommended one.

> Please note that GameMaker v2022.2+ brings modifications to the GameMaker project structure, in addition to HDR support. So PPFX v3.0 above will not work on older versions.

GMRT is not yet supported - this may change in the future.


## Shader Language <!-- {docsify-ignore} -->

Post-Processing FX has all shaders written in GLSL ES language (OpenGL for Embedded Systems), which is compatible with all platforms.

<a href="https://en.wikipedia.org/wiki/OpenGL_ES" target="_blank">![Bulb + PPFX](/../images/GLSL_ES.png) </a>

## Runner <!-- {docsify-ignore} -->

Post-Processing FX is compatible with all architectures (x86, x64, ARM, etc).
YYC (YoYo Compiler) is also supported.

## System Requirements <!-- {docsify-ignore} -->

Post-Processing FX works well on low-end devices, where the performance of some effects (Blurs, Depth of Field, Sunshafts...) depends on their quality settings (these demand more performance).


## Other Assets <!-- {docsify-ignore} -->

Post-Processing FX has been successfully tested and is compatible with the following assets (graphics based):
* [Crystal Lighting Engine](https://foxyofjungle.itch.io/crystal-2d-lighting-engine) (FoxyOfJungle)
* Bulb Lighting (Juju Adams)
* Scribble (Juju Adams)
* Transitions Pro (FoxyOfJungle)

PPFX + Crystal:

![Bulb + Crystal](/../images/Crystal_with_PPFX.png)


PPFX + Bulb:

![Bulb + PPFX](/../images/Bulb_with_PPFX.png)

Probably shouldn't have a problem with most assets, as PPFX is completely independent. It is also compatible with all my other libraries.
