# General Motion Retargeting (GMR)

LimX Dynamics' fork of [GMR](https://github.com/YanjieZe/GMR). It retargets human motion capture onto
the TRON-family humanoids `limx_oli_edu` and `limx_luna`, and exports the motion in the formats
[BeyondMimic](https://github.com/HybridRobotics/whole_body_tracking) and
[luna-beyondmimic](https://github.com/limxdynamics/luna-beyondmimic) read.

## Install from a checkout

The wheel carries the Python package only: the robot models under `assets/`, the IK configs and the
`scripts/` entry points are repository files, so this is installed from source.

```bash
git clone https://github.com/limxdynamics/GMR.git
cd GMR
conda create -n gmr python=3.10 -y
conda activate gmr
pip install -e .
conda install -c conda-forge libstdcxx-ng -y
git submodule update --init --recursive   # robot descriptions for the two LimX models
```

## Run a retarget

```bash
# SMPL-X to a robot
python scripts/smplx_to_robot.py --smplx_file <path.npz_or.pkl> --robot limx_oli_edu \
  --save_path output/lx_motion.npz

# BVH to a robot
python scripts/bvh_to_robot.py --bvh_file <path.bvh> --robot limx_luna --format lafan1 \
  --save_path output/luna_motion.npy

# Look at the result
python scripts/vis_robot_motion.py --robot limx_luna --robot_motion_path output/luna_motion.npy
```

`--save_path` picks the export schema by extension: `.npy` writes the luna-beyondmimic dict
(`body_states`, `dof_pos_vel`, `dof_names`, `body_names`, `fps`; quaternions xyzw), while `.npz` and
`.pkl` write the BeyondMimic key layout (`joint_pos`, `body_pos_w`, `body_quat_w` wxyz, ...).

## Documentation

- Fork README - installation, both LimX robots, input formats, export schemas:
  https://github.com/limxdynamics/GMR#readme
- Upstream manual - robot support table, the other capture pipelines, PICO streaming, benchmark,
  citation: https://github.com/limxdynamics/GMR/blob/master/docs/upstream.md
- IK config parameters: https://github.com/limxdynamics/GMR/blob/master/DOC.md

## License

MIT. `LICENSE` carries both copyright lines (Yanjie Ze, 2025; LimX Dynamics, 2026); the third-party
robot assets and their licences are listed in `THIRD_PARTY_NOTICES`.
