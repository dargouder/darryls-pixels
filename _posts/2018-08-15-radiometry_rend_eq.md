---
title:  "Measuring the Rendering Equation"
date:   2028-08-15 10:30
categories: mathematics, rendering, graphics
---
The root concept of all computer graphics is the propogation light, and how we describe and measure its propogration throughout a scene. In the case of physically based rendering, physics is our ground truth, therefore the quantities used to measure light and the ideas on how the light behaves under arbitrary conditions are an attractive and elegant way of simulating light transport. At the root all these physical ideas is radiometry.

This is a topic that I initially did not hold as much appreciation as I should have when I first started out in computer graphics, however I've come to realize that understanding radiometry well will lead to a more intuitive understanding of many different concepts in PBR. In this blog post, I'll use various sources to write at length about the different radiometric quantities, with examples and as many visual cues as possible.

Light is the visible part of the electromagnetic spectrum and therefor subject to all the ideas that we have regarding the behaviour of electromagnetic radiation. Most of us are familiar with the speed of light, approximately measured at 299,792,458 metres per second in a vacuum. This value is a commonly referred to as c and is part of a special group known as universal constants - a value which is constant both in the universe and in time. 

Light carries some form of energy, the unit of which we measure in Joules. We refer to this energy as radiant energy. The function that measures the amount of energy generated is in terms of the wavelength of the light. (write equation) states that the energy carried by a photon (which we can consider as 1 unit of light) is equal to the speed of light multiplied by Planck's constant, divided by the wavelength. Planck's constant is another one of these universal constants. (Talk a bit about Planck's constant).

Although energy is transmitted over an amount of time, when we'e rendering, we're measuring light at one point at a time - and to do so we need to take the differential energy per differential time, the resulting units being joules per second. This value is known as radiant flux, or power. It is the total amount of energy passing through a surface or region of space per unit time.

Now that we have radiant flux, we need to measure how much flux is arriving at or leaving a particular surface area, per 1 unit of time. This value is known as irradiance, if the area is receiving light, or radiant exitance if it is leaving it. The measurements of this will be W/m^2, we're taking the differential power per differential area at a point.

Before we measure radiance, we need to define the idea of a solid angle. We are accustomed to the idea of measuring angles about a point, in 2D, where a full revolution of 360 degrees results in a circle. An extension of this, whereby we define an angle with respect to a plane, results in a sphere covering all the possible angles about a point. We measure planar angles in radians - and the spherical angles in steradians, which is the total area subtended by the object projected on the unit sphere surrounding the point.

Radiance is the most important radiometric quantity. Whereas irradiance and radiant exitance gave us the differential power per differential area at a point p, the direction was not at all specified.

In this post we'll re-visit radiometry. I've been re-reading the radiometry chapters from Advanced Global Illumination and PBRT. And I had an 11 hour flight which I felt was the appropriate time to dive into the Veach thesis. For those unfamiliar with the Veach thesis, it's Eric Veach's PhD thesis titled Robust Monte Carlo Methods for Light Transport Simulation. It introduces the ideas of Multiple Importance Sampling, Bidirectional Path Tracing and Metropolis Light Transport, and is a must read for anyone interested in rendering and light transport. Dr. Veach does an excellent job of building up the foundational knowledge required to understamd thsese concepts.

I'll talk a bit about the radiometric quantities and borrow from many different sources to explain and visualize as best I can each one.

LOOK AT THE 2D GLOBAL ILLUMINATION PAPER BY JAROSZ.



The first quantity I'll describe is the radiant power which is the energy per unit time, measured in watts (joules per second). For example, it's used to describe how much energy is emitted by some light source. With Q(t) being the function that measures the photonic energy in an area D(t), the function to describe the rate of energy would be the differentiated function of this - dQ(t) / dt  The mathematical notation for the radiant power. In most cases however, we don't really worry about the energy changing over time, so we can usually leave out this parameter in our notation.

Irradiance is defined as the power per unit surface area, which would be Watts per meters squared. The irradiance is defined with respect to a point on a surface. The point is our unit surface area. The irradiance is ususally specific to the INCIDENT power per unit surface - if the light is leaving the surface then it is usually referred to as radiant exitance.

Radiance is is the ratio 



The beast that is the rendering equation attempts to measure the outputted light from a single point. The most common from most people are familiar with is the one below:

L(x, w_o) is attempting to measure the light outgoing from point x. Light outgoing, however, is a very ambiguous term. I'll attempt to explain some of the units related to the measuring of light and immediately tie them to the rendering equation.

