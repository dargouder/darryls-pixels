---
layout: page
title: Solstice (deprecated)
description: C++ Path Tracer
img: assets/img/solstice/cornellBoxOriginal.jpg
importance: 2
category: Personal
---
# Notice
The description below is quite old and pulled from my previous website. I stopped working on this renderer in 2020 and had not updated the text nor the images since way before that, therefore it does not reflect the current (dead) state of it. Nevertheless, for nostalgic reasons, I have kept the original page.
# Overview

Solstice is my multiplatform physically-based renderer, written in C++. The objective of this project is for self-teaching goals, given my interest in ray tracing techniques.

The renderer is self-contained on GitHub and all dependencies are part of the source.

embree for accelerated ray-intersection queries. There is also a self-authored BVH but the performance does not compete with that of embree and will be temporarily phased out as I will be focussing more on developing light transport related algorithms rather than ray-acceleration.

TBB is used for tile-based parallel rendering.

Ingo Wald's PBRTParser is used to parse PBRT scenes. For now, only the geometry, materials and lights are used from these files. Other settings are set in a JSON file.


### Current features

#### Shapes

* Triangles
* Spheres
* Cubes

#### Mesh import:

* OBJ (deprecated due to PBRT)
* PBRT

#### Cameras:

* Pinhole Camera

#### Integrators:

* Kajiya-style path tracing, with no explicit light sampling
* Path Tracing with Next Event Estimation
* Path Tracing with Next Event Estimation (using MIS)

#### Samplers:

* Random Sampler
* Stratified Sampler

#### Acceleration structure:

* Naive BVH
* embree's BVH

#### Materials

* Matte
* Glass
* Mirror 

### Images

Here's some images from what I've rendered so far, more images are incoming when I start implementing the cooler stuff.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/solstice/cornellBoxOriginal.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/solstice/cornellMirror.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/solstice/cornellGlass.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


The following render is of the Stanford Bunny inside the Cornell Box. If you look closely at the light it looks weird - that's because I was also allowing the light to light from the back. A bug from the olden days i.e. June 2018.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/solstice/bunny.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The following render is of the Sibenik Cathedral, lit only by the environment light visible from the windows. The white pixels were caused by PDF values that were 0, still in my learning phase here.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/solstice/sibenik.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
