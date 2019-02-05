---
title: "Solstice"
---

Solstice is my multiplatform physically-based renderer, written in C++. The objective of this project is for self-teaching goals, given my interest in ray tracing techniques.

The project has started and stopped many times, this current version has been ongoing for over a year and a half.

This version started from Peter Shirley's Ray Tracing in One Weekend books, and has continued with me changing chunks and pieces to improve it, such as the implementation of a Stratified Sampler, texture mapping, triangular meshes and most recently the addition of embree.

TBB is used for screen tile based parallel computation. and embree is used for accelerated ray intersection tests.

My objective is to have a working simple version of all the different parts of a renderer. Once I have a working example of each section, I'll start making more advanced implementations of each e.g. I'll write more sophisticated samplers, integrators, add so forth.

The blog posts I will write sometimes will have to do with Solstice. I usually try some new feature in a sandbox outside it and then move it into it.

### Current features

#### Shapes

* Triangles
* Spheres (currently unavailable due to embree)

#### Mesh import:

* OBJ

#### Cameras:

* Pinhole Camera

#### Integrators:

* Kajiya-style path tracing, with no explicit light sampling

#### Samplers:

* Random Sampler
* Stratified Sampler

#### Acceleration structure:

* Naive BVH (broken)
* embree-based BVH

#### Materials

* Lambert
* Glass (currently broken)
* Metal 

I've had to break a few things when I was on a big cleaning spree and adding in embree, hopefully I can manage to get things back to normal soon.

### Future work

There's a ton of work I intend to do. I'm still polishing some things and removing bugs, but these are some of the features I intend to implement:

* Next Event Estimation
* LDS Sampler
* CMJ and PCMJ Samplers
* Layered Materials
* Subsurface scattering materials
* BDPT/Metropolis/VCM integrators


### Images

Here's some images from what I've rendered so far, more images are incoming when I start implementing the cooler stuff

<p align="center">
<img src="{{ site.url }}/assets/solstice/bunny.png" alt="Cornell box with bunny - all diffuse">
</p>

The white pixels are making me sad

<p align="center">
<img src="{{ site.url }}/assets/solstice/sibenik.png" alt="Noisy sibenik with white pixels">
</p>