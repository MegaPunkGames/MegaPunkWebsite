---
title: Environment Avoidance
layout: plugins
parent: User Guide
nav_order: 7
---

<details open markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Environment Avoidance
MegaBoids provides two types of environment [avoidance](Anatomy-of-a-Boid#avoidance) techniques: static and runtime. They both serve the same purpose, steering the boid away from environmental obstacles. However they differ considerably in their implementation and performance and are therefore used for different purposes. The nomenclature of static vs. runtime relates to the evaluation method: static avoidance uses a baked [Signed Distance Field](https://en.wikipedia.org/wiki/Signed_distance_function) approach whereas boids evaluate their distance to runtime obstacles during the simulation.

## Obstacles
MegaBoids uses a shared obstacle structure for both static and dynamic avoidance. This structure defines a basic primitive shape that should be either avoided or that should contain our boids.

| Property    | Description |
| :---------- | :---------- |
| Shape | Primitive shape of the obstacle. Either box, sphere or capsule. |
| Constraint | How should the obstacle contrain the boids movement? Should they push away boids or keep them inside? |
| Location | World space location of the obstacle. |
| Rotation | **[Box and Capsule Only]**<br> World space rotation of the obstacle. |
| Size | **[Box Only]**<br> Size for each axis of the box. |
| Capsule Length | **[Capsule Only]**<br> Length of the central cylinder of the capsule. |
| Radius | **[Capsule and Sphere Only]**<br> Radius of the sphere, or of the cylinder and half spheres for a capsule. |

![Obstacles Preview](/assets/images/MegaBoids/ObstaclesPreview.png)

# Static avoidance
Signed Distance Fields (SDF for short) are voxel grids or "3D textures" that store the distance to their closest surface for each voxel. Using this data, we can know if we are within avoidance distance from an obstacle as well as the direction we need to steer towards to avoid the surface. Because all the distances are baked, this avoidance method scales very well for large numbers of obstacles, for complex geometry and for large environments since the runtime cost of evaluating obstacles remains constant. As usual though, it also comes with downsides. The first one is that all boids will use the same avoidance data. The second one is that it comes at the cost of higher memory usage; SDFs can grow very large when improperly used.

To tweak them properly for your use-case and scene, MegaBoids provide the following properties:

| Property    | Description |
| :---------- | :---------- |
| SDF Sources | Sources used to bake the Signed Distance Fields for this spawner. [See below](#sdf-avoidance-sources) for the available sources and their properties. |
| SDF Interpolation Mode | Interpolation method to evaluate the distance to the closest obstacle at runtime.<br>&nbsp;&nbsp;&nbsp;&nbsp;_Nearest Neighbor_: Use the closest voxel value without interpolation. This can make your boids jittery as it crosses voxel borders especially if you have no momentum in your movement model.<br>&nbsp;&nbsp;&nbsp;&nbsp;_Trilinear_: Trilinear interpolation. Requires more samples and computations than Nearest Neighbor so it is slower but more precise and smoother. It also helps keep memory usage under control as it does not require as dense of a grid to achieve similar results. |
| SDF Probe Distance | Distance that separates each SDF "probe" from the other probes. This defines the size of your voxels and has a large impact on memory usage and avoidance precision. |

## SDF Avoidance Sources
Because SDFs can be generated at design time and are baked within the spawner, we can support multiple different obstacle sources. These sources are additive so they will all be evaluated when baking the SDF and the closest distance to any of them will be stored in the voxel grid.

| Source    | Description |             |
| :-------- | :---------- | :---------- |
| Static Obstacles | List of primitive shapes that serve as obstacles. Each shape can be avoided inwards or outwards to respectively constrain boids inside it's volume or to avoid the obstacle. | [![Static Obstacles Preview](/assets/images/MegaBoids/StaticObstaclesPreview.png)](/assets/images/MegaBoids/StaticObstaclesPreview.png) |
| Static Geometry | **(In development)** Use the static geometry within the spawner bounds as collision for avoidance. |  |
| Collision Channel | **(In development)** Use the provided channel and avoid collisions within the spawner bounds. Only static actors are considered since the SDF is baked at design time. |  |
| Nav Mesh Boundaries | **(In development)** Use the navigation mesh boundaries as walls. |  |

{: .note }
> You can implement your own SDF avoidance sources by subclassing `UMegaBoidsStaticAvoidanceSource`. See the [Blueprint API documentation](Blueprint-API#static-avoidance-source) for more information.

# Runtime avoidance

Runtime obstacles are limited to basic primitive shapes. While this might appear redundant because basic primitives can also be used as a source for static obstacles, runtime obstacles allows different options. First, since the SDF is baked at design time, we cannot use moving obstacles within an SDF as we would need to rebuild the SDF continuously, greatly reducing performance.

Another feature that comes from dynamic evaluation is per-boid behavior. Since the runtime obstacles are evaluated during play, we have more information for each obstacle that allow the uuser to change the steering response per entity. Specifically, at the moment, this means we can have some osbtacles only affect certain boids depending on their configuration, similar to the [spawner driving subprocessors](Driving-Subprocessors).

| Property    | Description |             |
| :-------- | :---------- | :---------- |
| Obstacles | List of environment obstacles. Each obstacle can be turned on or off per boid configuration using the configuration selector. | [![Dynamic Obstacles Preview](/assets/images/MegaBoids/DynamicObstaclesPreview.png)](/assets/images/MegaBoids/DynamicObstaclesPreview.png) |

{: .note }
> Dynamic/Moving obstacles are currently supported by accessing the `FMegaBoidsDynamicObstaclesSharedFragment` shared fragment and modifying the obstacle transforms directly. This is somewhat inconvenient and a better approach would be to have dynamic obstacles being entities themselves. This will be evaluated in a future release.

# Editor helper options

Other options within the avoidance properties are tools to help with development.

| Property          | Description                                                      |
| :---------------- | :--------------------------------------------------------------- |
| Obstacles color   | Color to render the obstacle shapes in the editor viewport.      |
| SDF Auto Refresh  | **[Static Avoidance Only]** Option to automatically regenerate the SDF when data changes, or a button to do it manually. |
| Draw SDF Gradient | **[Static Avoidance Only]** Debug toggle to render the SDF gradients in the editor viewport. |
