# Preprocessing

Run these commands from the `vla-framework` repository root. They use full
script paths so the working directory is unambiguous.

## Copy recordings between computers

For example, copy recordings from a workstation named `thor`:

```bash
mkdir -p dataset/rerun_recordings/advanced-control
rsync -avh --progress \
  thor:~/Documents/vla-framework/dataset/rerun_recordings/advanced-control/piper_pickup_mug_to_tray \
  dataset/rerun_recordings/advanced-control/
```

Replace the host and dataset name to match your collection machine.

## Convert Rerun recordings to LeRobot

```bash
pixi run python python/preprocessing/rerun_to_lerobot.py \
  --dataset_dir "$PWD/dataset" \
  --repo_id advanced-control/piper_pickup_mug_to_tray \
  --fps 15
```

| Argument | Default | Meaning |
| --- | --- | --- |
| `--dataset_dir` | `/home/robot/Documents/vla-framework/dataset` | Base dataset directory. |
| `--repo_id` | `advanced-control/piper_pickup_mug_to_tray` | Repository ID used to derive both paths. |
| `--fps` | `15` | Positive integer for output frames per second. |

Input: `{dataset_dir}/rerun_recordings/{repo_id}`.

Output: `{dataset_dir}/lerobot_dataset/{repo_id}`.

The converter uses hand, chest, and top camera streams, the `ros_time` index,
joint command positions as actions, and joint state positions as observations.

## Resize dataset videos

```bash
pixi run python python/preprocessing/resize.py \
  --dataset_dir "$PWD/dataset" \
  --repo_id advanced-control/piper_pickup_mug_to_tray \
  --width 640 \
  --height 360 \
  --crf 18
```

| Argument | Default | Meaning |
| --- | --- | --- |
| `--dataset_dir` | `/home/robot/Documents/vla-framework/dataset` | Base dataset directory. |
| `--repo_id` | `advanced-control/piper_pickup_mug_to_tray` | Source dataset repository ID. |
| `--width` | `640` | Positive integer output width in pixels. |
| `--height` | `360` | Positive integer output height in pixels. |
| `--crf` | `18` | H.264 quality factor from 0 to 51; lower values retain more quality. |
| `--replace` | Off | Replace the source dataset after resizing and validation succeed. |

Choose even output dimensions for the script's H.264 `yuv420p` encoding.

Without `--replace`, the script creates
`{dataset_dir}/lerobot_dataset/{repo_id}_resized`. It copies non-video data,
resizes the videos, checks frame counts, and updates video metadata.

To put the resized dataset back at the original path:

```bash
pixi run python python/preprocessing/resize.py \
  --dataset_dir "$PWD/dataset" \
  --repo_id advanced-control/piper_pickup_mug_to_tray \
  --width 640 --height 360 --crf 18 \
  --replace
```

!!! warning "Replacement removes the original videos"
    On success, `--replace` replaces the original dataset and removes its
    temporary backup. Keep a separate copy if you need the original resolution.

An existing `_resized` output causes the script to stop. Replacement also stops
if a `_backup` path already exists. If processing fails, the source remains
unchanged; a partial `_resized` directory may remain for inspection. If installing
the resized directory fails, the script attempts to restore the original path.

## Upload a dataset to Hugging Face

Authenticate with an account that can write to the destination repository.
See the [Hugging Face Hub authentication guide](https://huggingface.co/docs/huggingface_hub/quick-start#authentication).

```bash
pixi run python python/preprocessing/push_to_hub.py \
  --dataset_dir "$PWD/dataset" \
  --repo_id advanced-control/piper_pickup_mug_to_tray
```

| Argument | Default | Meaning |
| --- | --- | --- |
| `--dataset_dir` | `/home/robot/Documents/vla-framework/dataset` | Base dataset directory. |
| `--repo_id` | `advanced-control/piper_test` | Local dataset ID and destination Hub repository ID. |

The uploader reads `{dataset_dir}/lerobot_dataset/{repo_id}`. Pass the intended
repository ID explicitly: its default differs from the converter's default.
To upload a separate resized output, use a repository ID ending in `_resized`.

Each script also supports `--help`.
