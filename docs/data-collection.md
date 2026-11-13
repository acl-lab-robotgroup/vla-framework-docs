# Data Collection

## Bring up Mira

Enter the existing Docker container:

```bash
docker exec -it isaac-ros bash
```

Inside the container, launch the robot:

```bash
ros2 launch mobile_manipulator_bringup bringup.launch.py
```

## Teleoperate the Piper arm

Connect the Meta Quest and workstation to the same network. Check the
application's `ros_ws/src/teleop/config/teleop.yaml` for the network settings
and teleoperation mode before launching.

From the application repository in another terminal:

```bash
pixi run ros2 launch teleop teleop_bringup.launch.py
```

## Record demonstrations with Rerun

Use the same repository ID throughout recording, conversion, and upload:

```bash
pixi run ros2 run collect logger --ros-args \
  -p repo_id:="advanced-control/piper_pickup_mug_to_tray" \
  -p start_idx:=0 \
  -p recording_root_folder:="$PWD/dataset/rerun_recordings"
```

Recordings are saved under:

```text
{recording_root_folder}/{repo_id}/episode_{index}.rrd
```

!!! warning "Existing recordings can be overwritten"
    Choose the recording directory and starting episode index carefully when
    continuing a collection session.

Once collection is complete, continue with [Preprocessing](preprocessing.md).
