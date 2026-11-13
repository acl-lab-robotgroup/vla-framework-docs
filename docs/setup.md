# Setup

## Application workspace

The guides assume the application is checked out at:

```text
~/Documents/vla-framework
```

Enter the application repository before running the robot or dataset commands:

```bash
cd ~/Documents/vla-framework
```

## Environment

The application uses Pixi for its Python and ROS environment. Its `pixi.toml`
defines ROS Jazzy, LeRobot, Rerun, FFmpeg, PyTorch, and CUDA dependencies for
Linux x86-64 and ARM64. Use the application's configured environment for the
examples in this documentation.

The collection workflow also assumes:

- A configured Mira/Piper robot setup.
- An existing `isaac-ros` Docker container for robot bringup.
- A Meta Quest and workstation on the same network for teleoperation.
- Camera and joint-state streams available to the collection nodes.
- A CUDA-capable environment for the training examples.

Hardware provisioning, container creation, and driver installation depend on
the machine and are outside the existing project guide.

## ROS workspace

The application has a `build` Pixi task that runs
`colcon build --symlink-install` in `ros_ws/`:

```bash
pixi run build
```

Its Pixi activation configuration sources `ros_ws/install/setup.sh`. The
application examples therefore expect a configured, built ROS workspace.

## Dataset layout

The preprocessing scripts share this layout:

```text
dataset/
├── rerun_recordings/
│   └── namespace/dataset_name/
│       └── episode_0.rrd
└── lerobot_dataset/
    ├── namespace/dataset_name/
    └── namespace/dataset_name_resized/
```

`--dataset_dir` selects the `dataset/` base directory, and `--repo_id` selects
`namespace/dataset_name`. Without `--replace`, resizing creates a sibling
dataset with the `_resized` suffix.

Continue with [Data Collection](data-collection.md) or, if recordings already
exist, [Preprocessing](preprocessing.md).
