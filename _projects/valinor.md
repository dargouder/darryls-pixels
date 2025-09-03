---
layout: page
title: Valinor
description: Vulkan Path Tracer
img: assets/img/valinor/valinor_logo.png
importance: 1
category: Personal
selected: true
# related_publications: true
images:
  compare: true
  slider: true
---

Valinor is a GPU-accelerated path tracer built with Vulkan, designed as both a learning platform and research tool. Originally created to explore Vulkan programming and real-time ray tracing techniques, it has evolved into a practical sandbox for my PhD research in computer graphics.

## Current status update as of July 2025
A lot of features are currently broken as I transition from cleaning up the Vulkan calls I have everywhere and hide all the graphics API calls behind a Render API, thus allowing me to support alternative APIs. I hope to continue this effort after the beginning of September.

# Key Features

## Mitsuba Scene Compatibility
Valinor can load Mitsuba scene files, enabling direct validation against Mitsuba's reference implementation to ensure rendering correctness.

<div class="row justify-content-center">
    <div class="col-sm-8">
<img-comparison-slider>
  {% include figure.liquid path="assets/img/valinor/valinor_cornell_box.png" class="img-fluid rounded z-depth-1" slot="first" %}
  {% include figure.liquid path="assets/img/valinor/mitsuba_cornell_box.png" class="img-fluid rounded z-depth-1" slot="second" %}
</img-comparison-slider>
    </div>
  </div>
<div class="caption">
    Valinor's rendition of Cornell Box vs Mitsuba's.
</div>


## Path Visualization Debugging

An integrated debugging system that visualizes light transport paths, invaluable for tracking down issues in light transport algorithms and appearance modeling.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/valinor/valinor_path_viz.png" title="example image" zoomable=true class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Specification

## Supported Geometry
- **OBJ Models** — Standard mesh loading
- **Rectangle** — Converted to 2 triangles
- **Cube** — Converted to 12 triangles

Spheres are converted into triangle meshes when used.

## Materials

- **Diffuse**
- **Smooth Conductor**
- **Smooth Dielectric**
- **Rough Conductor**
- **Rough Dielectric**

While spectral rendering is not supported yet, spectral data can be loaded and coverted to RGB, either from SPD files or from direct definition of the data in the scene description.

<swiper-container keyboard="true" navigation="true" pagination="true" pagination-clickable="true" pagination-dynamic-bullets="true" rewind="true">
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/valinor/valinor_smooth_metal_gold_milkyway.png" zoomable=true class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/valinor/valinor_rough_metal_copper_alpha_0.1.png" zoomable=true class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/valinor/valinor_smooth_glass_int_ior_1.5.png" zoomable=true class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/valinor/valinor_rough_glass_bromine_alpha_0.3.png" zoomable=true class="img-fluid rounded z-depth-1" %}</swiper-slide>
</swiper-container>

<div class="caption">
    (1) Smooth Conductor using a SPD for gold.
    (2) Rough Conductor using a SPD for copper and roughness=0.1.
    (3) Smooth Dielectric with an internal IOR of 1.5.
    (4) Rough Dielectric using bromine IOR and roughness=0.3.
</div>

## Lighting
- **Area Lights** — Emissive geometry for realistic soft shadows
- **Constant Environment Maps** — Uniform ambient lighting
- **Image-Based Lighting (IBL)** — HDR environment maps for realistic reflections

## Integrators
- **PtUni** — Brute-force random walk path tracer (no emission sampling)
- **PtNEE** — Path tracer with Next Event Estimation for direct lighting
- **PtMIS** — Multiple Importance Sampling combining NEE and BSDF sampling
- **PtRIS** — Resampled Importance Sampling for NEE at each vertex
- **PtWRS** — ReSTIR Direct Illumination at primary vertex, PtMIS for subsequent vertices ( Spatial and temporal reuse WIP )



# Gallery

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/valinor/valinor_glass_of_water.png" title="example image" zoomable=true class="img-fluid rounded z-depth-1" %}
    </div>

</div>

<div class="caption">
    The glass of water scene.
</div> 

## Miscellaneous Info

The renderer is named after a place described in the Silmarillion, the so-called "prequel" to Lord of the Rings. Valinor is the land of the Valar. The trees in the logo represent Telperion and Laurelin, the two trees which gave light to Valinor before they were destroyed by Morgoth and Ungoliant.
<!-- -->