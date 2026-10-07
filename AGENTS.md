# AGENTS.md

## Repository purpose

This repository is the long-term SO-101 robot-learning research platform.

The goal is to build one progressively expanding SO-101 benchmark covering
reinforcement learning, classical control/planning, imitation learning,
sim-to-real, multimodal/VLA policies, self-supervised learning, learned world
models, and eventually rapid adaptation.

Before making substantial changes, read:

- `docs/PROJECT_CONTEXT.md`
- `docs/ARCHITECTURE.md`
- the relevant section of `docs/ROADMAP.md`

## Current milestone

The current milestone is the first `reach` task.

Start with a minimal state-based SO-101 reaching environment.

The initial environment should contain only what is required for reaching:

- SO-101 robot
- target
- ground/work surface if needed
- lighting
- relative joint-position actions
- state observations
- modular rewards, events, and terminations

Do not prematurely add:

- grasping
- vial/rack logic
- cameras
- imitation learning
- ACT
- VLA
- ROS 2
- physical-robot integration
- world models
- unnecessary abstractions

The state-based reach environment should work and be validated before vision
or more complex manipulation is added.

## Isaac Lab

This is an external Isaac Lab project created from the official Blank template.

Use manager-based Isaac Lab environments.

Isaac Lab is pinned to the exact Git commit documented in
`docs/PROJECT_CONTEXT.md` and `pyproject.toml`.

Do not change the Isaac Lab revision or silently upgrade dependencies unless
explicitly requested.

When Isaac Lab API behavior is uncertain:

1. inspect the installed pinned Isaac Lab package;
2. consult the current official Isaac Lab documentation;
3. prefer behavior compatible with the pinned revision.

Primary documentation:

- https://isaac-sim.github.io/IsaacLab/develop/source/developer-tools/template_generator.html
- https://isaac-sim.github.io/IsaacLab/develop/source/how-to/index.html

The old SO-101 vial-placement tutorial may be used as an SO-101-specific
reference, but it should not define this project's architecture.

## Project structure

Task-wide logic belongs under:

`src/so101_robotics/tasks/<task>/`

Reusable MDP terms belong under:

`src/so101_robotics/tasks/<task>/mdp/`

SO-101-specific configuration belongs under:

`src/so101_robotics/tasks/<task>/config/so101/`

Keep tasks modular so later tasks can reuse appropriate components without
creating unnecessary coupling.

## Development environment

The authoritative development environment is the Google Cloud VM described in
`docs/PROJECT_CONTEXT.md`.

Run commands from the repository root:

`/home/hvonkent/so101_robotics`

Use:

`uv run ...`

Do not rely on system Python for project execution.

Useful checks:

`uv run pytest -q`

`uv run isaaclab list_envs`

`uv run python -c "import isaaclab; print(isaaclab.__file__)"`

Before declaring a coding task complete:

1. run relevant tests;
2. run formatting/lint checks where appropriate;
3. verify environment registration if registration changed;
4. report failures instead of hiding or bypassing them.

## Git and artifacts

Main branch:

`main`

GitHub repository:

`https://github.com/hvonkent/so101-robotics`

Make focused changes.

Do not commit:

- `.venv`
- simulator caches
- datasets
- large checkpoints
- training logs
- generated videos
- credentials
- tokens
- private keys
- cloud secrets

Source code, configuration, tests, documentation, and small reproducibility
artifacts belong in Git.

Large/private experiment artifacts will use Google Cloud Storage.

## Safety

Avoid broad destructive filesystem commands.

Do not use commands such as `rm -rf ~` or broad deletion wildcards.

Prefer explicit paths and inspect deletion targets before removing anything.

Do not modify files outside the repository unless the task explicitly requires
it.

## Design priorities

Prioritize:

1. reproducibility;
2. debuggability;
3. clean evaluation;
4. shared interfaces across algorithms where scientifically appropriate;
5. sim-to-real relevance;
6. modularity for future research stages.

Avoid unnecessary abstractions before there is a demonstrated reuse case.