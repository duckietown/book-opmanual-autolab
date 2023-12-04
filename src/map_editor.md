(autolab-map-editor)=
# Autolab Map Editor

```{needget}
* Knowledge about what an Autolab is
---
* Created map for Duckietown
```

## Map Editor

You can launch the map editor using the following command:

```bash
dts map editor
```

You will see the map editor window:

```{figure} ./_images/map_editor/start.png
:name: fig:editor1

Map editor interface
```

## How to Create a New Map?

To create a new map, you should specify the sizes (width, height) of the map, the size of tiles (width, height), map name, folder for saving the map, and base title for filling the map.

| Setting              | Value              |
|----------------------|--------------------|
| Default map size     | `5 x 5`            |
| Default tile size    | `0.585 x 0.585`    |
| Default map name     | `map_1`            |
| Default folder in Docker | `/out/maps/map1` |
| Default title        | `asphalt`          |

```{figure} ./_images/map_editor/create_map_form.png
:name: fig:editor2

Create map form
```

Then, the map generation process should be initialized. Sample editor window:

```{figure} ./_images/map_editor/start.png
:name: fig:editor3

Created map
```

## How to Add an Object to the Map

You can add any object(s) from the left menu, except the first two blocks, "Road tiles" and "Fill tiles," which describe the types of tiles. To add an object, double-click on the object in the left menu.

```{figure} ./_images/map_editor/add_obj.png
:name: fig:editor4

Add object to the map
```

For editing the attributes of an object, right-click on the object and edit its attributes in the opened window. The window should look like this:

```{figure} ./_images/map_editor/edit_obj.png
:name: fig:editor5

Edit object form
```

For editing tiles, you should activate the brush mode. After that, choose the type of tiles in the left menu. Then, you can select/brush the grids to apply the selected tile type.

```{figure} ./_images/map_editor/brush.png
:name: fig:editor6

Brush button
```

```{figure} ./_images/map_editor/after_brush.png
:name: fig:editor7

Painting tiles
```

## Select Objects

To select objects, simply click on them.

```{figure} ./_images/map_editor/select_obj.png
:name: fig:editor8

Select objects
```

## Rotate Tiles and Objects

For rotation, select the required tiles and objects, and click the rotation button in the toolbar.

```{figure} ./_images/map_editor/rotate.png
:name: fig:editor9

Rotate button
```

```{figure} ./_images/map_editor/after_rotate.png
:name: fig:editor10

Rotation result
```

## Delete Objects

To delete objects, select the required objects and click the delete button in the toolbar.

```{figure} ./_images/map_editor/delete.png
:name: fig:editor11

Delete button
```

## Copy Tiles and Objects

For copying, select the required tiles and objects, and click the copy button in the toolbar.

```{figure} ./_images/map_editor/copy.png
:name: fig:editor12

Copy button
```

For example, select straight road tiles and copy them.

```{figure} ./_images/map_editor/copy_obj.png
:name: fig:editor13

Select tiles for copying
```

## Paste Objects

To paste objects, copy tiles and objects, click on the tile on the map, and then click the paste button in the toolbar.

```{figure} ./_images/map_editor/paste.png
:name: fig:editor14

Paste button
```

```{figure} ./_images/map_editor/paste_obj.png
:name: fig:editor15

Pasting result
```

## Cut Objects

For cutting, select the required tiles and objects and click the cut button in the toolbar. Tiles will be replaced with "asphalt," and objects will be deleted. Cut tiles and objects will be copied.

```{figure} ./_images/map_editor/cut.png
:name: fig:editor16

Cut button
```

## Save Map to PNG

To save the map as an image, click the "Save to PNG" button in the toolbar.

```{figure} ./_images/map_editor/save_map_as_png.png
:name: fig:editor17

Save map as PNG button
```

Then, select the desired name for saving the image and its height in the form.

```{figure} ./_images/map_editor/save_to_png_form.png
:name: fig:editor18

Save map as PNG form
```

The image is saved in the directory from which the editor is launched.

```{figure} ./_images/map_editor/after_save_to_png.png
:name: fig:editor19

Result of saving the map as PNG
```

## Save Map

To save the map, click the "Save" button in the toolbar. If you want to select a folder to save the map, click the "Save Map As" button in the toolbar and select the folder on the filesystem where the map will be saved.

```{figure} ./_images/map_editor/save_map.png
:name: fig:editor20

Save map button
```

```{figure} ./_images/map_editor/save_map_as.png
:name: fig:editor21

Save map as button
```

```{figure} ./_images/map_editor/save_map_filesystem.png
:name: fig:editor22

Select folder in the filesystem for saving the map
```

## Open Map

To open a map, click the "Open" button in the toolbar and select the folder on the filesystem where the map is located.

```{figure} ./_images/map_editor/open_map.png
:name: fig:editor23

Open map button
```

```{figure} ./_images/map_editor/open_map_filesystem.png
:name: fig:editor24

Select folder in the filesystem for opening the map
```

## Moving Map

To move the map, click the middle mouse button and drag the map.

```{figure} ./_images/map_editor/paste_obj.png
:name: fig:editor25

Map before dragging
```

```{figure} ./_images/map_editor/move_map.png
:name: fig:editor26

Map after dragging
```

To move the map to the top left corner of the window, click the "To the Corner" button in the toolbar.

```{figure} ./_images/map_editor/corner.png
:name: fig:editor27

To the Corner button
```

## Scaling the Map

To scale the map within the window, simply roll the mouse wheel.

## Actions History

To navigate through the history of changes, you can click on the "Undo" button or the "Redo" button.

```{figure} ./_images/map_editor/undo.png
:name: fig:editor28

Undo button
```

```{figure} ./_images/map_editor/shift_undo.png
:name: fig:editor29

Redo button
```

## Debug Line

At the bottom of the editor, you will find a debug line displaying Qt coordinates and map-translated coordinates.

```{figure} ./_images/map_editor/debug_line.png
:name: fig:editor30

Debug line
```

## Dynamic Layers

Dynamic layers are layers not defined in `dt-maps`, but they can be read and configured directly from a file. Currently, you can only read and display such layers in the editor without the ability to add new objects to dynamic layers on the map.

```{figure} ./_images/map_editor/dynamic_layer_example.png
:name: fig:editor31

Example of a dynamic layer in a YAML file
```

To add a layer, create a file with the name of the layer, add layer objects with the desired configuration to it, and add the location of the objects within the frame layer.

```{figure} ./_images/map_editor/frames_dynamic_obj.png
:name: fig:editor32

Added frame objects for dynamic layers
```

When opening the map, the objects of dynamic layers will be displayed as gray icons.

```{figure} ./_images/map_editor/dynamic_layer_view.png
:name: fig:editor33

Dynamic layer objects on the map
```

Objects within dynamic layers can be modified using the edit form.

```{figure} ./_images/map_editor/dynamic_object_form.png
:name: fig:editor34

Edit form for dynamic objects
```

## Hotkeys

| Action          | Hotkey        |
|-----------------|---------------|
| Exit            | <kbd>Ctrl+Q</kbd>        |
| Create map      | <kbd>Ctrl+N</kbd>        |
| Open map        | <kbd>Ctrl+O</kbd>        |
| Save map        | <kbd>Ctrl+S</kbd>        |
| Save map as     | <kbd>Ctrl+Alt+S</kbd>    |
| Save to png     | <kbd>Ctrl+P</kbd>        |
| Delete          | <kbd>Ctrl+D</kbd>        |
| Undo            | <kbd>Ctrl+Z</kbd>        |
| Shift undo      | <kbd>Ctrl+Shift+Z</kbd>  |
| Rotate          | <kbd>Ctrl+R</kbd>        |
| To the corner   | <kbd>Ctrl+M</kbd>        |
| Copy            | <kbd>Ctrl+C</kbd>        |
| Cut             | <kbd>Ctrl+X</kbd>        |
| Paste           | <kbd>Ctrl+V</kbd>        |
| Brush mode      | <kbd>Ctrl+B</kbd>        |
