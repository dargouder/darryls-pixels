---
layout: page
title: Menegroth (deprecated)
description: Rust Path Tracer
img: assets/img/menegroth/cornell_box_spectral.png
importance: 2
category: Personal
---

# Notice

This project has been dead since March 2022. I finished implementing spectral rendering support and hero-wavelength sampling, then called it a day. Rust was a fun programming language, but weighing things out career-wise, it didn't make much sense to sink a lot of time in it. The industry I'm in will most probably remain more oriented towards C and C++. Nevertheless, I had quite a bit of fun working in Rust, and would recommend you try it out. Unfortunately I seem to have only 1 image saved, and getting thre renderer to compile again is not something I have the time for. But for historical purposes I'll keep the project page here.

## Overview

Menegroth is a physically-based renderer written primarily in Rust. The renderer supports both RGB and spectral rendering modes. It supports exporting of its functions to ecvxr, which can be used to write Jupyer notebooks in rust.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/menegroth/cornell_box_spectral.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Key Features

### Rendering Modes
- **RGB Rendering**
- **Spectral Rendering with hero-wavelength sampling**

### Integrators

- **Unidirectional Path Tracing**
- **Next Event Estimation (NEE)**
- **Multiple Importance Sampling (MIS)**


### Materials (BxDFs)

- **Diffuse Lambertian Reflection**
- **Smooth Conductor**
- **Smooth Dielectric**

### Geometric Primitives

- **Triangle Meshes**: Support for OBJ file loading and complex geometry
- **Spheres**: Analytical sphere primitives for perfect curved surfaces  
- **Cubes**: Basic box primitives

### Lighting & Environment

-- **Diffuse Area Lights**
-- **Constant Environment Maps**

### Texture Support

- **Constant Textures**: Solid color materials
- **Image Textures**: Bitmap texture mapping
- **Checker Textures**: Procedural checker patterns

## Performance & Validation

### Statistical Testing
Valinor employs rigorous statistical validation using Pearson Chi-Square Goodness of Fit tests to ensure sampling functions match their probability density functions. These are Rust ports of the statistical tests implemented in mitsuba.

### Parallelization
Built with Rayon for efficient CPU parallelization across multiple cores.

## User Interfaces

- **Command Line Interface**
- **Interactive GUI**: Built with Vulkan and ImGui for real-time preview and parameter adjustment.