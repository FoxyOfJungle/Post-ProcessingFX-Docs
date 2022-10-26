
<!-- <img class="w-8/8 flex justify-center items-center mx-auto" src="./images/PostProcessingFX_Logo.png" alt="Post-Processing FX a" /> -->
![Post-Processing FX](./images/PostProcessingFX_Logo.png ":no-zoom")

<center>The most complete Post-Processing solution for GameMaker.</center>
<center>Developed and maintained by FoxyOfJungle.</center>

<p align="center">
    Get it on: <a href="https://foxyofjungle.itch.io/post-processing-fx">Itch.io</a> | <a href="https://marketplace.yoyogames.com/assets/10916/post-processing-fx">Marketplace</a>
</p>

## Introduction

Post-processing is a generic term for a full-screen image processing effect that occurs after the overall rendering of the game. Post-processing can drastically improve the visuals of your product with little setup time.

You can use post-processing effects to simulate physical camera and film properties.

For information about requirements and compatibility, see section [Requirements](./pages/requirements.md).

The image below demonstrate a scene without and with post-processing:

![Before/After](./images/PPFX_Comparison.png)


## Features

Post-Processing FX is a modern post-processing system, focused on performance quality and stability, developed with the aim of drastically improving the visuals of games.

<ul class="a">
    <li>Cross-platform (including Android and HTML5)</li>
    <li>Performance. PPFX is super performant overall (CPU and GPU), it has been tested on mobile devices and has performed well in the tests</li>
    <li>Extreme easy to use, add to an existing game and highly customizable, whether you're new to GML, or an experienced developer</li>
    <li>A lot of ready-made customizable effects</li>
    <li>Profiles. A collection of effects that can be loaded and modified on the fly</li>
    <li>Layers. Apply effects to one or more layers in the room</li>
    <li>Areas. Use a sprite as a mask and draw it with the texture of a system, useful for creating glass UI effects and more</li>
    <li>Shockwaves. Easily add shockwaves to your game with few functions</li>
    <li>Split-screen support</li>
    <li>Modular. Create systems and return their IDs, which are used by other functions</li>
    <li>Resolution-independent</li>
    <li>All effects can work together or individually</li>
</ul>

## FAQ

### What platforms does Post-Processing FX support?
Everything. Post-processing has been carefully designed to be compatible with all platforms. Please note that there may be some unknown bugs (maybe shader-related) on platforms that we don't test often, so always report bugs if you find one, so we can take a look. The project uses pure GML simple logic with no dependency, so it shouldn't be a problem.


### Is it difficult to import into an existing project?
No. There are several examples in the project showing how simple this is to do. All you need to do is import the .yymps file into your game and use the appropriate functions to apply the effect to the screen. Please read this documentation, this way it will be even simpler to implement. If in doubt, ask on our Discord server.


### What is the shader performance?
All effects are super lightweight (includes Bloom), run smoothly on Android and on weak computers (tested in an onboard 128 MB VRAM GPU and GTX 1050 TI 4096 MB VRAM).
Please note that some shaders such as Gaussian Blur, Motion Blur and Radial Blur demand more from the GPU.


### How do I change shader performance?
Most shaders are lightweight in nature, but you can change the quality of some effects (Blur, Sunshafts...) within the fragment shader, via the ITERATIONS variable.  
See the documentation for more details.


### How can I contribute?

You can make suggestions on our <a href="https://discord.gg/4azKEdMNze" target="_blank">Discord</a>. If you want to support our work, you can do so through <a href="https://www.patreon.com/foxyofjungle" target="_blank">Patreon</a> or by <a href="https://foxyofjungle.itch.io/" target="_blank">purchasing other assets</a>. We kindly thank you! :)


## Help

If you have any questions about how to implement Post-Processing FX in your project, or want to report a bug, <a href="https://discord.gg/4azKEdMNze">join our Discord group by clicking here</a>. Be welcome!
