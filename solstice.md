---
title: "Solstice"
---

Solstice is my multiplatform physically-based renderer, written in C++. The objective of this project is for self-teaching goals, given my interest in ray tracing techniques.



The renderer is self-contained on GitHub and all dependencies are part of the source.

embree for accelerated ray-intersection queries. There is also a self-authored BVH but the performance does not compete with that of embree and will be temporarily phased out as I will be focussing more on developing light transport related algorithms rather than ray-acceleration.

TBB is used for tile-based parallel rendering.

Ingo Wald's PBRTParser is used to parse PBRT scenes. For now, only the geometry, materials and lights are used from these files. Other settings are set in a JSON file.

On a recent branch I have started moving to a data-driven approach, which simplifies memory management, extensibility and porting to the GPU. This decision was spurred by my extensive work with GPUs this past year. With OptiX 7.0 now out and Ingo's OptiX course published on GitHub, the world is my oyster when it comes to GPU ray tracing!

Suggested material for such an approach is Richard Fabian's Data Oriented Design Pramming book, the MoonRay architecture paper and Mike Acton's wonderful rant on DoD at CppCon. Working with GPUs forces you to think in this way, so go try out CUDA, OptiX, Vulkan, DX12, whatever.


### Current features

#### Shapes

* Triangles
* Spheres (in progress in feature/SphereAreaLights branch)

#### Mesh import:

* OBJ (deprecated due to PBRT)
* PBRT

#### Cameras:

* Pinhole Camera

#### Integrators:

* Kajiya-style path tracing, with no explicit light sampling
* Path Tracing with Next Event Estimation

#### Samplers:

* Random Sampler
* Stratified Sampler

#### Acceleration structure:

* Naive BVH
* embree-based BVH

#### Materials

* Matte
* Glass
* Mirror 

### Future work

The renderer is an educational project and a constant work in progress. Although efforts are made to keep the code well commented and clean, there are sections where unfortunately the code is not aesthetically pleasing.

The current feature I'm looking to implement is Multiple Importance Sampling for NEE. Although a simple idea, I'm taking my time with the Veach thesis so it will be a while before I get to coding this up especially with many holidays coming(this was written as of July 2019). Having said that, I will be writing up a post on the matter for those that are not mathematically inclided to lower the bar of entry for this feature. I hope to write and release the post by mid-September 2019.

Once I'm done with MIS, I feel like the next 3 things I should implement are Bidirectional Path Tracing, Metropolis Light Transport and Photon Mapping. These 3 beasts will probably take me ages, but I'm optimistic and hope to be done by October. I ping-pong from project to project, from my real-time engine to this so we'll see what happens.

The 2 things I really want to implement are a fur shader and a lot of sampling methods, like Eric Heitz's recent Blue Noise Sampling paper, Pixar's recent Progressive Multijittered Sequences, Matt Pharr's work on that, Alex Keller's seminal work on QMC sampling (beware of patents on this), so I might divert onto these.


### Images

Here's some images from what I've rendered so far, more images are incoming when I start implementing the cooler stuff. 
<p align="center">
<img src="{{ site.url }}/assets/solstice/cornellBoxOriginal.png" alt="Classic Cornell Box.">
</p>

<p align="center">
<img src="{{ site.url }}/assets/solstice/cornellMirror.png" alt="Cornell box with a mirror material box.">
</p>
<p align="center">
<img src="{{ site.url }}/assets/solstice/cornellGlass.png" alt="Cornell box with a mirror box.">
</p>



The following render is of the Stanford Bunny inside the Cornell Box. If you look closely at the light it looks weird - that's because I was also allowing the light to light from the back. A bug from the olden days i.e. June 2018
<p align="center">
<img src="{{ site.url }}/assets/solstice/bunny.png" alt="Cornell box with bunny - all diffuse">
</p>

The following render is of the Sibenik Cathedral, lit only by the environment light visible from the windows. The white pixels were caused by PDF values that were 0, still in my learning phase here.

<p align="center">
<img src="{{ site.url }}/assets/solstice/sibenik.png" alt="Noisy sibenik with white pixels">
</p>
