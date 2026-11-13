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

Continue with [Deployment](deployment.md) after preparing a trained policy.
