# Blender-Maya-Like-NDOF-Control
Maya-like NDOF controller movement scheme for Blender.

If you are using NDOF controller (e.g. 3DConnexion SpaceNavigator) and prefer the control scheme which allow you:

* rotating around the CENTRE of the SELECTED OBJECT
* panning the view SIMULTANOUSLY

like which in Maya, this is what you need.

## USAGE:

Download the source and libraries of Blender from official Blender Git and SVN repositories. (Details can be found in [there](https://wiki.blender.org/index.php/Dev:Doc/Building_Blender)). Replace "view3d_edit.c" with file provided in this repository. Compile and run.

After Blender started, in User Preference -> Input pannel, change Navigation Style to "Orbit".

## What happened?

The code handles NDOF orbit and zoom is in this function:

```
static int ndof_orbit_zoom_invoke(bContext *C, wmOperator *op, const wmEvent *event)
```


Specifically, in this part:

```
else if ((U.ndof_flag & NDOF_MODE_ORBIT) ||
            ED_view3d_offset_lock_check(v3d, rv3d))
{
    const bool has_rotation = NDOF_HAS_ROTATE;
    const bool has_zoom = (ndof->tvec[2] != 0.0f);

    if (has_zoom) {
        view3d_ndof_pan_zoom(ndof, vod->sa, vod->ar, false, has_zoom);
    }

    if (has_rotation) {
        view3d_ndof_orbit(ndof, vod->sa, vod->ar, vod);
    }
}
```

This piece of code handles the "Orbit" style movement. In original code, it only allows rotating and zooming. So we need to modify this part of the code.

In our version "view3d_edit.c" file, we changed it to this to made it can translate, rotate and zoom the view simultanously:

```
else if ((U.ndof_flag & NDOF_MODE_ORBIT) ||
            ED_view3d_offset_lock_check(v3d, rv3d))
{
    const bool has_rotation = NDOF_HAS_ROTATE;
    const bool has_translate = NDOF_HAS_TRANSLATE;
    const bool has_zoom = (ndof->tvec[2] != 0.0f) && !rv3d->is_persp;
    
    float dist_backup;
    if (has_rotation) {
        view3d_ndof_orbit(ndof, vod->sa, vod->ar, vod);
    }
    if (has_translate || has_zoom) {
        view3d_ndof_pan_zoom(ndof, vod->sa, vod->ar, has_translate, has_zoom);
    }
    
    dist_backup = rv3d->dist;

    ED_view3d_distance_set(rv3d, dist_backup);
}
```

## Known Problems

### Add-on Archimesh
The add-on "Archimesh" are not working correctly when update the room, e.g. change Number of Walls. (It seems that it is not my problem :-P . Details can be found in [there](https://developer.blender.org/T50632))
