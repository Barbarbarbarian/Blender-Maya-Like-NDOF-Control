# Blender-Maya-Like-NDOF-Control
Maya-like NDOF controller movement scheme for Blender.

If you are using NDOF controller (e.g. 3DConnexion SpaceNavigator) and preference control scheme which allow you rotating around CENTRE of the SELECT OBJECT as well as panning view SIMULTANOUSLY, like in Maya, this is what you need.

## USAGE:

Download the source and libraries of Blender from official Blender Git and SVN repositories. (Details can be found in [there](https://wiki.blender.org/index.php/Dev:Doc/Building_Blender)). Replace "view3d_edit.c" with file provided in this repository. Compile and run.

After Blender started, change NDOF setting to "Orbit".

## What happened?

The
