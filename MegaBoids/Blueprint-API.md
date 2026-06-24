---
title: Blueprint API
layout: plugins
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

### Spawning Boids

#### Spawning Groups

The following functions will create a new boid group and spawn all configured boid subsets (spawn configurations).

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Spawn Group | Schedules a spawn request of the group for which the spawner is configured, with optional completion callback.<br><br>**<ins>Parameters</ins>**<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the Id of the created group as well as the handles of all created boids. | [![Spawn Group Preview](/assets/images/MegaBoids/SpawnGroupPreview.png)](/assets/images/MegaBoids/SpawnGroupPreview.png) |
| Spawn Group Immediate | Spawns the group for which the spawner is configured immediately. Boids are created upon function return but are not fully initialized yet. They will not be simulated or rendered until fully initialized.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Return value</ins>**<br>_Group ID_: ID of the group that was spawned or -1 if it failed.<br>_Spawned Boids_: List of boid handles that were spawned | [![Spawn Group Immediate Preview](/assets/images/MegaBoids/SpawnGroupImmediatePreview.png)](/assets/images/MegaBoids/SpawnGroupImmediatePreview.png) |
| Respawn Group | Schedules a respawn request for a the contents of a group associated with this spawner, with optional completion callback. This function does not despawn the group, it adds an extra batch of boids to it.<br><br>**<ins>Parameters</ins>**<br>_Group Id_: Group Id th respawn, as returned by 'Spawn Group'.<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the Id of the reused group as well as the handles of all created boids. | [![Respawn Group Preview](/assets/images/MegaBoids/RespawnGroupPreview.png)](/assets/images/MegaBoids/RespawnGroupPreview.png) |
| Respawn Group Immediate | Respawn all the contents of the spawner in a group it is associated with immediately. This function does not despawn the group, it adds an extra batch of boids to it. Boids are created upon function return but are not fully initialized yet. They will not be simulated or rendered until fully initialized.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Parameters</ins>**<br>_Group Id_: Group Id th respawn, as returned by 'Spawn Group'.<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Respawn Group Immediate Preview](/assets/images/MegaBoids/RespawnGroupImmediatePreview.png)](/assets/images/MegaBoids/RespawnGroupImmediatePreview.png) |

#### Spawning Subsets

A subset is a subgroup as defined in the 'Spawn configurations' on the spawner. Using the following Blueprint functions, you can spawn subsets individually within a previously created group. 

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Spawn Subset | Schedules a spawn request for a subset of the group as configured in the spawner, with optional completion callback.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the boid handles of all created boids. | [![Spawn Subset Preview](/assets/images/MegaBoids/SpawnSubset.png)](/assets/images/MegaBoids/SpawnSubset.png) |
| Spawn Subset Immediate | Spawns a subset of the group as configured in the spawner immediately. Boids are created upon function return but are not fully initialized yet. They will not be simulated or rendered until fully initialized.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Spawn Subset Immediate Preview](/assets/images/MegaBoids/SpawnSubsetImmediate.png)](/assets/images/MegaBoids/SpawnSubsetImmediate.png) |
| Spawn Subset with Count | Schedules a spawn request for a subset of the group as configured in the spawner, but with the provided number of entities, with optional completion callback.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Count_: Number of boids to spawn.<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the boid handles of all created boids. | [![Spawn Subset with Count Preview](/assets/images/MegaBoids/SpawnSubsetWithCount.png)](/assets/images/MegaBoids/SpawnSubsetWithCount.png) |
| Spawn Subset with Count Immediate | Spawns a subset of the group as configured in the spawner immediately. Boids are created upon function return but are not fully initialized yet. They will not be simulated or rendered until fully initialized.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group in which to spawn, as returned by the 'Spawn Group' function variants.<br>_Subset Index_: Index of the subset configuration to spawn. This index matches the 'Spawn configs' list order.<br>_Count_: Number of boids to spawn.<br><br>**<ins>Return value</ins>**<br>_Spawned Boids_: List of boid handles that were spawned. | [![Spawn Subset with Count Immediate Preview](/assets/images/MegaBoids/SpawnSubsetWithCountImmediate.png)](/assets/images/MegaBoids/SpawnSubsetWithCountImmediate.png) |

### Despawning

You can despawn boids previously created by a spawner using one of the following functions.

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Despawn Subset | Schedules a despawn request for the subset with the provided Group ID and subset *index* within the spawner.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group containing the subset to despawn, as returned by the 'Spawn Group' variants.<br>_Subset Index_: Index of the subset to despawn within the list of boid config.<br>_Destroy group if empty_: Should the subset containing group be destroyed if there are no more boids in it? | [![Despawn Subset Preview](/assets/images/MegaBoids/SpawnerDespawnSubsetPreview.png)](/assets/images/MegaBoids/SpawnerDespawnSubsetPreview.png) |
| Despawn Subset Immediate | Despawns the subset with the provided Group ID and subset *index* within the spawner.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group containing the subset to despawn, as returned by the 'Spawn Group' variants.<br>_Subset Index_: Index of the subset to despawn within the list of boid config.<br>_Destroy group if empty_: Should the subset containing group be destroyed if there are no more boids in it? | [![Despawn Subset Immediate Preview](/assets/images/MegaBoids/SpawnerDespawnSubsetImmediatePreview.png)](/assets/images/MegaBoids/SpawnerDespawnSubsetImmediatePreview.png) |
| Despawn Group | Schedules a despawn request for the group with the provided ID.<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group to despawn, as returned by the 'Spawn Group' variants. | [![Despawn Group Preview](/assets/images/MegaBoids/SpawnerDespawnGroupPreview.png)](/assets/images/MegaBoids/SpawnerDespawnGroupPreview.png) |
| Despawn Group Immediate | Despawn the group with the provided ID immediately.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.**<br><br>**<ins>Parameters</ins>**<br>_Group ID_: Id of the group to despawn, as returned by the 'Spawn Group' variants. | [![Despawn Group Immediate Preview](/assets/images/MegaBoids/SpawnerDespawnGroupImmediatePreview.png)](/assets/images/MegaBoids/SpawnerDespawnGroupImmediatePreview.png) |
| Despawn All | Schedules a despawn request for the all groups that were instantiated with this spawner. | [![Despawn All Preview](/assets/images/MegaBoids/DespawnAllPreview.png)](/assets/images/MegaBoids/DespawnAllPreview.png) |
| Despawn All Immediate | Despawn all groups that were instantiated with this spawner immediately.<br>* **Users should preferably use the non-immediate version to avoid concurrency issues.** | [![Despawn All Immediate Preview](/assets/images/MegaBoids/DespawnAllImmediatePreview.png)](/assets/images/MegaBoids/DespawnAllImmediatePreview.png) |

## MegaBoids Subsystem API

The MegaBoids Subsystem API allows finer grained control of boids lifetime management as well as their group. You do not need a spawner to use the following functions but you will be responsible for manually spawning/despawning of created entities whereas [Spawners](Spawner-and-Groups) will despawn it's associated groups when unloaded as regular actors.

[![MegaBoids Subsystem Blueprint API Preview](/assets/images/MegaBoids/SubsystemBlueprintAPIPreview.png)](/assets/images/MegaBoids/SubsystemBlueprintAPIPreview.png)

| Function  | Details     |             |
| :-------- | :---------- | :---------- |
| Get Boid Group Id | Get the boid's group Id.<br><br>**<ins>Return value</ins>**<br>_Group Id_: The group Id. | [![Get Boid Group Id Preview](/assets/images/MegaBoids/GetBoidGroupIdPreview.png)](/assets/images/MegaBoids/GetBoidGroupIdPreview.png) |
| Get Boid Subset Id | Get the boid's subset Id.<br><br>**<ins>Return value</ins>**<br>_Subset Id_: The subset Id. | [![Get Boid Subset Id Preview](/assets/images/MegaBoids/GetBoidSubsetIdPreview.png)](/assets/images/MegaBoids/GetBoidSubsetIdPreview.png) |
| Get Boid Transform | Get the current transform for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Transform_: The boid's transform. | [![Get Boid Transform Preview](/assets/images/MegaBoids/GetBoidTransformPreview.png)](/assets/images/MegaBoids/GetBoidTransformPreview.png) |
| Get Boid Velocity | Get the current velocity for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Velocity_: The boid's velocity vector. | [![Get Boid Velocity Preview](/assets/images/MegaBoids/GetBoidVelocityPreview.png)](/assets/images/MegaBoids/GetBoidVelocityPreview.png) |
| Get Boid Transform and Velocity | Get the current transform and velocity for the provided boid.<br><br>**<ins>Return value</ins>**<br>_Transform_: The boid's transform.<br>_Velocity_: The boid's velocity vector. | [![Get Boid Transform and Velocity Preview](/assets/images/MegaBoids/GetBoidTransformAndVelocityPreview.png)](/assets/images/MegaBoids/GetBoidTransformAndVelocityPreview.png) |
| Get Boid Hit result | Gets the boid that was hit by a trace hit result.<br><br>**<ins>Parameters</ins>**<br>_Trace Hit Result_: Hit result from a trace for which we want to determine if a boid was hit. <br><br>**<ins>Return value</ins>**<br>_Boid Hit Result_: Boid hit result with the handle to the boid found. | [![Get Boid Hit Result Preview](/assets/images/MegaBoids/GetBoidHitResultPreview.png)](/assets/images/MegaBoids/GetBoidHitResultPreview.png) |
| Create Boid Group | Create a boid group container.<br><br>**<ins>Parameters</ins>**<br>_Descriptor_: Descriptor for the group that contains the details of how to configure the group, [see below](#megaboidsboidgroupdescriptor). <br><br>**<ins>Return value</ins>**<br>_Group Id_: Id of the newly created group. | [![Create Boid Group Preview](/assets/images/MegaBoids/CreateBoidGroupPreview.png)](/assets/images/MegaBoids/CreateBoidGroupPreview.png) |
| Create Group Subset | Create a subset of boids within a group. Subsets are used to spawn batches of boids that shared common properties.<br><br>**<ins>Parameters</ins>**<br>_Group Id_: Id for the group that contains the subset.<br>_Descriptor_: Descriptor for the subset that contains the details of how to configure the subset, [see below](#megaboidsgroupsubsetdescriptor). <br><br>**<ins>Return value</ins>**<br>_Subset Id_: Id of the newly created subset. | [![Create Group Subset Preview](/assets/images/MegaBoids/CreateGroupSubsetPreview.png)](/assets/images/MegaBoids/CreateGroupSubsetPreview.png) |
| Spawn Boid Subset | Schedule a task to spawn a batch of boids of the provided subset.<br><br>**<ins>Parameters</ins>**<br>_Spawn Data_: The spawning details, [see below](#megaboidsgroupsubsetspawndata)<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the boid handles of all created boids. | [![Spawn Boid Subset Preview](/assets/images/MegaBoids/SpawnBoidSubsetPreview.png)](/assets/images/MegaBoids/SpawnBoidSubsetPreview.png) |
| Spawn Boid Subsets | Schedule a task to spawn multiple batches of boids of the provided subsets.<br><br>**<ins>Parameters</ins>**<br>_Spawn Data_: Array of the spawning details, [see below](#megaboidsgroupsubsetspawndata)<br>_On Spawned Callback_: Optional event called upon spawn completion. Boids might not be fully initialized yet and will not be simulated or rendered until initialization completes. The event returns the boid handles of all created boids. | [![Spawn Boid Subsets Preview](/assets/images/MegaBoids/SpawnBoidSubsetsPreview.png)](/assets/images/MegaBoids/SpawnBoidSubsetsPreview.png) |
| Despawn Boid | Schedules a task to despawn the provided boid.<br><br>**<ins>Parameters</ins>**<br>_Handle_: Handle to the boid to despawn. | [![Despawn Boid Preview](/assets/images/MegaBoids/DespawnBoidPreview.png)](/assets/images/MegaBoids/DespawnBoidPreview.png) |
| Despawn Boids | Schedules a task to despawn the provided list of boids.<br><br>**<ins>Parameters</ins>**<br>_Handles_: List of handles to the boids to despawn. | [![Despawn Boids Preview](/assets/images/MegaBoids/DespawnBoidsPreview.png)](/assets/images/MegaBoids/DespawnBoidsPreview.png) |
| Despawn Subset | Schedules a task to despawn all boids in the provided subset.<br><br>**<ins>Parameters</ins>**<br>_Subset Id_: The subset id as returned by 'Create Group Subset' function. | [![Despawn Subset Preview](/assets/images/MegaBoids/DespawnSubsetPreview.png)](/assets/images/MegaBoids/DespawnSubsetPreview.png) |
| Despawn Group | Schedules a task to despawn all boids in the provided group.<br><br>**<ins>Parameters</ins>**<br>_Group Id_: The group id as returned by 'Create Boid Group' function. | [![Despawn Group Preview](/assets/images/MegaBoids/DespawnGroupPreview.png)](/assets/images/MegaBoids/DespawnGroupPreview.png) |
| Destroy Group | Schedules a task to despawn all boids in and destroy the provided group.<br><br>**<ins>Parameters</ins>**<br>_Group Id_: The group id as returned by 'Create Boid Group' function. | [![Destroy Group Preview](/assets/images/MegaBoids/DestroyGroupPreview.png)](/assets/images/MegaBoids/DestroyGroupPreview.png) |

## Structures
### MegaBoidsBoidGroupDescriptor

[![MegaBoidsBoidGroupDescriptor Preview](/assets/images/MegaBoids/MegaBoidsBoidGroupDescriptorPreview.png)](/assets/images/MegaBoids/MegaBoidsBoidGroupDescriptorPreview.png)

| Field     | Details     |
| :-------- | :---------- |
| _Environment Subprocessors_ | List of configurations for environments subprocessors appleid to all boids of the group. |
| _Obstacles_ | List of obstacles the boid in the group need to avoid. |
| _Group Bounds_ | Bounds of the group. Boids will be pushed inwards when near these bounds. |

### MegaBoidsGroupSubsetDescriptor

[![MegaBoidsGroupSubsetDescriptor Preview](/assets/images/MegaBoids/MegaBoidsGroupSubsetDescriptorPreview.png)](/assets/images/MegaBoids/MegaBoidsGroupSubsetDescriptorPreview.png)

| Field     | Details     |
| :-------- | :---------- |
| _Boid Config_ | Data asset describing the boid composition. |
| _Driving subprocessors_ | Extra driving subprocessors that are applied on top of those defined in the boid config. They are used for environment specific behavior or specialization. |
| _Override space partition_ | Should a custom space partition config be used instead of the one defined in the boid config? |
| _Space partition config_ | Space partition configuration to use if you override the boid config space partition setup. |

### MegaBoidsSpacePartitionConfig

[![MegaBoidsSpacePartitionConfig Preview](/assets/images/MegaBoids/MegaBoidsSpacePartitionConfigPreview.png)](/assets/images/MegaBoids/MegaBoidsSpacePartitionConfigPreview.png)

| Field     | Details     |
| :-------- | :---------- |
| _Type_ | Type of space partition to use. None, 3D Cell Grid or Spatial Hash. |
| _Grid cell size factor_ | **[Spatial Hash and Grid only]**<br>Size of the cells of the space partition as a factor of boid view distance. Cell size will be equal to 'BoidSize * ViewDistanceFactor * GridCellSizeFactor'. |
| _Max boids per grid cell_ | **[Spatial Hash and Grid only]**<br>The maximum number of boids tracked within a cell, allocated on a first come, first serve basis. |
| _Cells allocated per batch_ | **[Spatial Hash only]**<br>Spatial Hash cells are allocated in batches with each batch having this number of cells reserved in memory. |
| _Initial cell batches allocated_ | **[Spatial Hash only]**<br>Initial nmumber of cells allocated when the space partition gets created. Ideally, this would cover all required cells for the boids lifetime. |
| _Range query algorithm_ | **[Spatial Hash only]**<br>Algorithm used for range queries. Default is BigMaxLitMin |

### MegaBoidsGroupSubsetSpawnData

[![MegaBoidsGroupSubsetSpawnData Preview](/assets/images/MegaBoids/MegaBoidsGroupSubsetSpawnDataPreview.png)](/assets/images/MegaBoids/MegaBoidsGroupSubsetSpawnDataPreview.png)

| Field     | Details     |
| :-------- | :---------- |
| _Subset Id_ | Id of the subset to spawn (returned from _Create Group Subset_). |
| _Entity Count_ | Number of boids to spawn. |
| _Initializer subprocessors_ | List of initializers to apply after boid spawn. |

# Extending the plugin & Mass Entity blueprint support

MegaBoids is a plugin based on the Mass Entity framework. Unfortunately, Mass Entity is currently **not compatible** with [Unreal Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) and it seems unlikely it ever will given some core design choices from Epic when developing Mass Entity. There is currently no way to create or access data types from blueprint, since they are built upon a C++ feature called 'templates'. Therefore, the following plugin extensions must absolutely be added through our C++ API:
* Driving subprocessors
* Environment subprocessors
* Initialization subprocessors
* Movement models

This is a good opportunity for you to learn C++. We put a lot of effort into abstracting the concepts of Mass Entity in a digestible manner such that developers can focus on what matters to them: **making great games**! If all else fails, check out our [consulting services](About-MegaPunk-Games#consulting-and-help).
