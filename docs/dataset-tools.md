# Dataset Tools

The application preprocessing guide includes LeRobot dataset editing commands.
Run these tools in the application's configured Pixi environment.

## Modify task descriptions

Assign one task description across a dataset:

```bash
pixi run lerobot-edit-dataset \
  --repo_id dididog/piper_screw_driver_green \
  --root ~/lerobot_datasets/piper_screw_driver_green \
  --operation.type modify_tasks \
  --operation.new_task "Pick up the green-handled screwdriver and put it into a holder" \
  --push_to_hub true
```

Assign descriptions to individual episodes:

```bash
pixi run lerobot-edit-dataset \
  --repo_id dididog/piper_pick_up_bottle \
  --root ~/lerobot_datasets/piper_pick_up_bottle \
  --operation.type modify_tasks \
  --operation.episode_tasks '{"0": "Task A", "1": "Task B", "2": "Task A"}' \
  --push_to_hub true
```

## Split a dataset

Specify zero-based episode indices for each output split. Adapt this small
example to the episodes and tasks in your dataset:

```bash
pixi run lerobot-edit-dataset \
  --repo_id dididog/piper_pick_up_bottle \
  --operation.type split \
  --operation.splits '{"task1": [0, 2, 4], "task2": [1, 3, 5]}' \
  --push_to_hub true
```

## Merge datasets

Choose a destination repository ID and list the source repositories:

```bash
pixi run lerobot-edit-dataset \
  --repo_id dididog/piper_pick_up_bottle \
  --new_repo_id dididog/piper_pick_up_bottle_merged \
  --operation.type merge \
  --operation.repo_ids "['dididog/piper_pick_up_bottle_task1', 'dididog/piper_pick_up_bottle_task2', 'dididog/piper_pick_up_bottle_task3', 'dididog/piper_pick_up_bottle_task4']" \
  --push_to_hub true
```

These commands upload their changes because `--push_to_hub` is enabled.
Confirm the repository IDs and episode selections before running them.

For the tool's full options, see the
[LeRobot dataset tools documentation](https://huggingface.co/docs/lerobot/main/en/using_dataset_tools).
