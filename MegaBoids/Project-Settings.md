---
title: Project Settings
layout: megaboids
parent: User Guide
nav_order: 7
---

# Project Settings

Within the **Project Settings - MegaBoids** page, you will find some project specific settings for the plugin, unsurprisingly. These settings allow basic configuration of the plugin for your game.

![Project Settings Preview](/assets/images/MegaBoids/ProjectSettingsPreview.png)

| Settings | Details |
| :------- | :---------- |
| Maximum Delta Time | The maximum delta time to use when updating boids. Since movement is often based on velocity, it can get out of control with large timesteps (when there is a framerate drop / stutter). This settings allows to limit the frame simulation deltatime to a given maximum value to prevent excessive movement. |
| Default Space Partition | The space partition settings to use by default for the project. This setting can be overriden at the [boid configuration](Anatomy-of-a-Boid#boid-configuration) level as well as on the [spawner](Spawners-and-Groups). For more details on the properties within this setting, see [here](Space-Partition#space-partition-settings).
