# VLA Framework

A guide to collecting robot demonstrations, preparing LeRobot datasets,
training policies, and running the deployment components in `vla-framework`.

## Workflow

| Step | Guide | What you will do |
| --- | --- | --- |
| 1 | [Setup](setup.md) | Locate the workspace and check the required environment. |
| 2 | [Data Collection](data-collection.md) | Bring up Mira, teleoperate Piper, and record demonstrations. |
| 3 | [Preprocessing](preprocessing.md) | Convert Rerun recordings, resize videos, and upload datasets. |
| 4 | [Dataset Tools](dataset-tools.md) | Edit task descriptions, split datasets, and merge datasets. |
| 5 | [Training](training.md) | Run ACT or multi-task DiT training. |
| 6 | [Deployment](deployment.md) | Launch camera decoding and policy inference with ROS parameters. |

## Workspace conventions

Application commands in these guides run from the **`vla-framework` repository
root**, unless a section says otherwise. Documentation preview commands run
from the separate **`vla-framework-docs` repository**.

Examples use `advanced-control/piper_pickup_mug_to_tray` as a dataset repository
ID. Replace it with your own `namespace/dataset_name` when using the guides.

These pages are based on the current project README, preprocessing scripts,
training examples, and ROS launch files.
