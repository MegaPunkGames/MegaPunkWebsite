---
title: Spawners and Groups
layout: megaboids
parent: User Guide
nav_order: 5
---

<details open markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Spawners
In the previous pages, we explained the different elements that come together to create movement in a boid. Multiple times, we mentioned the fact that some boid properties do not belong in the [boid configuration data asset](Anatomy-of-a-Boid#boid-configuration). Most of the time it is because the boid needs some reference to a level actor or we want them to have environment specific behavior. To do so, we can specialize our boid configuration from inside the level, through it's spawner.

MegaBoids spawners are an actor class that you must place in your level in order to spawn a [group of boids](#groups). When we spawn a batch of boids, we are actually spawning one group. Boid within a group share a set of properties for the duration of their lifetime. It includes their bounding box, an optional space partition and specialized driving subprocessors among others. The whole set of properties is listed below:

![Spawner Preview](/assets/images/MegaBoids/SpawnerPreview.png)

| Parameter | Description |
| :-------- | :---------- |
| Spawn on BeginPlay | Should it automatically spawn the group on BeginPlay? |
| Spawn configs | List of boid configurations to spawn within the group. |
| Initializer subprocessors | List of [initializers](Anatomy-of-a-Boid#initialization) used to setup the boids in the spawned groups. |
| Driving subprocessors | List of [driving subprocessors](Driving-Subprocessors) to specialize the boids in the spawned groups. |
| Obstacles | List of environment obstacles used for [avoidance](Anatomy-of-a-Boid#avoidance). |
| Obstacles color | Color to render the obstacle shapes in editor. |
| Use space partitioning | Should the spawned boid groups use a space partition? More details [below](#space-partition) |

# Groups
The first thing to know about groups is that they are all **independent from each other** even if originating from the same spawner. Every time you spawn a group, it will live in it's own bubble without knowledge and communication with other groups. This is a conscious decision we made that allows us to achieve better multi-threading performance since we can process groups in parallel tasks. Groups also share common data like obstacles and having this data confined only to the boids who use it also reduces the required processing. It appears limiting at first, but it's really not when you consider [composition](#group-composition). We also don't expect a large number of groups to be active at any given time. [Let us know](mailto:contact@megapunkgames.com) if you come to different conclusions or have different needs.

# Group Composition
At this point, you possibly think that the spawner takes a boid configuration and spawns a batch using that template, straps a few extras on and we're good. Although not far from the truth, the spawner has an extra trick up its sleeve: it can create boid subgroups of the same or a different configuration. This opens up so many possibilities for complex behaviors, some of which we already touched on in the [anatomy page](Anatomy-of-a-Boid#ghost-boids). Keeping in mind that there is no interaction between groups, the spawner allows group composition and entities within a group can interact. This means you can actually have cool designs like a hundred small fish roaming around a pool of shark or different personalities for a entities within a clowder of cats. Coupled with different [representations](Anatomy-of-a-Boid#representation) per boid configuration, you can achieve incredibly realistic results.

{: .note }
> When compositing boids of vastly different sizes, the larger boids automatically ignore the "considerably" smaller ones. Just like a dragon won't budge for a goblin, your boids will behave organically!

![Spawner Details](/assets/images/MegaBoids/SpawnerDetails.png)
