# Usage Examples

Our *smglib* framework encompasses many different scripts, and so we've put together a number of usage examples to illustrate how different tasks can be achieved.

* In what follows, OHM denotes the *Oxford Hybrid Mapping* dataset.
* You can find the scripts by name in PyCharm using `Navigate -> File...` (they're mostly in `smg-rescueflight`).

### Visualising an OHM sequence in 3D

```
run_vicon_visualiser.py --persistence_folder=<input sequence dir> --persistence_mode=input
```

### Capturing and evaluating a new sequence in the OHM format (requires a Vicon system and an Asus ZenFone AR)

* Step 1:

  ```
  run_vicon_visualiser.py --persistence_folder=<output sequence dir> --persistence_mode=output --run_server
  run_height_based_metric_drone_client.py -t tello -r --output_dir=<output sequence dir> --save_scale
  ```

* Step 2:

  ```
  calculate_vicon_from_world.py -s <output sequence dir> --save
  ```

* Step 3:

  ```
  run_mask_rcnn_skeleton_detection_service.py
  run_lcrnet_skeleton_detection_service.py -p 7853
  run_open3d_mapping_server.py -p wait --debug --detect_skeletons --max_depth=4.0 --voxel_size=0.025 --save_reconstruction --output_dir=<output sequence dir>/reconstruction --reconstruction_filename=world_mesh.ply
  run_vicon_disk_client.py -s <output sequence_dir> --use_tracked_poses --use_vicon_scale
  ```

* Step 4:

  ```
  run_lcrnet_skeleton_detection_service.py
  run_lcrnet_skeleton_detection_service.py -p 7853
  run_octomap_mapping_server.py -p wait --detect_skeletons --max_depth=4.0 --octree_voxel_size=0.05 --save_skeletons --output_dir=<output sequence dir>/people/lcrnet
  run_vicon_disk_client.py -s <output sequence dir> --use_tracked_poses --use_vicon_scale
  ```

  (+ analogous for XNect with the obvious changes)

* Step 5 (requires a specific branch of [SemanticPaint](https://github.com/sgolodetz/spaint/tree/smglib-wytham)):

  ```
  associate_hacked.py --max_difference 1 <gt sequence dir>/depth.txt <gt sequence dir>/rgb.txt > <gt sequence dir>/associations.txt
  extract_poses.exe <gt sequence dir>
  spaintgui.exe -s <gt sequence dir>/frames -t Disk --renderFiducials --relocaliserType=none --saveMeshOnExit --saveFiducialsOnExit
  ```

* Step 6:

   ```
   evaluate_metric_reconstruction.py -s <output sequence dir> --gt_render_style=uniform --reconstruction_render_style=uniform
   ```

* Step 7:

  TODO: CloudCompare

* Step 8:

  ```
  evaluate_skeleton_sequence_vs_vicon.py -s <output sequence dir>
  ```
