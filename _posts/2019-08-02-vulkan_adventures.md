---
layout: post
title:  "Vulkan Adventures"
date:   2019-08-02 10:30
categories: rendering
---
After several months at work using DX12 I got the itch for doing some low-level graphics work in my personal time, so I started my foray into Vulkan a few weeks ago. 

I didn't really want to start from some beginner's tutorial so I did some hunting and found Arsen's Vulkan streams which are perfect for what I wanted.

He blazes through the code explaining a lot of things, but with my DX12 experience, they're quite brilliant because they get straight to the point and then I do my own digging into the spec.

This is the first time I've ever watched a coding stream and I'm really enjoying it. In my first ever development job in Malta, we did a lot of paired programming and this was ridiculuously useful. Seeing someone with 10+ years of experience coding (20+ in his case), making decisions and thinking out loud is very educational. One of the things that I like to hear the most about is not what they know about a topic but they learnt it - and seeing his problem solving approach is great.

Before I started it, I didn't really have any major reason for leaning towards Vulkan other than curiousity, but at this point I'm pretty hooked, because of...

# Validation Layers

The validation layers are tremendously helpful at telling you what might go wrong if you incorrectly setup certain structs. At some point I got a validation layer error that I wasn't understanding, so I ran the debugger through the validation layer itself to see what was triggering it. Being able to see inside them was brilliant and a huge leap from the opacity that comes with DX12, albeit DX12 has a fantastic debug layer.

# Mesh Shaders

Another major reason I was attracted to the streams was Mesh Shaders. They were a major reason for why I got my own RTX card, and having just finished the video where we get to do the mesh shading optimizations, I'm really looking forward to investigating more advanced uses for them. I've been fascinated with GPUs for a long time and I want to push my RTX to its limit with the amount of geometry it can process. Using the mesh shaders for doing extensive advanced culling is something I will definetly need when I try to process some of the massive scenes I intend to experiment with. The end goal is the Moana data set, we'll see how that fares, I'm going to need a big GPU for that!

The main drawback is the lack of tools for debugging mesh shaders but more on that in the next section.

I'll move the meshlet computation offline once I start working with bigger meshes because it can get quite expensive. 

# Debugging and Profiling Tools

It seems Vulkan is still not as well-catered as DX12 when it comes to debugging and profiling tools. I've had a lot of NSight crashes and it's the only decent free GPU profiling tool I can find. RenderDoc is robust but of no help when it comes to mesh shading. I'm quite disappointed with the debugging and profiling tools for Vulkan so far.

It seems DX12 has Mesh Shader debugging support in NSight but not Vulkan. With DX12 we used Pix extensively and it is loaded with a ton of useful features like performance counters, BVH visualization, gpu buffer readouts. NSight crashed immediately when I ran my Vulkan program :( I'll have to check my driver compatibility, hopefully it's something as trivial as that.

# volk

Extensions are an integral part of Vulkan, and volk comes a long way in solving that problem. It's useful but I did the mistake of not reading the fine print in the documentation. I was including it into my own project, but I did not replace the vulkan.h include with volk.h inside the libraries I was including in my own source, which resulted in me chasing a missing symbol related bug which appeared as a simple access violation. This swamped a whole Saturday and Sunday, resulting in a cancelled pub session and some time with Dark Souls. Learnt my lesson now!

# Vulkan Spec

The spec is super-detailed and well written but whoever thought that it should be in 1 web page deserves a world of pain! If it wasn't in 1 page I would consider it to being one of the finest pieces of literature, comparable to PBRT and Lord of the Rings :)

Reading the spec is very valuable, a lot of nuances are explained. The DX12 spec wasn't this detailed (although it's improved and I believe they've now released some really detailed documentation on GitHub).

# Verbosity

Brace yourself for a lot of typing. In terms of verbosity it seems to go OpenGL -> DX11 -----------> DX 12 ---> Vulkan. You have to write a lot of code to get things up and running. My previous experience with DX 12 prepared me for this so it was no shocker, but it can get exhausting.

# Predictability

Using the Vulkan C API becomes very intuitive once you get used to the nomenclature. The validation layer guides you when you do something wrong, the structs follow a similar structure and so you kind of get used to the verbosity.

# Final Thoughts

If you've never tried a low level graphics API, give Vulkan a go. It's been a great experience (sometimes aggravating but what's the fun in graphics programming if you sometimes don't want to scream) and learning how the GPU works is something every programmer should do.

I'm loading the Buddha obj and rendering it using a mesh shader pipeline, returning the surface normal as a colour in the fragment shader. I'm using ImGui to display some stats, such as the FPS, GPU time and a plot of the GPU time. I store 100 samples and plot them, using the min and max of the samples to get it displaying well, otherwise it's pretty much a straight light since the computation time doesn't fluctuate much.

I'd be lying if I told you that you should look at my code, but you shouldn't. It's a horrible mess that's been written in haste and hacky. But there is hope: I've also been watching Yan Chernikov's Game Engine series which is reaaaallllllyyyy good and once I do a forward pass and a deferred pass in Vulkan, I'll re-start the whole process and write th engine in a modular cleaner fashion. Unfortunately for now, all I have is a lot unclean code, but HEY IT WORKS SUE ME!

Here's a pretty image of where I'm at:

<p align="center">
<img src="{{ site.url }}/assets/posts/vulkan_adventures/vulkanMeshShaders.png" alt="Vulkan Mesh Shaders">
</p>

With that, I'm signing off. If you have any questions feel free to reach out to me on Twitter or LinkedIn or email, ciao!