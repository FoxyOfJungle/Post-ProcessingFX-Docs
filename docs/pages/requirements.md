
This page contains information on system requirements and compatibility of the Post-Processing FX.

## Compatibility <!-- {docsify-ignore} -->

Post-Processing FX is compatible with all platforms currently supported by GameMaker:
* Windows, Mac, Ubuntu, Android, iOS, HTML5, GX.Games, UWP, Playstation 4, Playstation 5, Xbox One, Xbox Series X | S and Nintendo Switch.

It uses only native GML code, in addition to shaders, without using any extra dependencies.

Post-Processing FX works in split-screen (in many screens as needed).


## GameMaker Version <!-- {docsify-ignore} -->

Post-Processing FX 4.0+ is compatible with the following versions of GameMaker:
* GameMaker v2023.2+ | Runtime v2023.2+

Post-Processing FX 2.3 is compatible with the following versions of GameMaker:
* GameMaker v2022.11 LTS+ | Runtime v2022.11+

Do not use runtime v2023.6, because there is a shader bug that affects PPFX. v2023.8 is perfectly compatible.

> Please note that GameMaker v2022.2+ brings modifications to the GameMaker project structure, in addition to HDR support. So PPFX v3.0 above will not work on older versions.


## Shader Language <!-- {docsify-ignore} -->

Post-Processing FX has all shaders written in GLSL ES language (OpenGL for Embedded Systems), which is compatible with all platforms.

<a href="https://en.wikipedia.org/wiki/OpenGL_ES" target="_blank">![Bulb + PPFX](/../images/GLSL_ES.png) </a>

## Runner <!-- {docsify-ignore} -->

Post-Processing FX is compatible with both x86 and x64 architectures.
YYC (YoYo Compiler) is also supported.

## System Requirements <!-- {docsify-ignore} -->

Post-Processing FX works well on low-end devices, where the performance of some effects (Gaussian Blur, Depth of Field, Sunshafts...) depends on their quality settings.

## Other Assets <!-- {docsify-ignore} -->

Post-Processing FX has been successfully tested and is compatible with the following assets (graphics based):
* Bulb Lighting (Juju Adams)
* Scribble (Juju Adams)
* Transitions Pro (FoxyOfJungle)

![Bulb + PPFX](/../images/Bulb_with_PPFX.png)

Probably shouldn't have a problem with most assets, as PPFX is completely independent.
