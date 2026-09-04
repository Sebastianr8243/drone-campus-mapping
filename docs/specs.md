# Target Specs

These four specs define "done" for the prototype. Each needs a demonstrable
before/after or ground-truth comparison, not just a qualitative claim.

## 1. Coverage of at least 100 ft² area

The drone must complete a mapping flight covering a contiguous area of at least
100 ft² and produce output (images + position log) for that entire area.

**How measured:** compute covered area from the flight path + camera footprint,
or from the stitched map's ground extent.

## 2. Minimal altitude and waypoint tracking error

The controller must hold commanded altitude and waypoints with small tracking
error throughout a mapping flight.

**How measured:** log commanded vs. actual altitude and position at each
waypoint; report tracking error (e.g. RMSE) for both. "Minimal" will be pinned
to a concrete number once the flight controller and control approach are
chosen (see [open-decisions.md](open-decisions.md)).

## 3. At least 25% reduction in GPS-only position jitter using the fused estimate

This is the core deliverable: fusing IMU + altitude + camera timestamps with
GPS must measurably reduce position jitter compared to GPS alone.

**How measured:** for the same flight, compute a jitter metric (e.g. standard
deviation of position estimate over a stationary or steady-motion segment, or
deviation from a smoothed reference trajectory) for:
- GPS-only position estimate
- Fused position estimate (EKF/UKF output)

Report percent reduction. This is the basis for the required before/after
GPS-only vs. fused comparison.

## 4. Identifiable ground features in the output map aligned to within 3 ft of ground truth

Features visible in the output map (e.g. painted lines, curbs, fixed campus
landmarks) must align with their real-world (ground truth) position within 3
ft.

**How measured:** pick a set of identifiable ground features, survey their
real-world position independently (e.g. measured GPS coordinates or a known
campus reference), and compare against their position as reconstructed in the
output map.
