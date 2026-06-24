---
  title: Release Notes
  layout: plugins
  nav_order: 1
  parent: MegaBoids
---

# Releases
## Version 0.1.0
 - First ALPHA release

## Version 0.1.1
 - Added Flee driving subprocessor
 - Reworked Follower driving subprocessor
 - Fix crash when reordering driving subprocessors in list

## Version 0.1.2
 - Added Wander driving subprocessor
 - Grouped all propulsion driving subprocessors into a master Propulsion
 - Merged Kinematic/Kinetic particle movements into Simple Particle movement

## Version 0.2.0
 - Added Mass Actor representation

## Version 0.3.0
 - Added Spatial hash space partition
 - Added boid size criteria to follow/flee driving subprocessors

## Version 0.3.1
 - Added Spatial hash LitMin/BigMax search algorithm
 - Added project settings
 - Added default boid config space partition
 - Added per spawner & spawn config space partition override
 
## Version 0.4.0
 - Allow respawning into existing group
 - Chaos physics integration allowing traces
 - Improved Blueprint API on Spawner and new Subsystem
 - Added max delta time setting in project settings
 - Added "Is instant" to impulse forces to make it a velocity change vs. an acceleration
