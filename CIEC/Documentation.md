---
title: Documentation
layout: plugins
parent: CallInEditorContainer
nav_order: 3
has_toc: true
---

<details open markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Plugin documentation

CallInEditorContainer is a simple editor utility plugin to help you manage your CallInEditor buttons. Unreal doesn't support the regular UPROPERTY attributes and meta tags on UFUNCTIONs and therefore limits how you can display your editor action buttons in the Details panel.

CallInEditorContainer aims to address these issues by adding a button container UPROPERTY which will gather the buttons that reference it and display them within the UPROPERTY's property row.

## Create your container
To create your CallInEditor buttons container, you must first declare your container property.

```
UPROPERTY(VisibleAnywhere, Category="CIEContainer|SubCategory")
FCallInEditorContainer ButtonContainer;
```

{: .important }
> Although the FCallInEditorContainer does not hold any data, it is prefereable to always include these properties within **WITH_EDITORONLY_DATA** preprocessor directives to exclude them from serialization in Shipping builds.

## Adding your CallInEditor function to your container
Once you have a container UPROPERTY, you can create the UFUNCTIONs you want it to display. We use a custom metadata tags named `CallInEditorContainer` to specify the container property to use.

```
UFUNCTION(meta=(CallInEditorContainer="ButtonContainer"))
void DoAction();
```

{: .note }
> You should not include the regular **CallInEditor** attribute, but instead use the CallInEditorContaier metadata tag. Adding CallInEditor will duplicate your button in the Details panel.

## Setup your container
Once you can see your button in the Details panel under your container, you can set it up to display how and when you want using regular UPROPERTY attributes and metadata.

| Category  | UPROPERTY attributes |
| :-------- | :--------            |
| Visibility | **Attributes**: VisibleAnywhere, VisibleDefaultsOnly, VisibleInstanceOnly, EditAnywhere, EditDefaultsOnly, EditInstanceOnly<br>**Metadata**: EditCondition, EditConditionHides<br><br>**_Note_**<br>**Visible*** and **Edit*** attributes are equivalent. The buttons are always considered "Editable". |
| Grouping and ordering | **Attributes**: Category<br>**Metadata**: DisplayAfter, DisplayPriority |

Hiding your button container can also be done with the `CanEditChange(FProperty*)` C++ function override.

You can, of course, also use the regular function attributes directly on the UFUNCTION.

| Category  | UFUNCTION attributes |
| :-------- | :--------            |
| Display   | **Attributes**: DisplayName<br>**Metadata**: Tooltip |

[![Attributes Preview](/assets/images/CIEC/AttributesPreview.png)](/assets/images/CIEC/AttributesPreview.png)

## Parent class containers
CallInEditorContainer supports adding buttons to a parent class container. This means that if class A has a container named `ParentContainer`, you can have a UFUNCTION in class B (which subclasses A) and provide it with `meta=(CallInEditor="ParentContainer")` to add it to the expected container.

```
UCLASS()
class A : public UObject
{
  UPROPERTY(VisibleAnywhere, Category="CIEContainer|SubCategory")
  FCallInEditorContainer ParentContainer;
};

UCLASS()
class B : public A
{
  UFUNCTION(meta=(CallInEditorContainer="ParentContainer"))
  void DoAction(); // Will show within the parent container property
}
```

## Blueprint support
CallInEditorContainer supports metadata injection plugins like MDMetaDataEditor. However, we cannot distribute it ourselves, nor would we want to do so. Therefore, we invite you to [download it directly from their GitHub](https://github.com/DoubleDeez/MDMetaDataEditor).

All of the previously described features can be replicated using a Blueprint metadata editor and note that the demo does demonstrate this Blueprint support, which was setup using MDMetaDataEditor. The metadata added through MDMetaDataEditor remains even when MDMetaDataEditor is not installed, therefore we can still see this feature in action in the demo.