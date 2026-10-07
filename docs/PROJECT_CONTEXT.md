# Project Context

## Project identity

Project name:

`SO-101 Robotics`

Google Cloud project:

`so101-robotics`

GitHub repository:

`https://github.com/hvonkent/so101-robotics`

Git branch:

`main`

Python package:

`so101_robotics`

Repository path on development VM:

`/home/hvonkent/so101_robotics`

## Development environment

Development is performed primarily on a Google Cloud VM through VS Code
Remote SSH.

Google Cloud VM:

`so101-dev`

Google Cloud zone:

`us-east1-c`

Linux user:

`hvonkent`

The local PC is primarily a client for VS Code, SSH, and browser access.

The VM contains the active working copy, Python environment, simulator
installation, caches, and active training runs.

The VM should be treated as replaceable infrastructure rather than the sole
long-term source of project data.

## Python environment

Python requirement:

`>=3.12,<3.13`

Current project virtual environment:

`/home/hvonkent/so101_robotics/.venv`

Initial project Python version:

`3.12.15`

Package/environment manager:

`uv`

Initial installed uv version:

`0.12.23`

Run project commands using:

`uv run ...`

Do not assume that system Python contains project dependencies.

## Isaac Lab

This repository was created from the official Isaac Lab Blank external-project
template.

Isaac Lab source repository:

`https://github.com/isaac-sim/IsaacLab.git`

Pinned Isaac Lab Git commit:

`06128939c6f3264594483a73e4344e4fefcea6d3`

This commit was on Isaac Lab's `develop` branch when the project was created.

The dependency is installed through the Isaac Lab aggregate wheel builder:

`tools/wheel_builder`

The authoritative dependency declaration is in:

`pyproject.toml`

The resolved dependency graph is in:

`uv.lock`

The project must not silently track the moving Isaac Lab `develop` branch.

Any future Isaac Lab upgrade should be an explicit dependency-upgrade change
with tests and a dedicated Git commit.

The project currently imports Isaac Lab from its own `.venv`.

It does not depend on a sibling Isaac Lab source checkout.

## Isaac Lab project configuration

Project type:

external Isaac Lab project

Template:

Blank

Environment architecture:

manager-based

Task discovery is configured through:

`[project.entry-points."isaaclab.tasks"]`

The Python package registered with Isaac Lab is:

`so101_robotics.tasks`

The Blank project initially contains no registered RL environments. New tasks
must register their Gymnasium environments explicitly.

## Isaac Lab dependency setup

The project depends on the aggregate Isaac Lab package:

`isaaclab`

The Git source points to:

`https://github.com/isaac-sim/IsaacLab.git`

at revision:

`06128939c6f3264594483a73e4344e4fefcea6d3`

with subdirectory:

`tools/wheel_builder`

The project uses the generated Isaac Lab `uv` configuration and CUDA/PyTorch
dependency indexes preserved in `pyproject.toml`.

RSL-RL is the intended first PPO implementation.

SAC will be added later after the environment and PPO baseline are working.

Do not introduce an RL library merely for architectural symmetry.

## Validation of initial project

Before task development began:

`uv run pytest -q`

passed.

Isaac Lab imported from:

`/home/hvonkent/so101_robotics/.venv/lib/python3.12/site-packages/isaaclab/`

The temporary Isaac Lab source checkout used to generate the Blank project was
deleted after independence was verified.

The repository therefore does not depend on an external local Isaac Lab source
checkout.

## Primary documentation

Primary project-structure and API references:

- https://isaac-sim.github.io/IsaacLab/develop/source/developer-tools/template_generator.html
- https://isaac-sim.github.io/IsaacLab/develop/source/how-to/index.html

When documentation and the pinned implementation differ, the pinned source/API
used by this project is authoritative for runtime behavior.

The official SO-101 Isaac Lab tutorial may be used for SO-101-specific details
such as robot configuration, but not as the primary project architecture.

## Current research milestone

Current task family:

`reach`

Current robot:

SO-101

The first implementation is a state-based reaching environment.

Initial observation concept:

`[joint_position, joint_velocity, target_position]`

Initial action concept:

relative joint-position commands

The environment should be established and validated before algorithm
comparison begins.

Initial Stage 0 comparisons are intended to include:

- classical geometric / IK / joint-control baseline
- PPO with privileged state
- PPO with vision
- SAC later

Possible perturbations later in Stage 0 include:

- camera noise
- lighting variation
- actuator strength variation
- latency

These should not all be implemented in the first environment version.

## Software strategy

Python is the primary language for:

- Isaac Lab
- PyTorch
- reinforcement learning
- imitation learning
- VLA work
- world models
- data analysis
- experiment/evaluation tooling

ROS 2 will be introduced later for physical-robot integration.

C++ will be introduced only where hardware integration, high-rate control,
safety, or performance requirements justify it.

ML research code should remain Python-first.

## Storage model

GitHub is the authoritative source for:

- source code
- configuration
- tests
- documentation
- dependency definitions
- small evaluation summaries

The Google Cloud VM is used for:

- active checkout
- `.venv`
- simulator installation
- caches
- active experiment outputs

Google Cloud Storage will later be the authoritative location for large/private
artifacts such as:

- simulated datasets
- real-robot datasets
- teleoperation recordings
- checkpoints
- experiment archives
- raw videos

Large artifacts should not be committed to Git.

The desired recovery workflow is:

1. provision a new VM;
2. clone the GitHub repository;
3. run `uv sync`;
4. restore required artifacts from Google Cloud Storage;
5. continue development.

A local PC should not be required to contain the simulator or full project
dataset.