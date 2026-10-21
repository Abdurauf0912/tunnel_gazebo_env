# Tunnel Gazebo Environment

Gazebo (Classic) simulation environments for testing robot navigation and path planning underground:

- **tunnel**: a cave/tunnel mesh reconstructed from a real point cloud scan (see [Data source](#data-source)).
- **room_pillar**: a room-and-pillar mine layout.

Each environment comes with a world file, a static Gazebo model and a 2D occupancy map (0.05 m/px) for `map_server` / AMCL.

The meshes, worlds and maps were created by **Abdurauf Abdukodirov** and used in the article [*Evaluation of Global Path Planning Algorithms for Mobile Robots in Simulated Underground Mining Environments*](https://doi.org/10.3390/mining6020038) (Mining, 2026).

## Contents

```
models/tunnel/          Gazebo model: cave mesh reconstructed from a point cloud
models/room_pillar/     Gazebo model: room-and-pillar mine mesh
worlds/                 tunnel.world, room_pillar.world
maps/                   2D occupancy maps (tunnel.yaml, room_pillar.yaml)
launch/                 ROS launch files for each world
```

## Download

The meshes (~87 MB) are stored with [Git LFS](https://git-lfs.com). Install it before cloning:

```bash
sudo apt install git-lfs
git lfs install
git clone https://github.com/Abdurauf0912/tunnel_gazebo_env.git
```

## Usage

### As a ROS (catkin) package

Tested with ROS Noetic and Gazebo 11.

```bash
cd ~/catkin_ws/src
git clone https://github.com/Abdurauf0912/tunnel_gazebo_env.git
cd ~/catkin_ws && catkin_make && source devel/setup.bash

roslaunch tunnel_gazebo_env tunnel.launch
roslaunch tunnel_gazebo_env room_pillar.launch
```

The package exports its `models/` folder to `GAZEBO_MODEL_PATH`, so the models resolve automatically.

To load a map for navigation:

```bash
rosrun map_server map_server $(rospack find tunnel_gazebo_env)/maps/tunnel.yaml
```

### Gazebo only (without ROS)

```bash
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:$(pwd)/tunnel_gazebo_env/models
gazebo tunnel_gazebo_env/worlds/tunnel.world
```

### Using with your own robot

The worlds contain only the environment, so you can spawn any robot into them. For example, include `tunnel.launch` from your own launch file and spawn your robot's URDF with `gazebo_ros/spawn_model`. A spawn pose that works well inside the tunnel is `x: 21.22, y: -7.03, z: 0.09, yaw: -2.64`.

## Data source

The tunnel mesh was reconstructed from the ground-truth 3D point cloud (RIEGL terrestrial laser scan of the Reiche Zeche mine, Freiberg, Germany) published in the **SubSurfaceGeoRobo** dataset:

> Li, Jing; Benndorf, Jörg; Köhler, Christian; Loskot, Paulina (2025): SubSurfaceGeoRobo: A Comprehensive Underground Dataset for SLAM-based Geomonitoring with Sensor Calibration [dataset]. PANGAEA, https://doi.org/10.1594/PANGAEA.975532

The dataset is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The original point cloud is **not** included in this repository. Only the mesh derived from it (with my changes: surface reconstruction, cleanup and conversion to a Gazebo model), the worlds and the maps are distributed here. If you use the tunnel environment, please also cite the dataset above.

## License

Copyright (c) 2026 Abdurauf Abdukodirov.

The meshes, worlds, maps and launch files in this repository are licensed under the
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](LICENSE).
You may use, share and adapt them, including commercially, **as long as you give appropriate credit**, link to the license and state what changes you made.

**Suggested attribution:**
> "Tunnel Gazebo Environment" by Abdurauf Abdukodirov (https://github.com/Abdurauf0912/tunnel_gazebo_env), licensed under CC BY 4.0.

## Citation

These environments were developed for and used in the following article. If you use them in academic work, please cite it (see also [CITATION.cff](CITATION.cff) or GitHub's "Cite this repository" button):

> Abdukodirov, A.; Benndorf, J. Evaluation of Global Path Planning Algorithms for Mobile Robots in Simulated Underground Mining Environments. *Mining* **2026**, *6*(2), 38. https://doi.org/10.3390/mining6020038

```bibtex
@article{abdukodirov2026evaluation,
  author  = {Abdukodirov, Abdurauf and Benndorf, J{\"o}rg},
  title   = {Evaluation of Global Path Planning Algorithms for Mobile Robots in Simulated Underground Mining Environments},
  journal = {Mining},
  volume  = {6},
  number  = {2},
  pages   = {38},
  year    = {2026},
  doi     = {10.3390/mining6020038}
}
```

To refer to this repository itself:

```bibtex
@misc{tunnel_gazebo_env,
  author       = {Abdurauf Abdukodirov},
  title        = {Tunnel Gazebo Environment},
  year         = {2026},
  howpublished = {\url{https://github.com/Abdurauf0912/tunnel_gazebo_env}},
  note         = {Version 1.0.0}
}
```
