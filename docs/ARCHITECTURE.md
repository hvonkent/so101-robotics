# Architecture

## Core principle

This repository represents one progressively expanding SO-101 robot-learning
platform.

The objective is to compare and combine multiple robot-learning approaches on
shared tasks rather than creating a separate mini-project for every algorithm.

Where scientifically appropriate, methods should share:

- task definitions
- observation semantics
- action semantics
- evaluation infrastructure
- data formats
- logging conventions

The architecture should remain simple during early stages and grow only when
new requirements justify additional abstractions.

## Repository structure

The high-level repository structure is:

    so101_robotics/
    ├── AGENTS.md
    ├── docs/
    │   ├── PROJECT_CONTEXT.md
    │   ├── ROADMAP.md
    │   └── ARCHITECTURE.md
    ├── scripts/
    ├── src/
    │   └── so101_robotics/
    │       ├── assets/
    │       └── tasks/
    ├── tests/
    ├── pyproject.toml
    └── uv.lock

The repository is an external Isaac Lab project.

## Isaac Lab task organization

Tasks should be organized by task family.

The intended structure is approximately:

    src/so101_robotics/tasks/
    ├── reach/
    │   ├── __init__.py
    │   ├── mdp/
    │   │   └── ...
    │   └── config/
    │       └── so101/
    │           ├── __init__.py
    │           ├── env_cfg.py
    │           └── agents/
    │               └── ...
    ├── push/
    └── pick_place/

Task-wide logic belongs under:

`src/so101_robotics/tasks/<task>/mdp/`

SO-101-specific configuration belongs under:

`src/so101_robotics/tasks/<task>/config/so101/`

Agent configuration should follow Isaac Lab conventions and remain separate
from the core task definition where practical.

Reusable task logic should not be duplicated unnecessarily.

## Environment architecture

Use Isaac Lab manager-based environments.

For the first reach task, the conceptual environment is:

    SO101ReachEnvCfg
    │
    ├── SO101ReachSceneCfg
    │   ├── SO-101 robot
    │   ├── target
    │   ├── ground/work surface if required
    │   └── lighting
    │
    ├── ActionsCfg
    ├── ObservationsCfg
    ├── RewardsCfg
    ├── EventsCfg
    └── TerminationsCfg

The first implementation should remain deliberately minimal.

Do not add grasping, object manipulation, cameras, domain randomization, or
other later-stage components until the state-based reaching environment works
and can be evaluated reliably.

## Reach task

The initial task is reaching a target with the SO-101 end effector.

The environment should establish a reliable benchmark before introducing
vision or more complex manipulation.

The central task quantity is the distance between the end effector and the
target.

Later versions may introduce orientation objectives or additional constraints,
but these should not complicate the first implementation.

## Initial observations

The initial privileged-state observation concept is:

- joint positions
- joint velocities
- target position

The exact coordinate representation should be chosen deliberately when the
environment is implemented.

Additional observations should only be added when justified by the task or by
a specific experiment.

Avoid accidentally exposing privileged simulator information that would not
exist on the physical robot unless the experiment explicitly studies a
privileged-state policy.

A later vision version of reach will replace or supplement privileged target
state with camera observations.

## Initial actions

The initial action concept is:

- relative joint-position commands

The action interface should remain simple and interpretable.

It should support fair later comparison between multiple RL algorithms without
requiring a different task definition for every method.

More complex controllers should not be embedded into the task unless they are
deliberately part of the experimental condition.

## Rewards

Reward terms should be:

- individually understandable
- individually inspectable
- easy to ablate
- logged where useful

For reaching, end-effector distance to the target should be the primary task
quantity.

Avoid creating a large opaque reward function with many interacting terms
before the basic task works.

Potential supporting reward or penalty terms may later include quantities such
as:

- reaching accuracy
- action magnitude
- joint-limit proximity
- smoothness

Such additions should be justified experimentally rather than added by
default.

## Events and resets

Reset logic should initially remain simple.

The first reach environment should support randomized target positions within a
defined reachable workspace.

Robot initial state randomization should be introduced carefully enough that
failures can still be debugged.

Later stages may add controlled domain randomization for:

- actuator strength
- latency
- camera properties
- lighting
- dynamics parameters

These are experimental conditions, not requirements for the first environment
version.

## Terminations

The environment should have clearly defined episode termination conditions.

Possible initial conditions include:

- fixed maximum episode length
- optional success termination when sufficiently close to the target

Termination semantics should be consistent across algorithms used in the same
comparison.

## Evaluation

Evaluation is part of the architecture rather than an afterthought.

Relevant metrics should eventually include appropriate subsets of:

- success rate
- final target distance
- minimum target distance
- episode length
- action magnitude
- joint-limit violations
- failure reason
- perturbation parameters
- random seed
- environment configuration
- policy/checkpoint identifier
- Git commit

The goal is to make experimental conditions reproducible and failures
diagnosable.

## Reproducibility

Experiments should record enough information to determine:

- which source revision was used
- which environment configuration was used
- which algorithm configuration was used
- which random seed was used
- which checkpoint produced the result

The exact Isaac Lab revision is pinned in the project dependency configuration
and should not be silently changed.

## Algorithm separation

The environment should describe the task.

Algorithm-specific code and configuration should remain separate from core task
logic where possible.

This allows the same environment to support:

- classical control
- PPO
- SAC
- imitation learning
- VLA evaluation
- learned-model approaches

without redefining the task for every method.

The purpose of this separation is not to force every algorithm into an
identical implementation, but to preserve a common experimental task wherever
a comparison is meaningful.

## Classical control and planning

Classical approaches should be treated as first-class baselines rather than
temporary debugging tools.

The project is expected to eventually include methods such as:

- geometric target processing
- inverse kinematics
- joint control
- PID-style control where relevant
- model predictive control

These should use the same task and evaluation definitions where possible.

## Reinforcement learning

The first RL baseline is expected to use PPO with RSL-RL.

SAC will be added later after the task and PPO baseline are validated.

RL algorithm configuration should not be mixed unnecessarily into the
environment definition.

The environment should remain usable by multiple learning libraries where
practical.

## Vision

The first reach environment is state-based.

Camera observations should be introduced only after the state-based benchmark
is working.

This allows the project to distinguish:

- task-design problems
- control problems
- RL problems
- perception problems

rather than debugging all of them simultaneously.

Later vision experiments should preserve the same underlying reaching task so
that state-versus-vision comparisons remain meaningful.

## Simulation-to-real architecture

Simulation and physical execution should eventually share conceptual
interfaces where practical:

    observation
        ↓
    policy/controller
        ↓
    action
        ↓
    robot/environment
        ↓
    next observation + logging

Simulation and hardware do not need identical implementations.

However, observations and actions should preserve comparable semantics where
possible so that sim-to-real differences can be measured rather than hidden by
unrelated interface changes.

## Physical robot integration

Physical SO-101 integration will be added later.

Initial physical control will remain Python-first.

ROS 2 may later provide:

- node separation
- message transport
- rosbag recording
- coordinate transforms
- visualization
- motion-planning integration

C++ should be introduced only for components where hardware integration,
high-rate execution, safety, or performance justify it.

ML training and most research code should remain Python-first.

## Physical robot logging

Physical experiments should eventually record synchronized:

- timestamps
- commanded actions
- measured joint states
- camera frames
- controller or policy outputs
- task state where measurable
- latency information
- episode outcome
- failure category

This is important for analyzing the reality gap rather than treating a failed
sim-to-real deployment as a single unexplained failure.

## Data architecture

Small project files belong in Git.

Examples include:

- source code
- configuration
- tests
- documentation
- small evaluation summaries

Large experimental artifacts do not belong in Git.

Large and/or private artifacts will later be stored in Google Cloud Storage.

A possible logical organization is:

    datasets/
        sim/
        real/
        teleop/

    checkpoints/
        ppo/
        sac/
        act/
        vla/
        world_models/

    experiments/

    videos/

    archives/

The exact bucket name and lifecycle policies will be defined when cloud
storage is provisioned.

## Experiment artifacts

The VM filesystem is active working storage, not the sole authoritative
archive.

Training outputs may be generated locally on the VM during experiments.

Important checkpoints, datasets, recordings, and experiment archives should
eventually be copied to the project's private cloud storage.

Source code and dependency definitions remain authoritative in GitHub.

## Long-term task progression

The architecture should support the project roadmap without attempting to
implement the entire roadmap during Stage 0.

The intended progression is approximately:

1. reach
2. push and model-based control
3. physical pick/place and imitation learning
4. reality-gap experiments
5. VLA experiments
6. self-supervised learning and world models
7. shared algorithm comparisons
8. rapid adaptation and unseen dynamics

Each stage should extend the existing platform rather than starting a
disconnected project.

## Software evolution

Prefer the simplest architecture that supports the current validated use case.

Add abstractions when there are real repeated patterns.

Do not design Stage 6 infrastructure during Stage 0 merely because it might be
useful later.

Refactoring is appropriate when actual reuse or complexity demonstrates a need
for it.