# Control

Flies the drone along a mapping path, holding commanded waypoints and altitude.

## Purpose

Consume the fused position/orientation estimate from `fusion/` and issue
control commands so the drone tracks a planned mapping path with minimal
altitude and waypoint error (spec 2 in [../docs/specs.md](../docs/specs.md)).

## Planned interface

- Input: fused `{position, orientation}` estimate from `fusion/`, plus a
  planned path (waypoints + target altitude) covering at least 100 ft² (spec
  1).
- Output: control commands to the flight controller/autopilot, and a log of
  commanded vs. actual position/altitude for tracking-error measurement.

## Open questions

- Whether control logic runs as our own code or is delegated to
  PX4/ArduPilot's built-in waypoint navigation depends on the autopilot stack
  decision (see [../docs/open-decisions.md](../docs/open-decisions.md)).
- Path planning for 100 ft² coverage (pattern shape, overlap for mapping) is
  not yet designed.
