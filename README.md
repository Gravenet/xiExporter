XiExporter
------------------

Export FFXI maps into OBJ format.

This is a work in progress. Maps are currently exported into many individual OBJ files.

## Building

You must install DirectX 9 SDK. Here is the link:
http://www.microsoft.com/en-us/download/confirmation.aspx?id=6812

You'll then need to add the d3dx9.h, lib, and dll to the linker in Visual Studio.

## Importing Into Blender

Currently, all objects on the map are exporting into individual files. To create
the full mesh:

1. Copy all OBJ files into "C:\objects".
2. Open a new Blender file and delete everything.
3. Open up the "Text Editor" window.
4. Create a new file (Ctrl+N).
5. Paste in this script:

```
import os
import bpy

# put the location to the folder where the objs are located here in this fashion# this line will only work on windows ie C:\objects
path_to_obj_dir = os.path.join('C:\\', 'objects')

# get list of all files in directory
file_list = sorted(os.listdir(path_to_obj_dir))

# get a list of files ending in 'obj'
obj_list = [item for item in file_list if item[-3:] == 'obj']

# loop through the strings in obj_list and add the files to the scene
for item in obj_list:
    path_to_file = os.path.join(path_to_obj_dir, item)
    bpy.ops.import_scene.obj(filepath = path_to_file)
```

6. Run the script.
7. Export the map as OBJ and you're done.

## Prepping Mesh for Navmeshing

1. Mirror the Y axis (Order matters)
2. Mirror the Z axis
3. Flip wrong faces (select object, tab, a, w, flip faces, press tab to exit). In Blender blue faces are facing the wrong way.
4. Delete walkable meshes. Meshes which do not stop player movement (bushes)
   should be removed.
5. Delete walkable areas which shouldn't be walkable (lakes.)

### Checking Coordinates

To validate the map has been rotated correctly, jump into the game and compare
points from the game via @pos and points in Blender. The y and z coordinates from FFXI
should be swapped and the new z should be reversed.

Example:

@pos in FFXI -> (100, 5, 20)
Formula: (x, y, -z)
So the coordinates in Blender should be (100, 20, -5).

Easiest way to test this is by moving the 3D cursor to the position. In Blender
press N and scroll down to 3D Cursor, then input the coordinates.

## RecastDemo Settings

Use these settings when creating a navmesh.

* Tile Size 256
* Cell Size 0.40
