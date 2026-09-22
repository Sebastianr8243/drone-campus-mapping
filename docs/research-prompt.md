# Master Research Prompt — Drone Campus Mapping Capstone

Paste the block below into ChatGPT (deep research mode), Astra, or another
research-capable AI tool. It's structured so each subsystem's findings can be
pasted straight back as a comment on its matching GitHub issue.

---

## PASTE STARTING HERE

You are supporting research for a university EE capstone project. Act as a
technical research assistant AND a systems engineer doing an architecture
trade study — not just a subsystem-by-subsystem shopping list. Conduct a
structured literature/market survey across multiple subsystems, but think at
the **system level**: the goal is to optimize the drone as a whole, not to
pick the "best" option in each subsystem in isolation. A locally-optimal
choice that creates a system-level problem (too heavy for the airframe, too
much compute draw for the battery budget, a camera that outpaces what the
companion compute can process in real time) is a bad choice even if it wins
its own category. Explicitly call out cross-subsystem interactions and
tradeoffs wherever you see them, and flag when a choice in one section
constrains or conflicts with a choice in another. For every claim, cite a
real, checkable source (paper, datasheet, product page, or repo) — do not
present numbers you can't source. Where sources disagree, say so instead of
picking one silently.

### Project context (read first, applies to every subsystem below)

**Mission:** A drone maps campus micromobility pathways (e-scooters, e-bikes,
pedestrians) via sensor fusion (GPS + IMU + altitude + camera) to support
safety/accessibility planning at UC Merced. The core deliverable is a
before/after comparison: GPS-only position estimate vs. sensor-fused estimate,
showing measurable jitter reduction.

**The 4 target specs the whole system must hit (this is "done"):**
1. Coverage of at least 100 ft × 100 ft (10,000 ft²) in one mapping flight.
2. Minimal altitude/waypoint tracking error (RMSE, exact number pinned once
   flight controller is chosen).
3. At least 25% reduction in GPS-only position jitter using the fused estimate
   (this is the core deliverable — GPS-only vs. fused comparison, same flight).
4. Identifiable ground features in the output map aligned to within 3 ft of
   surveyed ground truth.

**Known constraints:**
- Single prototype (crash risk = schedule risk, no assumed spares budget).
- Team is systems/software/autonomy-focused, not building airframe/mechanical
  from scratch — prefer COTS or lightly-modified platforms unless a subsystem
  specifically calls for custom build.
- Budget ceiling is currently unresolved — for now, report cost for every
  option so a budget decision can be made later, don't assume one.
- Nothing gets implemented until each relevant sweep below lands with concrete,
  comparable options (not just a single recommendation).

**Tool/library maturity bar:** For every open-source tool, library, or
framework you recommend anywhere below, only include it if you can point to
real evidence of maturity — active maintenance (recent commits/releases),
a real user base (GitHub stars/forks, citations, production or published
research use), and real ratings/reviews where applicable. Do not include
tools that only show up in AI-generated blog posts, listicles, or marketing
copy with no independent verification — flag and exclude anything you can't
confirm is real and actively used ("no AI slop"). If you're unsure whether a
tool is real or actively maintained, say so explicitly rather than including
it anyway.

Now research each of the following subsystems **as a separate, clearly
labeled section**, in this order. Don't skip the "why this matters" framing —
it's there to keep your answer scoped to what's actually decision-relevant,
not an exhaustive dump.

---

### 1. Requirements derivation (do this one first — it constrains the rest)

**Objective:** Translate the mission + 4 target specs above into concrete
technical requirements the other sweeps can filter against.

**Answer:**
- What map resolution / ground sample distance (GSD) is needed to reliably
  distinguish a scooter or e-bike from a pedestrian in an aerial image?
- Given the 100ft×100ft coverage area and a reasonable cruise speed, what
  flight endurance (minutes) and camera framerate (fps) avoid motion blur and
  complete the survey in one battery?
- What fusion filter update rate (Hz) is typical/sufficient for GPS+IMU+baro+
  vision fusion at this drone size class?
- What environmental bounds (wind speed, lighting, GPS multipath near
  buildings/trees) should the system be designed to tolerate for a campus
  environment specifically?

---

### 2. Hardware (sensors, flight controller, compute, power)

**Why this matters:** Gates what fusion/vision can run onboard and what the
flight controller interfaces with.

**Answer:**
- IMU, GNSS/GPS, altimeter/barometer, camera module options used in
  comparable low-cost drone mapping builds — current market options and price.
- Flight controller boards: Pixhawk-class vs. lightweight alternatives —
  tradeoffs for this project's scale.
- Companion compute realistic for running fusion/vision onboard (Raspberry
  Pi-class, Jetson-class, others) — capability vs. cost/power/weight tradeoff.
- Battery chemistry, ESCs, power distribution for this build class.
- **Ground this in what GVINS, and the Zhang and Suzuki papers on
  low-cost drone visual-inertial mapping actually used in their hardware
  setups**, where the papers specify it — not just generic options.

### 3. Mechanical (airframe, vibration isolation, fabrication)

**Why this matters:** Determines whether structural/vibration work is a
semester-long subsystem or a constraint accepted from a COTS vendor.

**Answer:**
- COTS airframe options at this project's likely weight/cost class vs. what
  building/modifying from scratch would require (CAD, fabrication, tolerances).
- Vibration isolation approaches for IMU mounting on small multirotors —
  passive (foam/gel mounts) vs. active, and what noise reduction they
  typically achieve.
- Center-of-mass and payload mounting considerations for adding a companion
  computer + camera + gimbal to a COTS frame.

### 4. Software — state estimation & mapping

**Why this matters:** Directly determines whether jitter-reduction spec #3 is
achievable and how map alignment spec #4 gets built.

**Answer:**
- Sensor fusion architecture options: EKF vs. UKF vs. factor-graph approaches
  for GPS+IMU+baro+vision — which are realistic to implement/tune in a
  semester, with real project examples.
- Visual odometry vs. full SLAM (with loop closure) as a fusion input — what's
  the practical difference in effort and in the accuracy this project needs.
- Image stitching / mapping pipeline options (e.g. OpenDroneMap or similar)
  for turning position log + images into the deliverable map.
- **Onboard vs. offline split:** the working assumption is that everything
  time-critical (sensing, fusion, control) runs onboard the drone in
  real time, but the heavy 3D mapping/reconstruction step (turning the
  position log + images into the final map) happens *after* the flight, on
  a ground PC or in the cloud — not onboard. Research what onboard compute
  (from section 2) is actually required if 3D mapping is offloaded this way
  vs. if it had to run onboard, and what mature tools exist specifically for
  post-flight/offline 3D reconstruction from drone imagery + pose data
  (e.g. photogrammetry/SfM pipelines) as opposed to real-time onboard
  SLAM/mapping.
- **Again ground this in GVINS, Zhang, and Suzuki's actual pipelines** where
  the papers specify their approach.

### 5. Autonomy (control tuning, mission path planning, obstacle handling)

**Why this matters:** Defines whether the team writes flight control from
scratch (not expected) or characterizes the plant and tunes existing autopilot
loops.

**Answer:**
- Typical process for system identification + PID/LQR tuning on
  ArduPilot/PX4 for a small multirotor — realistic scope for a semester team.
- Waypoint-following/mission-planning approaches appropriate for a fixed
  100ft×100ft survey pattern (vs. dynamic replanning, which is likely
  overkill here).
- Safety envelope patterns: altitude ceiling, geofence, battery threshold,
  GPS-loss recovery — what's standard practice on ArduPilot/PX4.

### 6. Ground Control Station (mission planning / telemetry)

**Why this matters:** This is the operator-facing tool for planning/monitoring
flights and pulling telemetry logs for the jitter-comparison analysis.

**Answer:**
- Open-source GCS options (QGroundControl, Mission Planner, etc.) — feature
  comparison for mission planning + live telemetry + log export.
- Log formats/tools for extracting GPS-only vs. fused position estimates
  post-flight for analysis (e.g. in Python/pandas or MATLAB).

### 7. Open-source stacks and frameworks

**Why this matters:** Determines how much of the fusion/control/mapping stack
the team gets "for free" vs. builds from scratch.

**Answer:**
- ROS vs. ROS2 vs. no middleware at all — tradeoffs for a team this size on
  this timeline.
- PX4 vs. ArduPilot — company/community support, MAVLink compatibility,
  companion-computer integration patterns.
- ORB-SLAM3 and comparable open visual-SLAM/VO libraries — maturity,
  hardware requirements, licensing.
- OpenDroneMap and comparable mapping/stitching tools — input requirements,
  output formats, compute cost.

### 8. Available models (CV/ML)

**Why this matters:** Needed for detecting/classifying scooters, e-bikes, and
pedestrians in the output imagery.

**Answer:**
- Pretrained object detection models realistic for identifying
  scooters/e-bikes/pedestrians in aerial imagery — accuracy, licensing, and
  whether they run on the companion compute options from section 2.
- Visual odometry / feature-extraction models usable as a fusion input
  (distinct from the SLAM/VO *libraries* in section 7 — this is about the
  underlying models).

### 9. Regulatory, safety, and privacy

**Why this matters:** Gates *when and where* the team is legally allowed to
fly and collect this data at all.

**Answer:**
- FAA Part 107 vs. student/educational exemption requirements for campus
  drone flight in the US.
- Remote ID compliance requirements as of 2026.
- General practice for university campus drone flight authorization — who
  typically approves this (risk management office, campus police, aviation
  safety office) and realistic lead time.
- Privacy considerations for aerial imagery that may capture identifiable
  people on a public university campus — does this typically require IRB
  review, and what precedent exists for similar campus mapping projects.

### 10. System-level synthesis (do this last)

**Objective:** Pull sections 1-9 together into a systems-engineering view,
not a list of independent recommendations.

**Answer:**
- Where do choices across sections conflict or constrain each other (e.g.
  compute choice in section 2 vs. model choice in section 8; airframe weight
  in section 3 vs. payload from sections 2/8; open-source stack in section 7
  vs. what's actually compatible with the hardware in section 2)?
- Propose 1-2 coherent, internally-consistent system configurations (not a
  mix-and-match menu) that would actually work together end to end, and state
  the key tradeoff each one makes.
- Flag anything that, based on this research, looks like it could threaten
  hitting the 4 target specs from the project context — call this out even if
  no other section asked about it directly.

---

## PASTE ENDING HERE

### After you get results back

Paste each section's findings as a comment on its matching GitHub issue:
- Section 1 → issue #23
- Section 2 → issue #14
- Section 3 → issue #15
- Section 4 → issue #16
- Section 5 → issue #17
- Section 6 → issue #18
- Section 7 → issue #19
- Section 8 → issue #20
- Section 9 → issue #24

Once all land, issue #25 (system architecture synthesis) is where they get
reconciled into one coherent stack decision.
