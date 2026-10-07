# SO-101 Robot-Learning Roadmap

## Project objective

Build one coherent SO-101 robot-learning platform rather than a collection of
unrelated demonstrations.

The same robot and progressively expanding benchmark should be used to study
classical control, reinforcement learning, imitation learning, VLA policies,
sim-to-real transfer, self-supervised learning, world models, and adaptation.

## Stage 0 — Reaching

Build a minimal SO-101 reaching benchmark.

Start with a state-based environment.

Initial methods:

1. classical target geometry / inverse kinematics / joint control;
2. PPO using privileged state;
3. PPO using vision;
4. SAC comparison later.

Initial state observation concept:

`[joint position, joint velocity, target position]`

Initial action concept:

relative joint-position commands.

Later perturbations:

- camera noise
- lighting
- actuator strength
- latency

Key questions:

- How easily can the task be learned?
- What reward and action representations work well?
- What is lost when replacing privileged state with vision?
- How robust are policies to controlled perturbations?

Do not introduce all Stage 0 components at once. First establish a simple,
debuggable state-based environment.

## Stage 1 — Reach and push

Extend the benchmark to object interaction.

Task:

push an object to a target.

Add a classical model-predictive-control baseline.

Compare:

- PPO
- SAC
- classical MPC

Study:

- contact dynamics
- planning horizon
- model mismatch
- replanning
- robustness

## Stage 2 — Physical pick/place and imitation learning

Move to the physical SO-101 platform.

Task concept:

object at randomized location -> randomized container or goal.

Add teleoperation and synchronized data collection.

Train imitation-learning baselines such as:

- behavioral cloning
- ACT

Maintain common evaluation metrics across simulation and reality where
possible.

## Stage 3 — Reality-gap benchmark

Characterize the physical system instead of treating sim-to-real failure as a
single undifferentiated problem.

Measure or estimate:

- command latency
- motor response
- joint noise
- repeatability
- backlash/deadband
- camera calibration/extrinsics
- visual shift
- friction
- payload effects

Compare:

1. nominal simulation;
2. calibrated simulation;
3. domain-randomized simulation.

Deploy selected simulation policies to the physical robot.

Use synchronized logging and an explicit failure taxonomy.

## Stage 4 — Language-conditioned manipulation / VLA

Introduce language-conditioned multi-object manipulation.

Fine-tune an existing pretrained VLA rather than attempting to train a large
VLA from scratch.

Compare VLA behavior with imitation-learning baselines such as ACT.

Measure deployment latency in addition to task success.

## Stage 5 — Self-supervised learning and world models

Use accumulated simulated and physical robot data for representation and
dynamics learning.

Possible directions:

- self-supervised visual representation learning
- predictive latent models
- latent dynamics
- learned world models
- representation probes
- learned-model planning with MPC

The goal is to reuse data accumulated by earlier stages rather than creating a
separate disconnected project.

## Stage 6 — Shared algorithm comparison

Compare methods under common tasks and metrics:

- IK / PID / geometric control
- classical MPC
- PPO
- SAC
- behavioral cloning
- ACT
- VLA
- learned-model MPC
- TD-MPC2
- Dreamer-style approaches where appropriate

Avoid claiming that methods are directly comparable unless observations,
actions, task definitions, and evaluation conditions make the comparison
meaningful.

## Stage 7+ — Rapid adaptation and unseen dynamics

Explore adaptation to previously unseen dynamics and task conditions.

Possible directions:

- context encoders
- latent system identification
- active system identification
- meta-learning
- fast online adaptation
- habit-versus-deliberation controllers

This is a later research direction and should not complicate early task
implementation.

## Optional late-stage tasks

Dynamic tasks such as throwing/tossing may eventually be useful for studying
timing, high-speed perception, learned dynamics, and latency.

They are not an early project priority.