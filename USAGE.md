# Usage Examples

Our *smglib* framework encompasses many different scripts, and so we've put together a number of usage examples to illustrate how different tasks can be achieved. We hope to add more examples gradually over time.

* In what follows, OHM denotes the *Oxford Hybrid Mapping* dataset.
* You can find the scripts by name in PyCharm using `Navigate -> File...` (they're mostly in `smg-rescueflight`).

### Common Tasks

#### Reconstruct a GTA-IM scene (online)

*Requires: Just the GTA-IM dataset*

See `reconstruct_gta_im_scene_online.sh`. Example:

```
reconstruct_gta_im_scene_online.sh "FPS-5/2020-06-09-17-14-03" batch gt gt --max_depth=10.0 --octree_voxel_size=0.2
```

#### Reconstruct an OHM scene (online)

*Requires: Just the OHM dataset*

See `reconstruct_ohm_scene_online.sh`. Example:

```
reconstruct_ohm_scene_online.sh single1 batch maskrcnn lcrnet --max_depth=4.0 --octree_voxel_size=0.05
```

#### Visualise an OHM sequence in 3D

*Requires: Just the OHM dataset*

```
run_vicon_visualiser.py --persistence_folder=<input sequence dir> --persistence_mode=input
```

### Less Common Tasks

#### Capture and evaluate a new sequence in the OHM format

*Requires:* Asus ZenFone AR, Drone, Futaba T6K, TangoCapture, Vicon System

*Note #1:* This is more of an aide-mémoire, as end users are unfortunately quite unlikely to have all of the hardware and software necessary to do this.

*Note #2:* If you want to use the automated scripts (recommended), then choose a name for the sequence and set the output sequence directory to e.g. `C:/datasets/ohm/<sequence name>`. The rest of these instructions assume you've done this (if not, you'll need to dig into the scripts accordingly).

* **Step 1: Capture the drone sequence**

  ```
  run_vicon_visualiser.py --persistence_folder=<output sequence dir> --persistence_mode=output --run_server
  run_height_based_metric_drone_client.py -t tello -r --output_dir=<output sequence dir> --save_scale
  ```

* **Step 2: Reconstruct the drone sequence**

  ```
  reconstruct_ohm_sequence.sh <sequence name>
  ```

* **Step 3: Capture the ground-truth sequence**

  You'll need an Asus ZenFone AR for this, and a copy of TangoCapture. You'll then need to copy it across to the PC using `adb pull`.

* **Step 4: Reconstruct the ground-truth sequence**

  *Note:* This uses scripts from the `smglib-wytham` branch of [SemanticPaint](https://github.com/sgolodetz/spaint/tree/smglib-wytham), which you'll need to build separately.

  ```
  associate_hacked.py --max_difference 1 <gt sequence dir>/depth.txt <gt sequence dir>/rgb.txt > <gt sequence dir>/associations.txt
  extract_poses.exe <gt sequence dir>
  spaintgui.exe -s <gt sequence dir>/frames -t Disk --relocaliserType=none --saveMeshOnExit
  ```

  Rename the resulting PLY file to `mesh.ply`, and copy it into `<output sequence dir>/gt`.

* **Step 5: Align the ground-truth mesh with the drone mesh, and evaluate the drone mesh**

  Align `<output sequence dir>/gt/mesh.ply` with `<output sequence dir>/reconstruction/world_mesh.ply` using [CloudCompare](https://www.danielgm.net/cc), and save the result as `<output sequence dir>/gt/world_mesh.ply`. A video showing how to do this can be found [here](https://www.doc.ic.ac.uk/~ahanda/VaFRIC/living_room.html), as well as a script called `computeStats.py` that can be used to compute the evaluation metrics for the drone mesh as per the video. Since we use C2C distances rather than C2M ones, we include a tweaked version of this script that uses C2C distances and also runs under Python 3. (The copyright for this script clearly still belongs to Thomas Whelan, as per the notice in the file.)

* **Step 6: Make the Vicon-space versions of the meshes**

   ```
   evaluate_metric_reconstruction.py -s <output sequence dir> --gt_render_style=uniform --reconstruction_render_style=uniform
   ```

  *Note:* This is only really needed to make the `vicon_mesh.ply` files that are required by our Vicon visualiser.

* **Step 7: Run the 3D skeleton evaluation**

  ```
  evaluate_ohm_sequence.sh <sequence name>
  ```
