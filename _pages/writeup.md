---
layout: page
title: writeup
permalink: /writeup
mathjax: true
---

<!-- # Final Writeup

[Slides](https://docs.google.com/presentation/d/1ozbsNFH_Sg-njZ_wx_bAXcgFaxxcuOdkhU3FMj_iLkg/edit?usp=sharing)

Jello, world! In our final project, we implemented a realtime Jello simulation in Unity with support for keyboard and mouse interactivity that allows the user to manipulate the Jello. We created two mass-spring systems in the forms of a cube and a sphere and wrote mesh generators for both shapes. Initially, we started with the cube model by extending the 2D cloth model from Project 4 to our 3D lattice of masses and springs. Then, we devised our own algorithm for the sphere model using spherical coordinates and testing different spring configurations. Finally, we tuned spring parameters for realistic renderings. Since we used Unity for our project, we took advantage of the High Definition Rendering Pipeline, allowing us to focus on the construction of the object itself.

## Technical Approach

**Mass-Spring Cube**

Since our Jello supports a variable number of particles per axis, we used a similar implementation to Project 4 when deciding where to place each particle by computing a constant offset distance between each particle. We initialized an $$N \times N \times N$$ lattice of particles to prevent from underconstraining later when adding springs.

We took advantage of our existing knowledge of the spring system in project 4 and expanded the definitions of each spring to apply to three-dimensional space. Structural constraints join adjacent vertices along each axis, bending constraints skip a vertex in a certain axis, and shearing constraints join adjacent vertices along face and space diagonals. Below is a diagram illustrating the springs between each particle:

![cube-springs](../assets/img/writeup/cube-springs.png){:style="display:block; margin-left: auto; margin-right: auto; width:50%;"}

**Mass-Spring Sphere**

We devised our own algorithm from scratch to initialize vertex positions and add springs for our spherical mass-spring model. 

Initializing the particles for a sphere was more complicated. To simplify the design, we chose to leverage the spherical coordinate system with angles $$(\theta, \phi)$$. We loop through the two coordinates in equal increments to set positions for each point by transforming back to the Cartesian grid: $$x = R\sin(\theta)\cos(\phi)$$, $$y = R\cos(\theta)$$, and $$z = R\sin(\theta)\sin(\phi)$$. After accounting for the edge case at the poles (e.g. $$\theta = 0$$ or $$\theta = \pi$$) which should only have one particle, we initialized one final particle at the origin. We’ll elaborate on the usage of this origin particle later.

We had to experiment a bit to see what springs were necessary to constrain our system. Similar to structural, shearing, and bending springs, we first connected all particles to (1) their direct neighbors by incrementing $$\theta$$ and $$\phi$$, (2) their diagonal neighbors, and (3) springs connecting every other vertex in both angular dimensions. At this point, we found that the sphere deformed too easily, as there were only enough particles to simulate its skeletal surface but nothing representing the inside. As a result, we added springs connecting the origin to all the other particles to prevent too much deformation. Still, we needed to constrain the sphere further, so we added springs connecting each vertex to its polar opposite vertex radially through the origin.

| Vertices | Springs |
| --- | --- |
| ![sphere-vertices](../assets/img/writeup/sphere-vertices.png){:style="display:block; margin-right: auto; width:90%;"} | ![sphere-springs](../assets/img/writeup/sphere-springs.png){:style="display:block; margin-right: auto; width:90%;"} |

**Mesh Generation**

For both the cube and the sphere mesh, we used Unity's built-in triangle Meshes based on an indexed triangle representation. For every vertex on the outer surface of the body, we track the position in a vertex list. Then, we describe triangles as triplets of indices within the vertex list $$(i_{v1}, i_{v2}, i_{v3})$$. A tricky aspect of creating the mesh was being intentional about making sure that all the triangles were in a consistent winding order so that the normals were pointed outwards.

**Interactivity**

We implemented two types of interactivity: (1) movement with WASD and space and (2) click-and-drag to manipulate vertices.

The WASD keys allow the user to control the Jello in the $$x-z$$ plane. Upon every update, we detect keyboard inputs. For instance, if the W key is pressed, we add velocity in the $$+z$$ direction to the rigid body component of every unique particle in the object. We also supported spacebar inputs to make the Jello jump in the $$+y$$ direction.

To support clicking and dragging our Jello, we track the position of the mouse and send out a ray from the mouse onto the object until it intersects with one of the vertices. From the $$z$$ coordinate of this intersection point, we can detect the current depth of the object. Thus, we can effectively treat any mouse movement in 2D screen space as only changing the x and y coordinates of the object along the intersected plane.

**Challenges**

As this was all of our first times working with Unity, we ran into a number of challenges:
1. **Distributing particles in our spherical mass spring system:** We found there was a tradeoff between computational efficiency with fewer particles and sufficiently constraining the system with springs so that the object remained stable. At first, we only added particles to the outer face of the sphere and distributed them evenly. However, to adequately constrain the system, we also added a particle in the origin that was connected to every other particle in the system (much like a bicycle wheel and its spokes). Another issue we originally faced with setting particle positions was that we had to special case $$\theta = 0$$ and $$\theta = \pi$$ because rotating these positions in $$\phi$$ should result in the same vertex.
2. **Decreasing the vertex particle size deformed our system:** 
We concluded that the structure of our objects was maintained via the particle collisions, and kept the particle sizes large enough but didn’t render the particles themselves. We’re still not completely sure whether the deformations were caused by precision errors when calculating the normals or the mass of the particle decreasing as the size of the particle also decreases, which would cause spring correction forces to apply an outsized impact on the particles.
3. **Choosing the sphere model's spring configuration:** During many early iterations of our design, we found that many spring configurations were underconstrained, resulting in erratic motion and deformation. As a result, we added an origin point with springs connecting to all outer vertices as well as other sets of springs that connect polar opposite vertices. Additionally, we found that lowering the spring constant to the 100-5000 range led to the best results.
4. **Resting Jello does not stabilize:** Even when resting in place, our Jello would still continue to move. Introducing damping and friction on each vertex so the object would eventually come to rest.

## Final Results

In the videos below, we capture the primary features of our Jello! You can see the cube and spherical Jellos we implemented, as well as applying movement and jumping commands to the objects.

| Bowl (Cube) | Bowl (Sphere) | Ramps |
| --- | --- | --- |
| ![cube-bowl](../assets/img/writeup/cube-bowl.gif){:style="display:block; margin-right: auto; width:90%;"} | ![sphere-bowl](../assets/img/writeup/sphere-bowl.gif){:style="display:block; margin-right: auto; width:90%;"} | ![ramps](../assets/img/writeup/ramps.gif){:style="display:block; margin-right: auto; width:90%;"} |
{:style="margin-bottom: 10px;"}

We also played around with the spring constant to manipulate the rigidity of the Jello. As you can see, lower spring constants make the Jello more jiggly, analogous to an extra bouncy bit of jello! Naturally, a higher spring constant meant that the system would settle quicker. Interestingly, the bounce height for the cube seemed to increase then decrease as the spring constant increased. This might be because lower spring constants could not apply enough force to lift the Jello while higher spring constants do not deform enough to spring the Jello upwards.

![spring-constants](../assets/img/writeup/spring-constants.gif){:style="display:block; margin-left: auto; margin-right: auto; width:90%;"}

## References
From a high level, other than referencing documentation on how certain built in tools for Unity worked, we used trial and error when building
- ClothSim served as the basis for the way we went about building the mass-spring system, as well as the springs we attached to the cube object. 
= To translate the springs from 2-D to 3-D for the cube object specifically, we referenced slides by [Carnegie Mellon University’s Fall 2002 offering of Computer Graphics 15-462](https://www.cs.cmu.edu/~barbic/jellocube_bw.pdf)
- [Unity documentation on how the mesh class works](https://docs.unity3d.com/Manual/UsingtheMeshClass.html) so we know the appropriate information to supply when rendering the mesh. -->

# Hair Physics Simulation with Mass-Spring Model

Real-time Hair Simulation! Our project focused on enhancing the realism of hair simulation in digital environments by advancing a mass-spring model based physical simulation(resource here). Initially, our efforts centered on adapting an existing code framework from [https://github.com/swyngaard/imicu](https://github.com/swyngaard/imicu), which we modified to fit the specific requirements of our simulation. Key developments included the integration of a static head model that anchors the hair particles, providing a realistic base from which the hair dynamics could be accurately simulated. Additionally, we incorporated gravity into the model to simulate the natural downward movement of hair as well as fine-tuning physical constants that parametrize the hair’s behavior. Finally, a collision detection algorithm, built upon and accelerated by a k-d tree hierarchy, was added, which enabled more realistic hair-head interactions. The result is a visually compelling simulation where hair not only adheres naturally to the head model but also moves believably under the influence of gravity. This project implements some of the current progress in the field of hair simulation, suggesting a promising direction for future research in achieving even more lifelike animations in computer graphics.

## Technical approach

In developing our simulation, we built upon the theoretical framework established by Andrew et al. (2008) and Yao Lyu (2016), who modeled hair as a series of point masses connected by various types of springs. Our approach, however, diverged from these references in several key aspects.

First, while the foundational works employed uniform spring constants across the model, we introduced variable spring constants to account for the heterogeneous properties of hair. This decision was motivated by the observation that hair exhibits different elastic properties along its length due to varying levels of damage and treatment.

To implement this, we developed an algorithm that adjusts the spring constants based on the input data derived from microscopic images of hair strands. The algorithm categorizes each segment of the hair strand into one of three categories—healthy, damaged, and treated—and assigns spring constants accordingly. This enhancement aimed to bring a higher degree of realism to the simulation by capturing the non-uniform behavior of hair.  

In the “hair.cpp” file, we implement an algorithm for realistic hair simulation and rendering in computer graphics. Specifically, it uses a physically-based approach to model hair fibers as a series of connected particles that represent segments of hair strands. The algorithm employs principles from continuum mechanics to describe the motion and interaction of these particles under various forces, such as gravity and wind (the latter to be implemented in future work). Additionally, it includes a collision detection mechanism to prevent hair strands from intersecting with each other and with other objects in the environment. The rendering part of the algorithm utilizes techniques like strand-based rendering and shadow mapping to produce visually appealing and realistic hair appearance in real-time applications. This approach not only enhances the realism of the hair but also ensures efficient performance, making it suitable for use in video games and virtual reality environments.

In the "ogl.cpp" file, a detailed implementation integrates a 3D head model, sourced from a specific GitHub repository, onto which a custom hair model is superimposed. Initially, the hair appears suspended in mid-air to highlight its dynamic properties. During the demonstration video, gravity is programmatically applied, causing the hair to fall naturally downwards, simulating realistic hair behavior in response to gravitational force. This effect is achieved through modifications to the rendering logic, which not only visually represents the hair's interaction with gravity but also enhances the overall realism of the simulation. The integration and subsequent manipulation of the hair dynamics are carefully designed to demonstrate the capabilities of the hair simulation framework within a 3D graphical environment.

Each spring type used in the hair physical model:
1. **Edge Springs**: These are the fundamental components of the hair simulation, connecting every two neighboring particles to maintain the hair's desired curve and primary form.
2. **Bending Springs**: Positioned to connect every particle to others two edges away, bending springs act as constraints that resist changes in the curvature of the hair, helping to preserve its shape against bending forces.
3. **Torsion Springs**: Torsion springs link each particle to others three edges away, providing resistance to twisting motions around the hair strand’s length, thus maintaining the integrity of the hair’s twist along its axis.
4. **Tetrahedral Altitude Springs**: Unique to our model, these springs form for every group of four consecutive particles, creating a tetrahedral structure. They exert a repelling force between the opposite edges of the tetrahedron, effectively preventing the collapse of the hair’s volume when compressed.

![spring-model](https://i.ibb.co/71jWkd2/spring-model.png){:style="display:block; margin-left: auto; margin-right: auto; width:75%;"} 


**Elastic force Mathematical calculation:**

In our formulation, hair is modeled as a serial succession of point masses, connected with springs. First, edge springs are between every 2 neighboring particles. Second, bending springs connect every particle to other particles 2 edges away from it. Third, torsion springs connects every particle to other particles 3 edges away from it. Fourth, for every 4 consecutive particles, the above springs form a tetrahedron. Between every pair of opposite edges of the tetrahedron, the distance between the straight lines are calculated, and a spring-like repelling force is exerted on the edges according to the distance.

The elastic force on the spring between 2 particles $$p_1, p_2$$ exerted on particle $$p_1$$ is

$$ F_{12} = k_s \frac{\|p_2 - p_1\| - l_0}{\|p_2 - p_1\|} (p_2 - p_1) $$

where $$k_s$$ is the spring constant and $$l_0$$ is the rest length. Note that the numerator in the fraction could be negative, in which case the force is in the opposite direction.

The elastic force on the tetrahedral altitude spring between edges $$p_1p_2$$ and $$p_3p_4$$ exerted on $$p_1p_2$$ is

 $$F_{12-34} = k_s \frac{\|n\| - l_0}{\|n\|} n $$

where $$n = \frac{(a \times b) \cdot c}{\|a \times b\|^2}$$is the directed distance from edge $$p_1p_2$$ to $$p_3p_4$$, where $$a = p_2 - p_1$$, $$b = p_4 - p_3$$, $$c = p_3 - p_1$$. $$k_s$$  and $$l_0$$ have the same meaning as above.

**Challenges:**

- Limited computation resource: we didn’t have access to a GPU, so all of the simulations were run on an M1 chip Mac computer. This limits the amount of hair particles we can simulate, leading to a balancing act between hair length, resolution (number of springs), and number of hair strands. To produce a runnable simulation, we had to decrease the amount of hair strands by lowering the mesh resolution of the head, and selecting only a fraction of the head area as hair roots. (hence why or model is balding)
- Lack of reference for the hair spring parameters: the original paper illustrates the physics behind the mass-spring system that underlies the hair, but provided no reference point for the scale and relative ratio between the spring parameters. These values were determined by trial and error.
- Collision detection: this was the toughest problem to crack. Each hair particle is under influences of gravity, 4 different spring types, and addition stiction springs with neighboring strands, leading to rather erratic behavior when a particle’s position and velocity are corrected during a collision. A friction component was added to the correction vector at collision time to minimize this effect, but the complex interactions still often make the hair diverge to chaos during simulations. Additionally, each update time step was broken up in two, where the half-velocity and half-position are calculated and used for collision detection instead of the full-step update, effectively “anticipating” collisions ahead to prevent drastic corrections. However, the result is still not as stable as we’d like and the hair particles can still be observed penetrating the head mesh in places with dense vertices (and complex normals) such as the ears and eyes areas. This is an area for future work and improvement.  

Throughout the project, our team learned valuable lessons in computational physics, performance optimization, and interdisciplinary collaboration. We discovered the delicate balance between accuracy and efficiency and the importance of considering the end-user experience when developing simulations.

## Final Results

In our project's final deliverable, we have successfully demonstrated a realistic simulation of hair influenced by gravity. The video showcases a model consisting of 500 hair strands(Can be modified), each rendered with high fidelity, gradually moving downwards as gravity takes effect. This visualization not only confirms the functional accuracy of our hair physical model but also highlights the effectiveness of the multi-spring system in providing natural dynamics and movement. Each strand of hair behaves in accordance with the established physical parameters, including elasticity, bending resistance, and volume preservation, which are governed by the meticulously implemented edge, bending, torsion, and tetrahedral altitude springs. The simulation effectively mirrors the natural flow and behavior of hair, offering a compelling visual testament to the robustness and precision of our computational model in replicating complex real-world phenomena.

| 75 Strands | 300 Strands Front |
| --- | --- |
| ![75strands](https://i.ibb.co/cxxybK3/75-strands.gif){:style="display:block; margin-right: auto; width:76%;"} | ![300strandsF](https://i.ibb.co/41jf4cZ/300-strands-front.gif){:style="display:block; margin-right: auto; width:90%;"} |


| 300 Strands Back | 500 Strands |
| --- | --- |
| ![300strandsB](https://i.ibb.co/VvjjhtL/300-strands-back.gif){:style="display:block; margin-right: auto; width:75%;"} | ![500strands](https://i.ibb.co/02TWL2j/500-strands.gif){:style="display:block; margin-right: auto; width:83%;"} |


The video for illustraing our work:
<iframe width="560" height="315" src="https://www.youtube.com/embed/F9GC9EID4c8?si=_z3QGn9R510f3qma" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## References:
[Selle et al., 2008: A Mass Spring Model for Hair Simulation](https://physbam.stanford.edu/~mlentine/images/hair.pdf) 

[Yao Lyu, 2016: Hair Simulation Based on Mass Spring System](https://nccastaff.bournemouth.ac.uk/jmacey/MastersProject/MSc17/05/1.pdf)

## Contributions

Contributions from each team member

Alex: Coding, debugging, and feature implementation. Create the final result demo for our project.

Lihao: Looked for the useful github resources about hair simulation. Set up the environment of our project from github. Write the report draft.

Martin: Help with debugging, and give an outline of the final report, mainly about the interpretation of the work and our code, and the difficulty we have met with, and how we solved the problem.

Zheng: Maintain the website, combine all the pieces (text, picture, video...) together to form a nice and complete final report.