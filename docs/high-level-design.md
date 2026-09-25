# High-Level Systems Design Document
## Autonomous Aerial Mapping & Spatial Data Engine

> **STATUS: PROPOSAL, not decided.** Draft architecture for team review. Nothing here
> resolves an item in [open-decisions.md](open-decisions.md); see "Assumptions that touch
> open decisions" and "Open questions" at the bottom. Targets below are reconciled with
> [specs.md](specs.md).

**Project Context:** Undergraduate Capstone Project  
**Target Budget:** $500 Cap  
**Core Mission:** Deploy a budget-friendly autonomous quadcopter to perform automated aerial grid surveys, capturing dynamic real-world campus spatial data (such as informal "desire paths" and temporary construction) and offloading the data for 3D map generation and ground navigation updates.

---

## 1. High-Level Architectural Concept

Rather than attempting heavy real-time 3D processing on a low-cost drone, the system utilizes a **decoupled software architecture**:
* **Onboard (Lightweight Flight Node):** Handles real-time flight stability, multi-sensor state estimation, camera triggering, and reactive local obstacle avoidance.
* **Offboard (Heavy Spatial Engine):** Handles post-flight 3D photogrammetry, image feature matching, geometric corrections, and orthomosaic map rendering on a ground laboratory server.

```
┌────────────────────────────────────────────────────────┐
│             1. AIRFRAME & POWER SUBSYSTEM              │
│    Base Quad Frame • Motors & ESCs • Battery Pack      │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│         2. FLIGHT CONTROL & AUTONOMY SUBSYSTEM          │
│   • Multi-Sensor Fusion (GPS for XY, Baro for Z)       │
│   • Active Motor Vibration Filtering                   │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│          3. PAYLOAD & IMAGE CAPTURE SUBSYSTEM          │
│   • Optical Camera Module • Low-Power Companion Board  │
│   • GPS Time Sync & Image Exposure Timestamping        │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│             4. REACTIVE OBSTACLE SENSING               │
│   • 2D Proximity Distance Sensor                       │
│   • Local Vector Avoidance Algorithm                   │
└───────────────────────────┬────────────────────────────┘
                            │ (Post-Flight Data Offload)
                            ▼
┌────────────────────────────────────────────────────────┐
│          5. OFFBOARD 3D PHOTOGRAMMETRY ENGINE          │
│   • Feature Matching & Rolling Shutter Un-shearing     │
│   • Output: 3D Point Cloud & Map Layer for Ground Nav  │
└────────────────────────────────────────────────────────┘
```

---

## 2. Core Functional Subsystems

### 1. Airframe & Power Subsystem
* **Role:** Provides the physical flight platform and distributes electrical power within the strict $500 budget limit.
* **Key Functions:** Houses flight components, balances center of mass, isolates motor vibrations, and provides ~10 minutes of usable flight time per battery charge.

### 2. Flight Control & Autonomy Subsystem
* **Role:** Manages vehicle stability, trajectory tracking, and real-time state estimation.
* **Key Functions:** Fuses GNSS position data for horizontal tracking ($XY$) and internal barometer readings for altitude hold ($Z$). Runs dynamic notch filtering to attenuate motor resonance before it corrupts sensor data.

### 3. Payload & Image Capture Subsystem
* **Role:** Acquires high-resolution aerial imagery along survey grid lines.
* **Key Functions:** Uses a low-power companion board to trigger the camera module at set distance intervals and logs exact millisecond GPS exposure timestamps into flight logs for downstream geotagging.

### 4. Reactive Obstacle Sensing Subsystem
* **Role:** Provides real-time collision prevention without heavy onboard computation.
* **Key Functions:** Operates a 2D planar proximity sensor and runs a lightweight local vector avoidance algorithm to steer around obstacles (trees, posts) while maintaining mission progress.

### 5. Offboard 3D Photogrammetry Engine
* **Role:** Transforms raw aerial imagery into spatially accurate 3D maps post-flight.
* **Key Functions:** Runs Structure-from-Motion (SfM) feature matching on a ground server, applies software rolling-shutter geometric correction, aligns data using ground control points, and exports updated map layers to ground systems (such as campus GIS or AR navigation apps).

---

## 3. End-to-End Operational Pipeline

1. **Pre-Flight Setup:** Lay down surveyed ground markers and upload an automated lawnmower survey grid.
2. **Autonomous Flight:** Drone takes off and navigates grid waypoints at 2.0 m/s using multi-sensor fusion.
3. **Synchronized Image Capture:** Camera triggers based on distance flown, logging millisecond GPS exposure timestamps.
4. **Reactive Avoidance:** Proximity sensor scans for dynamic obstacles and steers the drone safely around them.
5. **Post-Flight Reconstruction:** Raw images and telemetry logs are transferred to the offboard server for 3D stitching and rolling-shutter correction.
6. **Ground System Integration:** The generated 3D point cloud or orthomosaic map is published to update navigation interfaces with real-world path changes.

---

## 4. Measurable System Objectives

* **Total Cost:** Capped strictly at **$500**.
* **Area Coverage (R1):** Complete full photographic coverage of a **929 m² (10,000 sq ft)** survey grid in a single pass.
* **Trajectory Control (R2):** Maintain an altitude Root Mean Square Error **RMSE ≤ 0.5 m** using barometer hold, **and** report waypoint tracking error (RMSE of commanded vs. actual position at each waypoint). Waypoint target is TBD until the flight controller is chosen (specs.md #2).
* **State Smoothing (R3):** Reduce fused position jitter to **≤ 75%** of raw GNSS jitter via multi-sensor filtering.
* **Map Accuracy (R4):** Achieve offline 3D map spatial alignment within **0.9144 m (< 3 ft)** of ground checkpoints.
* **Safety Failsafes:** Execute automated Return to Launch (RTL) upon sensor loss or low battery thresholds.

---

## 5. Assumptions that touch open decisions

The design above implicitly picks these. They are **not** decided; each maps to a research sweep.

| Assumption in this design | Open item | Issue |
|---|---|---|
| Low-power companion board for triggering + timestamping | Companion compute / hardware | #14 |
| Photogrammetry runs offboard, post-flight | On-board vs. offline mapping | #16 |
| 2D planar proximity sensor | Sensor selection | #14 |
| Dynamic notch filtering | Implies a specific FC/firmware class | #14, #19 |
| $500 total budget | Not stated elsewhere in the repo. Confirm it is a real constraint | — |
| No visual odometry / SLAM in the state estimate | SLAM vs. VO (SCOPE_AND_QUESTIONS.md #2) | #16, #19 |

## 6. Open questions

1. **Where does the `fusion/` EKF run?** Stock autopilot EKF already fuses GPS (XY) + baro (Z).
   The core demo (specs.md #3) needs GPS + IMU + altitude + camera timestamps, GPS-only vs. fused.
   Options: (a) offboard EKF on flight logs, (b) custom EKF on the companion board in real time,
   (c) autopilot EKF only, tuned and analyzed. **Unresolved.**
2. **Survey altitude and camera swath width** are undefined; coverage (R1) depends on both.
   Sanity check: 929 m² is roughly a 30.5 m × 30.5 m square. At 2.0 m/s, ~10 min of flight is
   ~1.2 km of path, so endurance is not the constraint. Line spacing and image overlap are.
3. **Obstacle avoidance and rolling-shutter correction are required** (team decision) but have no
   spec in specs.md yet. Proposed specs are drafted below. Both add significant workload:
   proximity sensing + avoidance logic in `control/`, and correction in `mapping/`.

### Proposed additional specs (numbers TBD by team)

- **Obstacle avoidance:** clear N of M staged obstacle trials (e.g., post, tree-sized object)
  without contact while maintaining mission progress. *Measured by:* logged closest-approach
  distance per trial.
- **Rolling-shutter correction:** map alignment error (ground checkpoints) with correction
  vs. without, on the same image set. *Measured by:* checkpoint error in metres, both cases.
