# Scope & System-Level Questions

## Critical Scope Decisions

### 1. Airframe — Build or COTS?

**The question:** Are we building/heavily modifying an airframe from scratch, or integrating perception/compute onto a commercial platform (Mavic, etc.)?

**Why this matters:** Defines whether CAD, structural analysis, vibration isolation, and center-of-mass tuning are graded deliverables or constraints we accept from a vendor.

**If building:** CoM, vibration mounting, fabrication tolerances, and structural integration become semester-long subsystems.

---

### 2. SLAM vs. Visual Odometry

**The question:** Do we need full SLAM with loop closure and persistent map, or is visual odometry adequate as a fusion input into the state estimator?

**Why this matters:** VO is a sensor (pose velocity sensor). SLAM is a system with relocalization, graph optimization, and multi-scale uncertainty. VO = weeks. Full SLAM = semester.

**Concrete choice:** Are we tracking the drone's pose in a global map, or just drift-correcting local trajectory using visual features?

---

### 3. Control Loop Design — System ID + Tuning

**The question:** Do we identify the airframe's dynamics (plant model) and design a controller from first principles, or identify the plant and tune ArduPilot/PX4's existing autopilot loops?

**Why this matters:** Writing a flight controller from scratch is an RTOS + embedded-control project. The expectation is not that. But "inside the control loop" means we're doing system identification and either:
- Deriving a linearized model and designing gains (classical or LQR), OR
- Measuring response data and tuning existing PIDs with understanding of why.

**Concrete choice:** Are we implementing our own attitude/rate controllers, or are we characterizing the plant and improving existing firmware?

---

### 4. Vibration Isolation — Deliverable or Background?

**The question:** Is IMU vibration isolation design and noise characterization a graded system (with specs, testing, design iteration), or background knowledge we apply?

**Why this matters:** Isolation is one line in SLAM/filter tuning. But if it's explicit, it means mechanical design, frequency-response testing, and noise floor validation.

**Concrete choice:** Do we measure vibration spectra, design/build isolators, and verify noise reduction? Or do we accept the drone's baseline noise and tune the filter?

---

## System-Level Thinking Questions

### Sensor & Compute Stack

1. **Vision Hardware:** Monocular, stereo, or RGB-D? Resolution, framerate, and compute cost matter for real-time SLAM/VO.
2. **Companion Computer:** What platform (Jetson Nano/Orin, Intel NUC, Raspberry Pi)? This gates which SLAM/perception algorithms are feasible.
3. **MAVLink Bridge:** Is the companion computer talking to flight controller over serial UART, or are we replacing the flight controller entirely?
4. **Other Sensors (IMU, Baro, Compass):** Are we fusing vendor-provided data or replacing with our own hardware?

### State Estimation Pipeline

1. **Filter Architecture:** EKF, UKF, or particle filter? Multi-threaded or real-time OS constraints?
2. **Sensor Fusion Order:** What's the priority—GPS, vision, IMU, barometer? How do we handle GPS drop-outs?
3. **Initialization & Drift:** How does the filter initialize heading and position? How do we bound visual drift?

### Autonomy & Mission Planning

1. **Path Planning:** Waypoint following, obstacle avoidance, or continuous replanning?
2. **Replanning Rate:** Real-time (every ~50ms) or offline once per mission?
3. **Safety Envelope:** What are hard stops—altitude ceiling, geofence, battery threshold, GPS-loss recovery?

### Testing & Validation

1. **Simulation First:** Do we test in Gazebo/SITL before flight, or iterate live?
2. **Flight Test Phases:** What's the progression from hover → waypoint → autonomous mapping → robustness under wind/obstruction?
3. **Metrics:** What defines success—mapping accuracy (m²), drift over distance, revisit localization (loop closure)?

---

## Workstream Leads & Dependencies

| Workstream | Owner? | Dependencies | Graded? |
|-----------|--------|--------------|---------|
| **Airframe & Vibration** | — | None (go first) | Part of #5? |
| **Sensor Integration & Calibration** | — | Airframe done | Part of comp design |
| **System Identification** | — | Flight platform stable | Part of #2 |
| **SLAM/VO Pipeline** | — | Sensor cal + compute platform | Part of #1 |
| **State Estimator & Fusion** | — | SLAM + SysID | Part of #2 |
| **Control Tuning** | — | Plant model identified | Part of #2 |
| **Mission Autonomy** | — | Everything above | Part of integration |
