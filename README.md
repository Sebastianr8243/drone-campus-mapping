# Drone-Based Campus Mapping (Sensor Fusion + Control)

EE capstone project. We build a drone mapping prototype for the UC Merced campus that
digitally maps micromobility pathways (e-scooters, e-bikes, skates), pedestrian use of
space, and open areas — to support safety assessments, accessibility planning, and
campus planning.

Existing commercial drone mapping solutions are too expensive or not suited to this use
case, so we are building our own sensing, fusion, control, and mapping pipeline.

**Status: scaffolding only. No flight code has been written yet.** See
[Open Decisions](docs/open-decisions.md) before starting implementation on any module.

## What the drone needs to do

- Sense on-board: GPS location, inertial measurements (IMU), altitude, and images
- Fuse GPS + IMU + altitude + camera timestamps to estimate drone position and
  orientation (sensor fusion — e.g. EKF/UKF)
- Control the drone to fly a mapping path and hold waypoints/altitude accurately
- Produce a map output from the collected images and position data

## Target specs

See [docs/specs.md](docs/specs.md) for the full detail on each spec and how it will be
measured. Summary:

1. Coverage of at least 100 ft² area
2. Minimal altitude and waypoint tracking error
3. At least 25% reduction in GPS-only position jitter using the fused estimate (vs. GPS
   alone)
4. Identifiable ground features in the output map aligned to within 3 ft of ground truth

The core demo for this project is a **before/after comparison: GPS-only localization
vs. fused localization**, showing the jitter reduction from spec 3 directly.

## Repo layout

```
sensing/   GPS, IMU, altitude, and camera capture
fusion/    EKF/UKF sensor fusion of GPS + IMU + altitude + camera timestamps
control/   waypoint and altitude flight control
mapping/   turns collected images + fused position/orientation into a map output
docs/      specs, open decisions, design notes
```

Each module folder has its own README describing its purpose, expected inputs/outputs,
and open questions specific to that module.

## Open decisions

Hardware/software stack is not yet locked in. Tracked as GitHub issues and summarized in
[docs/open-decisions.md](docs/open-decisions.md):

- Flight controller / autopilot stack (e.g. ROS2, PX4/ArduPilot, custom microcontroller)
- Camera + gimbal hardware
- Whether mapping/image stitching happens on-board or post-processed offline

## How to run

Nothing to run yet — this repo currently contains structure and documentation only.
This section will be filled in once the stack decisions above are made and the first
module has working code.

## Working conventions

- Keep sensing, fusion, control, and mapping code in their respective folders — no
  dumping everything into one file.
- Favor clarity and reproducibility over cleverness. This code needs to be gradeable and
  handed off to teammates.
- Fusion math must be unit tested — "it flew and looked fine" is not sufficient
  verification.
- Documentation and comments are engineer-facing: precise, not marketing language.
