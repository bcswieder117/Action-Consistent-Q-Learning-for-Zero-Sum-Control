# Action-Consistent Q-Learning for Zero-Sum Control

**Learning reliable adversarial control policies when commanded and applied actions differ.**

**Author:** Blaine Christopher Swieder

**Project status:** Research Design and Implementation Planning

## Overview

This project investigates how actuator mismatch, action measurements, and action representation affect data-driven control in two-player zero-sum games.

A controller issues a command, an actuator applies an input, and a learning algorithm records an action. These quantities can differ because of persistent offsets, calibration errors, drift, or measurement noise.

The central research question is:

> How do differences between commanded, applied, and recorded actions affect Q-learning, policy recovery, and closed-loop performance?

The planned study combines quadratic Q-learning, calibration, representation analysis, and controlled numerical experiments. Proposed contributions describe work to be developed; they are not claims of completed results.

## Research Objectives

- Characterize how actuator mismatch affects quadratic Q-function estimation.
- Distinguish action-label errors from representation limitations and measurement noise.
- Identify the information and excitation conditions needed for offset estimation.
- Evaluate how calibration uncertainty propagates into learned policies.
- Compare compensation strategies under constant and changing actuator offsets.
- Develop reproducible experiments and a focused research manuscript.

## Initial Problem Formulation

The starting point is a discrete-time linear-quadratic zero-sum game:

```math
x_{k+1}
=
Ax_k
+
B_Pu^{\mathrm{act}}_{P,k}
+
B_Eu^{\mathrm{act}}_{E,k}
```

Commanded and applied inputs satisfy:

```math
u^{\mathrm{act}}_{i,k}
=
u^{\mathrm{cmd}}_{i,k}
+
b_{i,k},
\qquad i \in \{P,E\}
```

Here:

- $`x_k`$ is the system state.
- $`u^{\mathrm{cmd}}_{i,k}`$ is the commanded input.
- $`u^{\mathrm{act}}_{i,k}`$ is the applied input.
- $`b_{i,k}`$ represents actuator mismatch.
- $`P`$ and $`E`$ denote the minimizing and maximizing players.

Initial experiments will use constant offsets. Later experiments will introduce drift and changes in the offsets.

The stage cost is defined using applied inputs:

```math
\begin{aligned}
\ell_k
&= x_k^\top Qx_k \\
&\quad + \left(u^{\mathrm{act}}_{P,k}\right)^\top
R_Pu^{\mathrm{act}}_{P,k} \\
&\quad - \left(u^{\mathrm{act}}_{E,k}\right)^\top
R_Eu^{\mathrm{act}}_{E,k}.
\end{aligned}
```

The pursuer minimizes the objective, while the evader maximizes it.

Commanded actions can also provide valid action coordinates when the dynamics, cost, and function representation consistently account for actuator mismatch. This project will explicitly examine that alternative.

## Research Questions

1. How does command-versus-applied action mismatch affect Q-function estimation and policy recovery?
2. When does a homogeneous quadratic representation become insufficient?
3. Can affine-aware features account for persistent mismatch in commanded-action coordinates?
4. Which measurements are required to identify actuator offsets?
5. How do calibration length, measurement noise, and training-data size affect performance?
6. What happens when actuator bias changes after calibration?

## Planned Comparisons

| Configuration | Action information or representation | Purpose |
| --- | --- | --- |
| Command-only quadratic learner | Commanded actions with homogeneous quadratic features | Test mismatch under the nominal representation |
| Applied-action learner | Measured applied actions | Establish a reference under explicit measurement assumptions |
| Calibration-based learner | Applied-action estimates reconstructed from commands and estimated offsets | Evaluate finite calibration accuracy |
| Affine-aware command learner | Commanded actions with constant, linear, and quadratic features | Test representation-based accommodation of persistent mismatch |
| Model-based reference | Known dynamics and appropriately specified mismatch information | Provide reference policies and value functions |

Comparisons will distinguish algorithm choice from measurement access. Shared datasets and matched information will be used where possible.

## Experimental Plan

### Phase 1: Establish Baselines

- Implement and verify nominal GARE and quadratic Q-learning references.
- Check Riccati and Bellman residuals.
- Verify closed-loop stability and saddle-point conditions.
- Reproduce a constant-offset calibration benchmark.

### Phase 2: Isolate Action Representation

- Compare commanded, measured applied, and reconstructed applied actions.
- Compare homogeneous quadratic and affine-aware feature classes.
- Use shared transition datasets where appropriate.
- Separate noiseless consistency experiments from noisy-data experiments.

### Phase 3: Evaluate Uncertainty

- Sweep actuator-bias magnitude.
- Vary calibration length and training-data size.
- Vary exploration strength and feature conditioning.
- Introduce input-measurement noise.
- Repeat experiments across seeds and multiple admissible plants.
- Evaluate drift and post-calibration bias changes.

### Phase 4: Develop Analysis and Deliverables

- Derive conditions or bounds supported by the findings.
- Document failure cases and successful recovery.
- Package code, configurations, raw results, and figures.
- Prepare a manuscript centered on the established contribution.

## Evaluation Metrics

Planned metrics include:

- Q-function and value-function errors.
- Feedback-gain errors against suitable references.
- Training and held-out Bellman residuals.
- Feature rank and conditioning.
- Closed-loop spectral radius for linear policy evaluations.
- Offset-estimation error and uncertainty.
- State-trajectory deviation.
- Cumulative finite-window game cost.
- Runtime and data requirements under defined comparison conditions.

Stability and performance conclusions will be stated for the assumptions and evaluation horizon used in each experiment.

## Implementation Approach

The initial implementation plan uses MATLAB reference experiments.

The computational workflow will separate:

1. Plant and actuator simulation.
2. Data collection and action recording.
3. Calibration and action reconstruction.
4. Feature construction.
5. Policy learning.
6. Independent validation and plotting.

This separation will support controlled comparisons and make the information supplied to each learner explicit.

Entry-point names, dependencies, and execution commands will be documented as the implementation is established.

## Relationship to Earlier Work

This project builds on the author's Tennessee Tech research:

**Disturbance-Aware Model-Based and Model-Free Zero-Sum Pursuit-Evasion Control: Theory, Algorithm Development, and MATLAB Validation**

That work provides a foundation in four established solution methods, constant command-offset calibration, affine compensation, and matched numerical validation.

The new project focuses more directly on action representation, learning consistency, identifiability, and uncertainty.

Reused code and reference results will be attributed to the earlier work. New findings will be reported after the corresponding experiments and analyses are completed.

See link: https://github.com/bcswieder117/Disturbance-Aware-Model-Based-and-Model-Free-Zero-Sum-Pursuit-Evasion-Control

## Initial Scope

The first stage focuses on:

- Linear discrete-time dynamics.
- Two-player zero-sum games.
- Quadratic and affine-aware action-value representations.
- Explicit actuator-information assumptions.
- Constant offsets followed by controlled bias variation.

Nonlinear vehicle experiments, constraints, safety filtering, and hardware validation are possible later extensions.

## Planned Deliverables

- Reproducible reference implementations.
- Configurable experiment definitions.
- Action-representation and calibration comparisons.
- Repeated-trial uncertainty studies.
- Raw numerical results and publication figures.
- Analytical results supported by the experiments.
- A focused research manuscript.

## Foundational References

1. A. Al-Tamimi, F. L. Lewis, and M. Abu-Khalaf, "Model-Free Q-Learning Designs for Linear Discrete-Time Zero-Sum Games With Application to H-Infinity Control," *Automatica*, vol. 43, no. 3, pp. 473-481, 2007.

2. S. A. A. Rizvi and Z. Lin, "Output Feedback Q-Learning for Discrete-Time Linear Zero-Sum Games With Application to the H-Infinity Control," *Automatica*, vol. 95, pp. 213-221, 2018.

3. B. Luo, Y. Yang, and D. Liu, "Policy Iteration Q-Learning for Data-Based Two-Player Zero-Sum Game of Linear Discrete-Time Systems," *IEEE Transactions on Cybernetics*, vol. 51, no. 7, pp. 3630-3640, 2021.

