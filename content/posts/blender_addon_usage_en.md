+++
title = "[Blender] arch4e's add-ons usage"
date = 2023-12-01

[taxonomies]
tags = ["3DCG", "Blender", "Python"]
+++

## Overview

This article compiles the usage instructions for custom add-ons, detailing the operating environment for each add-on separately.
As it may not be fully compatible with the latest release, please refer to GitHub release notes or similar sources for changes beyond the specified version.

For inquiries and tool creation requests, please check the end of the article.

## Add-on Usage

### bpgrip

> Download: [GitHub](https://github.com/arch4e/bpgrip) / [Booth](https://arch4e.booth.pm/items/4566568)

#### Main Features

* Register keyboard shortcuts for switching blending modes of brush-type tools

#### Usage (Assuming Weight Paint Mode)

> Tested on: bpgrip v2.0 / Blender 3.6.5
> It should also work on Blender 4.0, the latest version at the time of writing.

1. Go to `Edit >> Preferences` from the top left of the screen and navigate to Keymap.
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/1.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Scroll down a bit and go to `3D View >> Weight Paint >> Weight Paint (Global)` (※1).
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/2.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Press `Add New` , then press the triangle icon.
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/3.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Change the entry that says `none` to `bpgrip.set_bbmode` , and assign the keyboard shortcut.
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/4.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Set the `bl_mode` to `Draw` (※2) and the `bb_mode` to the tool you want to assign the shortcut (※3).
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/5.png", alt="screenshot", position="center", style="height: 30vh") }}
1. With these steps, you can switch Brush Blending Modes using the keyboard shortcut.

```
※1,2: For brush tools in Sculpt Mode and similar, refer to the "Keymap" section in the "readme.md" on GitHub.
       https://github.com/arch4e/bpgrip
※3: Basic tool names are usually straightforward, such as "Add" for the "ADD" brush. You can also check from the Inspector at the bottom left of Blender's "Scripting" tab, which shows the operation log. Also, the `bl_mode` in ※2 can be confirmed in a similar log.
```

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/6.png", alt="screenshot", position="center", style="height: 30vh") }}

### lab04

> Download: [GitHub](https://github.com/arch4e/lab04)

#### Main Features

※Implements various small functions not substantial enough to be a separate add-on, and relatively frequently conducts feature integration and removal.

* Match mesh names with object names
* Rename vertex groups in bulk (with regular expression)
* Toggle `UV Sync Selection` , allowing other UV maps to remain visible while disabled
* Assist in assigning weights to vertex groups
* ~~Perform simple tasks by instructing ChatGPT (joke feature)~~ Deprecated in v0.5.0

#### Usage

> Tested on: bpgrip v2.0 / Blender 3.6.5, 4.0.0

##### Match Mesh Names with Object Names

1. 3D Viewの右にある `Tool` エリアから `Lab. 04` パネルを開く
1. `sync name (obj. to mesh)` ボタンを押すと全てのメッシュの名前がObjectの名前で上書きされる
1. Open the `Lab. 04` panel from the `Tool` area on the right side of the 3D View.
1. Press the `sync name (obj. to mesh)` button to overwrite all mesh names with object names.

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/1.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/2.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Rename Vertex Groups in Bulk (with Regular Expression)

1. Select and make active the object you want to operate on.
1. Enter the search pattern and replacement string, then press `Rename V-Groups` .
   * The pattern is interpreted as a regular expression, so you can use it to add a prefix by specifying ^ (meaning "start of line" in regular expressions).

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/1.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/2.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Toggle `UV Sync Selection` , Allowing Other UV Maps to Remain Visible While Disabled

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/1.png", alt="screenshot", position="center", style="width: 50vw") }}

1. Go to `Edit >> Preferences` from the top left of the screen and navigate to `Keymap` .
1. Scroll down a bit and go to `Image >> UV Editor >> UV Editor (Global)` .
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/2.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Similar to bpgrip, assign a keyboard shortcut to `lab04.switch_uv_select_sync` using `Add New` .
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/3.png", alt="screenshot", position="center", style="height: 30vh") }}
1. Execute the set keyboard shortcut in the UV Editing tab to toggle `UV Sync Selection` .

##### Assist in Assigning Weights to Vertex Groups

A panel for assigning weights to vertex groups appears in the `Item` area.
Although it's essentially the same as the default functionality, it allows specifying weight values with the mouse cursor. Note that the application of weights is triggered when the `Assign` button is pressed.

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/weight_assign_util/1.png", alt="screenshot", position="center", style="width: 50vw") }}

### shake

> Download: [GitHub](https://github.com/arch4e/shake/releases) / [Booth](https://arch4e.booth.pm/items/4843394)

#### Main Features

* Create Shape Keys from CSV
* Move active Shape Key to any position on the list
* Align based on Prefix (based on runtime order, not AtoZ)
* Sort by Prefix
* Transcribe Shape Keys

#### Usage

> Tested on: bpgrip v2.0 / Blender 3.6.5, 4.0.0

##### Create Shape Keys from CSV

1. Load a list of Shape Keys you want to create in your Blender project from CSV.
1. Delimiters: `line break (LF or CR+LF)` and `,` .
   * You can also create them from the Scripting Tab.
1. Open `Create Shape Keys from CSV` in the `ShaKe` area of the 3D View.
1. Select the CSV file from the dropdown menu.
1. Choose the object to create Shape Keys for and press `create shape keys from csv` .

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/create_shape_keys_from_csv/2.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/create_shape_keys_from_csv/3.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Move active Shape Key to any position on the list

1. Select an object and make it active.
1. Choose the Shape Key you want to move and hover over `Move Shape Key` in the right-click menu.
1. The active Shape Key will move below the selected one in the list.

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/move_shape_key/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Align based on Prefix (based on runtime order, not AtoZ)

1. Select an object and make it active.
1. Press Align by Prefix in the right-click menu to sort by each prefix.
   * It sorts based on the order of prefix existence, not AtoZ.

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/align_by_prefix/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Sort by Prefix

1. Open Order Management in the `ShaKe` area of the `3D View` .
1. Create a list of prefixes from the panel's right button.
   * Click the arrow icon to automatically retrieve information from the active object (be careful, as it overwrites).
   * Manual creation is also possible with the `+` icon.
   * Supports multi-level prefixes like `MAIN_sub_` , determined by the longest match from the beginning.
1. Press Rearrange, and Shape Keys will be sorted according to the list order.
   * Since it looks for a match from the beginning, the delimiter ( `_` ) is not mandatory.
     * Example: `K` matches `Key~` .

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/reorder/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Transcribe Shape Keys

1. Open `Shape Keys Selector` and `Transcribe Shape Keys` in the `ShaKe` area of the 3D View.
1. In `Shape Keys Selector` , choose the Shape Keys you want to transcribe from either the project or the selected object.
1. In `Transcribe Shape Keys` , select the object where you want to transcribe (create) the Shape Keys.
1. Press `transcribe` , and the selected object will have the corresponding Shape Keys created.

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/transcribe/1.png", alt="screenshot", position="center", style="width: 50vw") }}

### stree

> Download: [GitHub](https://github.com/arch4e/stree) / [Booth](arch4e.booth.pm/items/4694745)

#### Main Features

* Backup objects to a separate collection
* Easily preview and restore backups

#### Usage

Not specified, as it is not intended for general use.

## Inquiries and Requests for Addon Creation and Error Support

For questions about released addons, requests for addon creation, and consultations regarding technical support (such as hearing about errors in other addons or creating simple patches), please contact the SNS listed at [the top of this page](https://arch4e.com).
Please also provide details about the addon or budget for addon creation and technical support.
