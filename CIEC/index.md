---
title: CallInEditorContainer
layout: plugins
parent: Projects
nav_order: 1
has_toc: false
---

# Introduction
Customization helper to add CallInEditor buttons nested under subcategories in the editor Details panel.
Unreal doesn't allow having nested CallInEditor in subcategories, hiding the buttons or reordering them in the Details panel. This plugin adds those features by forwarding the buttons to a "Container".

To do so, you must first add a UPROPERTY of class `FCallInEditorContainer`. You will use this UPROPERTY to manage your buttons using categories or meta tags (DisplayAfter, EditConditionHides, etc.)
You then simply need to mark your UFUNCTIONs with the `CallInEditorContainer` meta tag to place them within the container.


[Get CallInEditorContainer on Fab](https://www.fab.com/listings/dcc4e9bb-9097-48c0-a9cf-30168b003463)

[Check out the documentation](Documentation)

# Features
* Editor helper tool to manage your CallInEditor buttons location, visibility and order
* Place CallInEditor buttons within subcategories
* Split your CallInEditor buttons within different containers
* Change buttons order with DisplayAfter meta tags
* Support for EditCondition && EditConditionHides meta tags
* Blueprint support (with support from 3rd party plugins like MDMetaDataEditor)
