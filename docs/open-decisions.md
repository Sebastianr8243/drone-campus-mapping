# Open Decisions

Nothing below is decided yet. We're doing a research sweep first — see the 7 open
issues on the project board (https://github.com/users/Sebastianr8243/projects/5),
one per subsystem. Each sweep's findings get posted as comments on its issue; once a
decision is actually made, it gets written up here and the issue gets closed.

## Research sweeps in progress

1. **Hardware** (sensors, flight controller boards, companion compute, power) — issue #14
2. **Mechanical** (airframe, vibration isolation, fabrication) — issue #15
3. **Software — state estimation & mapping** (sensor fusion, image stitching) — issue #16
4. **Autonomy** (control tuning, mission path planning, obstacle handling) — issue #17
5. **Ground Control Station** (mission planning / telemetry software) — issue #18
6. **Open-source stacks** (ROS/ROS2, PX4/ArduPilot, ORB-SLAM3, OpenDroneMap, etc.) — issue #19
7. **Available models** (visual odometry, feature extraction, obstacle detection) — issue #20

## Decisions this blocks

Nothing gets implemented in `fusion/`, `control/`, `mapping/`, or `sensing/` until the
relevant sweep(s) above land with concrete options. In particular:

- **Flight controller / autopilot stack** — depends on Hardware (#14) + Autonomy (#17)
  + Open-source stacks (#19). Affects what fusion runs on, what control interfaces
  exist, and how much of the stack we get "for free."
- **Camera + gimbal hardware** — depends on Hardware (#14) + Mechanical (#15).
  Determines image quality, whether stabilization is physical or software-only, and
  how camera timestamps sync with the rest of the sensor stack.
- **On-board vs. offline mapping/image stitching** — depends on Software (#16) +
  Hardware (#14, compute capability). Affects `mapping/` module design.
- **Efficiency metric / budget allocation** — deferred until the sweeps above narrow
  the option space. "Efficient" can't be defined against unknown alternatives; revisit
  once real options with real costs are on the table.
