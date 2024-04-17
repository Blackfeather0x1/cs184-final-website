---
layout: page
title: proposal
permalink: /
---

# Real-Time Hair Simulation using GPU shader/CUDA

Team Members: Lihao Zheng, Zheng Guo, Martin Guo, Alex Nguyen Ngoc


### Summary

This project will develop a straightforward real-time hair simulation using GPU shaders, providing a basic yet effective representation of hair physics suitable simple interactive applications. Building on ShaderToy examples, the goal is to create an accessible model for demonstrating hair dynamics and environmental interactions.

### Problem Description

Real-time hair simulation poses a challenge due to the fine, complex nature of hair dynamics and the intensive computational power required to animate numerous strands simultaneously. The main problem lies in achieving an interactive system that can provide both the desired realism and performance. Current solutions typically fall short in interactive contexts, such as gaming or virtual environments, where user input requires immediate visual feedback. Our approach will apply proven rendering methods to a GPU-based simulation framework, thus providing the computational power needed for the complex calculations involved in simulating hair in motion, ensuring both responsiveness and visual authenticity in real-time applications.


### Goals and Deliverables

<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/4n5AfHYST6E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> -->

The primary goal of this project is to develop a real-time, interactive hair simulation that utilizes GPU shaders for efficient computation and rendering. We aim to extend the principles from a basic cloth simulation (as demonstrated in the provided ShaderToy demo) to the more complex domain of hair physics.

**Baseline Deliverables:**

1. Hair Physics Simulation: Create a real-time hair simulation model on the GPU, starting from the ShaderToy cloth simulation example. The simulation will accurately model hair behavior, including strand-strand interaction and reaction to external forces such as gravity. An example of the type of simulation we aim to achieve can be seen in this demo video.
2. Environmental Interaction: Integrate environmental effects such as wind to showcase how simulated hair interacts with external forces, making the simulation more dynamic and realistic.
3. Object Interaction: Replace the sphere in the ShaderToy demo with a more complex object that interacts with hair, such as a comb. This will demonstrate the hair's response to collisions and forces exerted by everyday grooming tools.

**Aspirational Deliverables:**
1. Advanced Rendering Techniques: Implement advanced hair rendering techniques based on the "Hair rendering project", enhancing the visual realism of the simulation. This will include shaders for hair shading, self-shadowing, and possibly subsurface scattering for a more lifelike appearance.
2. Interactive Demo: Develop an interactive demo that can be shared online, allowing users to adjust parameters such as hair length, color, stiffness, and wind strength to see the effects in real time.

**Quality/Performance Measurement:**

To measure the quality and performance of the hair simulation, we will:
- Use frame rate as a performance metric, aiming for a smooth experience (e.g., 60 frames per second) to ensure the simulation runs in real time.
- Implement a benchmark comparing the simulation's performance with and without GPU acceleration to quantify the speedup achieved.
Evaluate the visual realism qualitatively through user feedback and comparison with high-quality reference images of real hair under similar environmental conditions.

**Evaluation Plan:**

The success of the project will be evaluated by the ability to:
Run the hair simulation interactively in real time on a standard modern GPU.
Render the hair with a high degree of visual fidelity as compared to the previous project's rendering techniques.
Introduce secondary elements (comb and wind) that interact with the hair in a believable manner, enhancing the overall simulation realism.
We acknowledge that precise target metrics may be challenging to define at this early stage; however, we will develop these as the project progresses, using the initial benchmarks as a baseline.


### Schedule

Week 1: search for papers and develop a physical model of hair.
Week 2: try to successfully render hair based on other’s work.
Week 3: realize basic physical simulation of hair.
Week 4: try to bring it on GPU and test the results.


### Resources



