# Fusion

Fuses GPS + IMU + altitude + camera timestamps into a single position and
orientation estimate (e.g. via an Extended or Unscented Kalman Filter).

## Purpose

Take raw sensor samples from `sensing/` and produce a fused state estimate
that is measurably less jittery than the GPS-only position estimate (spec 3
in [../docs/specs.md](../docs/specs.md): at least 25% jitter reduction).

## Testability requirement

The core fusion math (predict/update steps, state transition, measurement
models) must have unit tests independent of any real flight. "It flew and
looked fine" is not acceptable verification for this module — tests should
cover the math against known/synthetic inputs with known expected outputs.

## Planned interface

- Input: timestamped `{gps, imu, altitude}` samples from `sensing/`.
- Output: timestamped fused `{position, orientation}` estimate, plus (for the
  before/after comparison) the GPS-only position estimate over the same
  timespan.

## Open questions

- What language/framework this runs in depends on the autopilot stack
  decision (see [../docs/open-decisions.md](../docs/open-decisions.md)) — a
  ROS2 stack likely means Python/C++ nodes, PX4/ArduPilot may run fusion
  on-board or push raw data to a companion computer, a custom microcontroller
  setup means fusion runs in embedded C/C++.
