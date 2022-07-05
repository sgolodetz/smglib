# smglib

This is the open-source Python framework associated with our drone research in the [Cyber-Physical Systems](https://www.cs.ox.ac.uk/activities/cyberphysical/) group at the University of Oxford.

### Development Installation

Note #1: Installing the framework currently takes a bit more effort than we would prefer (sorry!). We plan to address this in future.

Note #2: The installation instructions are currently for Windows, although there's no inherent reason why the framework can't work on Linux or Mac OS X. Again, we plan to address this in future.

1. Choose a root directory, hereafter (and in the submodule-level README files) referred to as `<root>`.

2. Clone the `smglib` repository into `<root>`, e.g.

```
git clone --recursive git@github.com:sgolodetz/smglib.git <root>
```

3. Install [Anaconda](https://www.anaconda.com).

4. Create a Conda environment for the framework, e.g.

```
conda create -n smglib python==3.7
```

5. Activate the Conda environment, e.g.

```
conda activate smglib
```

6. Follow the installation instructions specified in the submodule-level README files. Since some of the submodules depend on other submodules, the order in which the submodules are installed matters. One suitable installation order is:

- [smg-imagesources](https://github.com/sgolodetz/smg-imagesources/blob/master/README.md)
- [smg-joysticks](https://github.com/sgolodetz/smg-joysticks/blob/master/README.md)
- [smg-pyopencv](https://github.com/sgolodetz/smg-pyopencv/blob/master/README.md)
- [smg-rigging](https://github.com/sgolodetz/smg-rigging/blob/master/README.md)
- [smg-utility](https://github.com/sgolodetz/smg-utility/blob/master/README.md)
---
- [smg-open3d](https://github.com/sgolodetz/smg-open3d/blob/master/README.md) -> smg-utility
- [smg-opengl](https://github.com/sgolodetz/smg-opengl/blob/master/README.md) -> smg-rigging, smg-utility
- [smg-openni](https://github.com/sgolodetz/smg-openni/blob/master/README.md) -> smg-imagesources [optional, only needed if using OpenNI cameras]
- [smg-rotory](https://github.com/sgolodetz/smg-rotory/blob/master/README.md) -> smg-imagesources, smg-rigging
---
- [smg-mediapipe](https://github.com/sgolodetz/smg-mediapipe/blob/master/README.md) -> smg-open3d [optional, only needed if using the chair detector]
- [smg-meshing](https://github.com/sgolodetz/smg-meshing/blob/master/README.md) -> smg-open3d, smg-opengl
- [smg-skeletons](https://github.com/sgolodetz/smg-skeletons/blob/master/README.md) -> smg-opengl
---
- [smg-comms](https://github.com/sgolodetz/smg-comms/blob/master/README.md) -> smg-rigging, smg-skeletons, smg-utility

TODO

### Publications

If you build on this framework for your research, please cite the following paper:

```
@inproceedings{Golodetz2022TR,
author = {Stuart Golodetz and Madhu Vankadari* and Aluna Everitt* and Sangyun Shin* and Andrew Markham and Niki Trigoni},
title = {{Real-Time Hybrid Mapping of Populated Indoor Scenes using a Low-Cost Monocular UAV}},
booktitle = {IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
month = {October},
year = {2022}
}
```

### Acknowledgements

This work was supported by Amazon Web Services via the [Oxford-Singapore Human-Machine Collaboration Programme](https://www.mpls.ox.ac.uk/innovation-and-business-partnerships/human-machine-collaboration/human-machine-collaboration-programme-oxford-research-pillar), and by UKRI as part of the [ACE-OPS](https://gtr.ukri.org/projects?ref=EP%2FS030832%2F1) grant. We would also like to thank [Graham Taylor](https://www.biology.ox.ac.uk/people/professor-graham-taylor) for the use of the Wytham Flight Lab, [Philip Torr](https://eng.ox.ac.uk/people/philip-torr/) for the use of an Asus ZenFone AR, and [Tommaso Cavallari](https://uk.linkedin.com/in/tcavallari) for implementing TangoCapture.