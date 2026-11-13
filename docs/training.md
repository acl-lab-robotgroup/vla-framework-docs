# Training

The application training guide provides the following ACT and multi-task DiT
examples. Run from the application repository using its Pixi environment.
Adapt dataset IDs, policy IDs, and output directories for your experiment.

## ACT

```bash
pixi run lerobot-train \
  --dataset.repo_id=advanced-control/piper_pickup_mug_to_tray \
  --policy.type=act \
  --output_dir="$PWD/policies/act/piper_pickup_mug_to_tray" \
  --batch_size=8 \
  --job_name=act_piper_pickup_mug_to_tray \
  --policy.device=cuda \
  --wandb.enable=true \
  --policy.repo_id=advanced-control/act_piper_pickup_mug_to_tray
```

## Multi-task DiT

```bash
pixi run lerobot-train \
  --dataset.repo_id=dididog/piper_screw_driver_green \
  --output_dir="$PWD/policies/dit/piper_screw_driver_green" \
  --batch_size=128 \
  --steps=30000 \
  --policy.type=multi_task_dit \
  --policy.device=cuda \
  --policy.horizon=32 \
  --policy.n_action_steps=24 \
  --policy.objective=diffusion \
  --policy.noise_scheduler_type=DDPM \
  --policy.num_train_timesteps=100 \
  --policy.repo_id=dididog/dit_piper_screw_driver_green \
  --wandb.enable=true \
  --save_freq=5000 \
  --num_workers=2 \
  --save_checkpoint_to_hub=true
```

These examples enable Weights & Biases logging. Configure your logging account
or disable that option for local experiments. The DiT example also enables
checkpoint upload to the Hub.

If you resized into a separate `_resized` dataset and uploaded it under that ID,
use that repository ID for training. An existing experiment output directory
may require selecting a new path or configuring resume behavior.

## Video timestamp errors with merged datasets

Merged datasets may raise `FrameTimestampError` during training. Concatenated
videos can have small timestamp offsets that affect TorchCodec's approximate
frame seeking, while floating-point rounding can exceed a strict timestamp
tolerance. If this occurs, try adding both flags to the training command:

```bash
--dataset.video_backend=pyav \
--tolerance_s=0.001
```

`pyav` decodes forward from a preceding keyframe using actual frame timestamps,
avoiding the approximate seeking issue. It may be slower. `tolerance_s=0.001`
allows a 1 ms timestamp difference; set it explicitly because the training CLI
defaults to `0.0001` (0.1 ms).

If errors persist, inspect the video timestamps and dataset metadata. Keep the
tolerance well below one frame interval (`1 / fps`) so that it does not hide
missing frames or image/state/action misalignment.

Continue with [Deployment](deployment.md) after preparing a trained policy.
