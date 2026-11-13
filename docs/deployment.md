# Deployment

## Launch the policy and camera decoders

Run from the `vla-framework` application repository root:

```bash
pixi run ros2 launch deploy deploy.launch.py \
  policy_type:=act \
  model:=advanced-control/act_piper_pickup_mug_any_to_any \
  repo:=advanced-control/piper_pickup_mug_any_to_any \
  task:="Pick up the mug and place it on the tray"
```

The launch file starts the `deployed_policy` node and FFmpeg image transport
decoders for the hand, top, and chest cameras. The policy publishes joint
commands when its required observations are available.

## Deployment parameters

The launch arguments map to ROS parameters with the same names. All four
parameters are strings and are read at startup. Restart the node to change
them; setting a parameter on an already running node does not reload the
policy or update the inference task.

| Parameter / launch argument | Default | Meaning |
| --- | --- | --- |
| `policy_type` | `act` | Policy architecture: `act` or `dit`. Other values are rejected. |
| `model` | `advanced-control/act_piper_pickup_mug_any_to_any` | Trained policy Hub repository ID or local pretrained checkpoint directory. |
| `repo` | `advanced-control/piper_pickup_mug_any_to_any` | Dataset Hub repository ID or local dataset directory, used to load metadata and normalization statistics. |
| `task` | `Pick up the green-handled screwdriver and put it into a holder` | Task description passed to policy inference. |

Pass the model, dataset, and task for the same experiment. The current DiT
wrapper uses the task text to prepare its observation; the ACT wrapper accepts
the task argument but does not use it for conditioning.

Inspect the available launch arguments without starting the robot nodes:

```bash
pixi run ros2 launch deploy deploy.launch.py --show-args
```

## Deploy a DiT policy

For example, use the model and dataset from the [DiT training example](training.md#multi-task-dit):

```bash
pixi run ros2 launch deploy deploy.launch.py \
  policy_type:=dit \
  model:=dididog/dit_piper_screw_driver_green \
  repo:=dididog/piper_screw_driver_green \
  task:="Pick up the green-handled screwdriver and put it into a holder"
```

For local artifacts, pass paths instead of Hub repository IDs:

```bash
pixi run ros2 launch deploy deploy.launch.py \
  policy_type:=act \
  model:="$PWD/policies/act/piper_pickup_mug_to_tray/checkpoints/last/pretrained_model" \
  repo:="$PWD/dataset/lerobot_dataset/advanced-control/piper_pickup_mug_to_tray" \
  task:="Pick up the mug and place it on the tray"
```

Adjust the checkpoint path to an existing pretrained model directory.

## Run the policy node directly

Use `--ros-args -p` to pass ROS parameters without the launch file:

```bash
pixi run ros2 run deploy deploy --ros-args \
  -p policy_type:=act \
  -p model:=advanced-control/act_piper_pickup_mug_any_to_any \
  -p repo:=advanced-control/piper_pickup_mug_any_to_any \
  -p task:="Pick up the mug and place it on the tray"
```

This starts only the policy node. Camera decoding must already be running, or
another process must provide the decoded image topics below.

## Camera and joint interfaces

Each decoder republishes `/{camera}/color/image_raw/ffmpeg` to
`/{camera}/color/image_raw/decoded`, using the configured H.264 or HEVC decoder
preferences. The collector resizes decoded images to 640 × 360 pixels before
inference.

| Direction | Topic | Message |
| --- | --- | --- |
| Input | `/piper_joint_states` | `sensor_msgs/msg/JointState` |
| Input | `/camera_hand/color/image_raw/decoded` | `sensor_msgs/msg/Image` |
| Input | `/camera_top/color/image_raw/decoded` | `sensor_msgs/msg/Image` |
| Input | `/camera_chest/color/image_raw/decoded` | `sensor_msgs/msg/Image` |
| Output | `/piper_joint_cmd` | `sensor_msgs/msg/JointState` |

The current wrappers use CUDA. Confirm the model, dataset metadata, image
dimensions, and joint ordering match the intended robot setup before running
inference.

See [Training](training.md) for the policy training examples and
[Preprocessing](preprocessing.md) for dataset preparation.
