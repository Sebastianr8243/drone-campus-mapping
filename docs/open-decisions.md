# Open Decisions

These are not yet decided. Each is tracked as a GitHub issue on the project board —
resolve there and update this file to reflect the decision once made.

## 1. Flight controller / autopilot stack

Options on the table: ROS2 (with a companion computer), PX4 or ArduPilot (running on a
dedicated flight controller board), or a custom microcontroller setup built from
scratch.

This decision affects nearly everything downstream: what fusion runs on, what
control interfaces are available, what languages/frameworks the team writes in, and
how much of the sensing/fusion/control stack we get "for free" vs. build ourselves.

**Do not start implementation on `fusion/` or `control/` until this is resolved.**

## 2. Camera + gimbal hardware

Determines image resolution/quality, whether a physical gimbal is available for
stabilization (vs. software-only stabilization), and how camera timestamps are
synced with the rest of the sensor stack for fusion.

## 3. On-board vs. offline mapping/image stitching

Whether image stitching and map generation happens on the drone in real time, or
images + position/orientation data are collected in flight and stitched into a map
afterward on a ground computer.

Affects `mapping/` module design and the on-board compute requirements chosen
alongside the autopilot stack decision above.
