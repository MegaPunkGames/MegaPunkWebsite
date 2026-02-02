---
title: Blueprint API
layout: megaboids
parent: Technical Reference
nav_order: 1
back_to_top: true
back_to_top_text: "Back to top"
---

<details open markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Blueprint API

Some functions of the plugin are be exposed through blueprint but the blueprint API is fairly limited. This is due to [blueprint support in the Mass Entity framework](#Extending-the-plugin-Mass-Entity-blueprint-support).
The blueprint API is curently limited to spawning and despawning and can be accessed through the [spawner actor](Spawner-and-Groups).

## Spawner API
![Spawner Blueprint API Preview](/assets/images/MegaBoids/SpawnerBlueprintAPIPreview.png)

### Spawning Groups

The following functions will create a new boid group and spawn all configured boid subsets (spawn configurations).

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Spawn Group | Schedules a spawn request of the group for which the spawner is configured. | [![Spawn Group Preview](/assets/images/MegaBoids/SpawnGroupPreview.png)](/assets/images/MegaBoids/SpawnGroupPreview.png) |
| Spawn Group with Completion | Schedules a spawn request of the group for which the spawner is configured, with completion callback.<br><br>**<ins>Parameters</ins>**<br>_Completion Callback_: Event called upon spawn completion. The event returns the Id of the spawned group as well as the handles of all created boids. | [![Spawn Group with Completion Preview](/assets/images/MegaBoids/SpawnGroupWithCompletion.png)](/assets/images/MegaBoids/SpawnGroupWithCompletion.png) |
| Spawn Group Immediate | Spawns the group for which the spawner is configured immediately.<br>**Boids are created upon function return but are not fully initialized yet. Users should preferably use the non-immediate version.**<br><br>**<ins>Return value</ins>**<br>_Group ID_: ID of the group that was spawned or -1 if it failed.<br>_Spawned Boids_: List of boid handles that were spawned | [![Spawn Group Immediate Preview](/assets/images/MegaBoids/SpawnGroupImmediatePreview.png)](/assets/images/MegaBoids/SpawnGroupImmediatePreview.png) |
| Respawn Group | Schedules a respawn request for a the contents of a group associated with this spawner. This function does not despawn the group, it adds an extra batch of boids to it. | [![Respawn Group Preview](/assets/images/MegaBoids/RespawnGroupPreview.png)](/assets/images/MegaBoids/RespawnGroupPreview.png) |
| Respawn Group Immediate | Respawn all the contents of the spawner in a group it is associated with immediately. This function does not despawn the group, it adds an extra batch of boids to it.<br>**Boids are created upon function return but are not fully initialized yet. Users should preferably use the non-immediate version.**<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Respawn Group Immediate Preview](/assets/images/MegaBoids/RespawnGroupImmediatePreview.png)](/assets/images/MegaBoids/RespawnGroupImmediatePreview.png) |
| Respawn Group with Completion | Schedules a respawn request for a the contents of a group associated with this spawner, with completion callback. This function does not despawn the group, it adds an extra batch of boids to it.<br><br>**<ins>Parameters</ins>**<br>_Completion Callback_: Event called upon spawn completion. The event returns the Id of the reused group as well as the handles of all created boids. | [![Respawn Group with Completion Preview](/assets/images/MegaBoids/RespawnGroupWithCompletionPreview.png)](/assets/images/MegaBoids/RespawnGroupWithCompletionPreview.png) |

### Spawning Subsets

A subset is a subgroup as defined in the 'Spawn configurations' on the spawner. Using the following Blueprint functions, you can spawn subsets individually within a previously created group. 

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Spawn Subset | Schedules a spawn request for a subset of the group as configured in the spawner.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order. | [![Spawn Subset Preview](/assets/images/MegaBoids/SpawnSubset.png)](/assets/images/MegaBoids/SpawnSubset.png) |
| Spawn Subset Immediate | Spawns a subset of the group as configured in the spawner immediately.<br>**Boids are created upon function return but are not fully initialized yet. Users should preferably use the non-immediate version.**<br><br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Spawn Subset Immediate Preview](/assets/images/MegaBoids/SpawnSubsetImmediate.png)](/assets/images/MegaBoids/SpawnSubsetImmediate.png) |
| Spawn Subset with Completion | Schedules a spawn request for a subset of the group as configured in the spawner.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Completion Callback_: Event called upon spawn completion. The event returns the entity handles of all created boids. | [![Spawn Subset with Completion Preview](/assets/images/MegaBoids/SpawnSubsetWithCompletion.png)](/assets/images/MegaBoids/SpawnSubsetWithCompletion.png) |
| Spawn Subset with Count | Schedules a spawn request for a subset of the group as configured in the spawner, but with the provided number of entities.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Count_: Number of boids to spawn. | [![Spawn Subset with Count Preview](/assets/images/MegaBoids/SpawnSubsetWithCount.png)](/assets/images/MegaBoids/SpawnSubsetWithCount.png) |
| Spawn Subset with Count Immediate | Spawns a subset of the group as configured in the spawner immediately.<br>**Boids are created upon function return but are not fully initialized yet. Users should preferably use the non-immediate version.**<br><br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Count_: Number of boids to spawn.<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Spawn Subset with Count Immediate Preview](/assets/images/MegaBoids/SpawnSubsetWithCountImmediate.png)](/assets/images/MegaBoids/SpawnSubsetWithCountImmediate.png) |
| Spawn Subset with Count and Completion | Schedules a spawn request for a subset of the group as configured in the spawner.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Count_: Number of boids to spawn.<br>_Completion Callback_: Event called upon spawn completion. The event returns the entity handles of all created boids. | [![Spawn Subset with Count and Completion Preview](/assets/images/MegaBoids/SpawnSubsetWithCountAndCompletion.png)](/assets/images/MegaBoids/SpawnSubsetWithCountAndCompletion.png) |

### Despawning Groups

You can despawn boids previously created by a spawner using one of the following functions.

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Despawn Group | Schedules a despawn request for the group with the provided ID.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group to despawn, as returned by the 'Spawn Group' variants. | [![Despawn Group Preview](/assets/images/MegaBoids/DespawnGroupPreview.png)](/assets/images/MegaBoids/DespawnGroupPreview.png) |
| Despawn Group Immediate | Despawn the group with the provided ID immediately. This is an unsafe call to do on the game thread as there might be background tasks currently running.<br>**Users should preferably use the non-immediate version.**<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group to despawn, as returned by the 'Spawn Group' variants. | [![Despawn Group Immediate Preview](/assets/images/MegaBoids/DespawnGroupImmediatePreview.png)](/assets/images/MegaBoids/DespawnGroupImmediatePreview.png) |
| Despawn All | Schedules a despawn request for the all groups that were instanciated with this spawner. | [![Despawn All Preview](/assets/images/MegaBoids/DespawnAllPreview.png)](/assets/images/MegaBoids/DespawnAllPreview.png) |
| Despawn All Immediate | Despawn all groups that were instanciated with this spawner immediately. This is an unsafe call to do on the game thread as there might be background tasks currently running.<br>**Users should preferably use the non-immediate version.** | [![Despawn All Immediate Preview](/assets/images/MegaBoids/DespawnAllImmediatePreview.png)](/assets/images/MegaBoids/DespawnAllImmediatePreview.png) |

## MegaBoids Subsystem API

The MegaBoids Subsystem API allows finer grained control of boids lifetime management as well as their group. You do not need a spawner to use the following functions but you will be responsible for manually spawning/despawning of created entities whereas [Spawners](Spawner-and-Groups) will despawn it's associated groups when unloaded as regular actors.

[![MegaBoids Subsystem Blueprint API Preview](/assets/images/MegaBoids/SubsystemBlueprintAPIPreview.png)](/assets/images/MegaBoids/SubsystemBlueprintAPIPreview.png)

{: .note }
> More Blueprint functions will be provided in the future for finer control over boids. We are currently planning to add spawning and despawning without a spawner reference, optimized collision tracing, etc. in order to support more complex gameplay scenarios.


| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Get Boid Transform | Get the current transform for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Transform_: The boid's transform. | [![Get Boid Transform Preview](/assets/images/MegaBoids/GetBoidTransformPreview.png)](/assets/images/MegaBoids/GetBoidTransformPreview.png) |
| Get Boid Velocity | Get the current velocity for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Velocity_: The boid's velocity vector. | [![Get Boid Velocity Preview](/assets/images/MegaBoids/GetBoidVelocityPreview.png)](/assets/images/MegaBoids/GetBoidVelocityPreview.png) |
| Get Boid Transform and Velocity | Get the current transform and velocity for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Transform_: The boid's transform.<br>_Velocity_: The boid's velocity vector. | [![Get Boid Transform and Velocity Preview](/assets/images/MegaBoids/GetBoidTransformAndVelocityPreview.png)](/assets/images/MegaBoids/GetBoidTransformAndVelocityPreview.png) |
| Get Boid Hit result | Gets the boid that was hit by a trace hit result.<br><br>**<ins>Parameters</ins>**<br>_Trace Hit Result_: Hit result from a trace for which we want to determine if a boid was hit. <br><br>**<ins>Return value</ins>**<br>_Boid Hit Result_: Boid hit result with the handle to the boid found. | [![Get Boid Hit Result Preview](/assets/images/MegaBoids/GetBoidHitResultPreview.png)](/assets/images/MegaBoids/GetBoidHitResultPreview.png) |
| Despawn Boid | Schedules a task to despawn the provided boid.<br><br>**<ins>Parameters</ins>**<br>_Handle_: Handle to the boid to despawn. | [![Despawn Boid Preview](/assets/images/MegaBoids/DespawnBoidPreview.png)](/assets/images/MegaBoids/DespawnBoidPreview.png) |
| Despawn Boids | Schedules a task to despawn the provided list of boids.<br><br>**<ins>Parameters</ins>**<br>_Handles_: List of handles to the boids to despawn. | [![Despawn Boids Preview](/assets/images/MegaBoids/DespawnBoidsPreview.png)](/assets/images/MegaBoids/DespawnBoidsPreview.png) |


# Extending the plugin & Mass Entity blueprint support

MegaBoids is a plugin based on the Mass Entity framework. Unfortunately, Mass Entity is currently **not compatible** with [Unreal Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) and it seems unlikely it ever will given some core design choices from Epic when developing Mass Entity. There is currently no way to create or access data types from blueprint, since they are built upon a C++ feature called 'templates'. Therefore, the following plugin extensions must absolutely be added through our C++ API:
* Driving subprocessors
* Environment subprocessors
* Initialization subprocessors
* Movement models

This is a good opportunity for you to learn C++. We put a lot of effort into abstracting the concepts of Mass Entity in a digestible manner such that developers can focus on what matters to them; making great games! If all else fails, check out our [consulting services](About-MegaPunk-Games#consulting-and-help).
