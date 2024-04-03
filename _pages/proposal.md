---
layout: page
title: proposal
permalink: /
---

# Jello, World!

Team Members: Noah Adhikari, Jedi Tsang, Johnny Lu, Allen Gu

![jello-jiggle](https://media.tenor.com/hiF1rq8rc2AAAAAd/gelatin-wiggle.gif){:style="display:block; margin-left: auto; margin-right: auto; width:50%;"}

### Summary

We hope to replicate jello through softbody simulation and track its interaction
with light during collisions and deformations. If time permits, we would also
like to include more interesting rigid-body collisions such as blades or
bullets.

### Problem Description

In this project, we strive to create an accurate and realistic simulation of
jello, a task that comes with many challenges in rendering and physical
simulation. 

**Why it's important:** Simulating jello-like substances extends to many
different materials that combine both non-rigid and rigid bodies. With our
approach, we would expand the capabilities of our simulation and possibly be
able to model many other materials as well.

**Challenges:** 
1. Rendering a translucent object to look like jello
2. Implementing a technique to simulate jello
3. Handling interactions and collisions with other rigid objects (or even
   fluids)
4. To fully simulate jello, we'd want to be capable of modeling how it breaks
   apart under high stress

**Ideas:** For rendering, we might extend our ray tracer from Project 3 or look into more optimized renderers. For simulation, we are looking into [material point methods (MPMs)](https://en.wikipedia.org/wiki/Material_point_method) and how to simulate multi-phase materials. Another approach might be to extend our mass-spring mesh models, though this wouldn't be able to simulate jello breaking. 

### Goals and Deliverables

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n5AfHYST6E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Main Deliverable: Our goal is to develop a jello simulation to produce videos of
jello-like substances interacting with each other and rigid objects. The
substances should realistically bounce and deform, and we also intend to support
translucent jello so the shading and refraction of light through the jello
should be realistic as well. We will test with different degrees of damping
which will affect how rigid or stiff the jello is. 

This should be achievable since it will involve many concepts we've learning in
class like spring systems, external forces and motion simulation, as well as
light refraction and other ray tracing concepts.

Intended goals:
1. (baseline) Opaque jello dropped onto rigid floor
2. (expected) Translucency
3. (expected) Interactions with other bodies (soft and/or rigid)
4. (stretch) Simulating more interesting interactions (e.g. knives, needles, or
   bullets)
5. (stretch) Optimizing simulation for faster rendering

A minimum for success is the baseline goal of a simulation of an opaque softbody
jello-like object dropped onto a rigid floor, with realistic bouncing and
deformation. Goals 2 and 3 are what we plan to achieve, making the simulation
more realistic by including translucent jello and collisions with non-floor
objects. If we are running ahead of schedule, our stretch goals are to simulate
more interesting physical interactions and improve the performance of our
simulations.

Quantifiable metrics are difficult to define for this project, but we have at
least one: seconds per frame for a given render. Quality will mostly be
judged by the realism of the simulation.

### Schedule

Week one:
* Basic rendering code with no physics
  * Opaque raytracing

Week two:
* Develop jello model
* Add physics simulation i.e. gravity
* Collison detection
* Milestone deliverables

Week three:
* Deformations
* Refraction

Week four:
* Stretch goals

### Resources

- [Jello cube animation case study](https://www.cs.cmu.edu/~barbic/jellocube_bw.pdf)
- [Paper describing point mass spring system to represent jello structure](https://www.cs.rpi.edu/~cutler/classes/advancedgraphics/S10/final_projects/wellington_boeckel.pdf)
- Material Point Methods
    - [A Moving Least Squares Material Point Method with Displacement Discontinuity and Two-Way Rigid Body Coupling](https://yuanming.taichi.graphics/publication/2018-mlsmpm/mls-mpm-cpic.pdf) - ([video](https://www.youtube.com/watch?v=9M18rc9-VWU))
    - [A Hybrid Material Point Method for Frictional Contact with Diverse Materials](https://www.math.ucla.edu/~jteran/papers/HGGWJT19.pdf) - ([video](https://www.youtube.com/watch?v=54YvCE8_7lM))
