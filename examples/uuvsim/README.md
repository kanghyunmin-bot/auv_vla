# MuJoCo UUV integration (`uuvsim` branch)

Use the matching `uuvsim` branch of `kanghyunmin-bot/auv_vla_data_collector`.
It holds the existing simulator training/serving wrappers, external transfer
preprocessor, optional experimental samplers and span-400 checkpoint settings.
The model's real-vehicle PR remains on `codex/real-training-integrity`.

`kmu26_auv_vla_policy/` is the standalone ROS package used by the simulator. Build
it alongside the collector in a ROS workspace. Its explicit `sim_policy.yaml`
uses ROS simulation time, span 400, command_limit 1.0 and optional raw IMU motion;
it retains dry-run as the default. These values are not physical-vehicle defaults.

Example launch, after building and sourcing that workspace:

```bash
ros2 launch kmu26_auv_vla_policy sim_policy.launch.py
```

Run model training/serving through the collector branch's
`tools/finetune_transfer.py` and `tools/serve_transfer.py`, providing this model
checkout as `u0_root`. The serving wrapper finds this standalone adapter here.
Use existing compatible simulator checkpoints with their external
`Kmu26TransferDataConfig`; do not relabel them as `kmu26_auv_real_v2` checkpoints.

Only source/configuration is preserved: no recorded data, model weights, physics
scenes, GUI or residual PPO pipeline is included. Source provenance is recorded
in `../../docs/UUVSIM_SNAPSHOT.json`. Package/unit checks do not establish a new
full simulator rollout or task success.
