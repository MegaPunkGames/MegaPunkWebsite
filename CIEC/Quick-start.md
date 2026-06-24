---
title: Quick Start
layout: plugins
parent: CallInEditorContainer
nav_order: 2
has_toc: false
---

# Installation
## From Fab
- Open Fab in the Epic Game Launcher
- Find the plugin from the catalog
- Click the install plugin button
- Select the desired engine version and Install
- Open your game project
- Go to the Plugins page and find CallInEditorContainer
- Enable the plugin

## From Source
- Get the source from this repository.
- Copy the Plugins/CallInEditorContainer folder into your own game project's Plugin folder.
- Generate project files
- Compile in Visual studio
- Open your game project
- Go to the Plugins page and find CallInEditorContainer
- Enable the plugin

# First steps
- Add a UPROPERTY of class `FCallInEditorContainer`. You will use this UPROPERTY to manage your buttons using attributes and meta tags (Category, DisplayAfter, EditCondition, etc.).
**Make sure to put this property within `WITH_EDITORONLY_DATA` preprocessor directives or your packaged game will not build.**
- Mark your UFUNCTIONs with the `meta=(CallInEditorContainer=[PROPERTYNAME])` meta tag to place them within the container declared above.

See the [full documentation](Documentation) for more information.