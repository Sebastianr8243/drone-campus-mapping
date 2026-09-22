---
kanban-plugin: basic
---

## This board has moved

We now track work on the GitHub Project board instead of this file, so there's one
source of truth instead of two drifting copies:

**https://github.com/users/Sebastianr8243/projects/5**

That board holds:

- The 9 active research-sweep issues (Hardware, Mechanical, Software, Autonomy, GCS,
  Open-source stacks, Models, Test & Validation, Admin)
- Milestone: PDR - Preliminary Design Review, due Oct 22
- Everything this file used to track (Decisions Needed, Research, Outreach, Backlog)
  is now issues on that board instead of checkboxes here

Everything below is kept for historical reference only — it's what this file
contained before the move. Don't add new items here; add them as GitHub issues.

---

## Decisions Needed (blocking) — historical

- [ ] Airframe: build vs. COTS — lean COTS given $500 budget (blocks control/, mechanical work)
- [ ] Flight controller / autopilot stack: ROS2 + companion computer vs. PX4/ArduPilot vs. custom MCU (see docs/open-decisions.md — blocks fusion/ and control/)
- [ ] Camera + gimbal hardware (blocks sensing/ and mapping/ timestamp sync)
- [ ] On-board vs. offline image stitching (blocks mapping/ design + compute budget)
- [ ] SLAM/VO scope — note: docs/specs.md only requires GPS+IMU+camera-timestamp fusion, not full SLAM; don't over-scope past the graded spec
- [ ] Control loop depth — system ID + tune existing autopilot PIDs vs. write a custom controller from scratch
- [ ] Vibration isolation — graded deliverable (design + test) or background knowledge we just apply?
- [ ] Autonomy level target — reconcile "Level 3 potentially" (Week 1 notes) against $500 budget, one-semester timeline, and Part 108 DAA requirements (research-paper-comparison.md); specs.md as written matches Level 1 VLOS + offline mapping

## Research — historical

- [ ] Survey existing campus maps — what already exists, what's missing/outdated (left blank in Week 1 notes)
- [ ] COTS drone options under budget that can carry a companion computer + camera payload
- [ ] Companion computer options (Jetson Nano/Orin vs. Raspberry Pi) — cost vs. compute needed for chosen fusion/mapping approach
- [ ] EKF vs. UKF for GPS+IMU+altitude+camera-timestamp fusion — pick one, write down why
- [ ] FAA Part 107 constraints for flying over campus + who at UC Merced approves flight operations
- [ ] Check if UC Merced facilities/GIS already has campus pathway data (avoid duplicating work)

## Outreach — historical

- [ ] Email Dr. Stark — drone
- [ ] Email Dr. Xiaofan Yu
- [ ] Email civil engineering professor
- [ ] Follow up with Ayush — controls

## Backlog — historical

- [ ] Answer PROJECT_GOALS_AND_CONSTRAINTS.md (stakeholders, success metrics, deliverables, timeline)
- [ ] Assign workstream owners in SCOPE_AND_QUESTIONS.md dependency table
- [ ] Purchase airframe/hardware once build-vs-COTS + budget allocation is decided
- [ ] Sensor integration + calibration
- [ ] EKF/UKF fusion implementation + unit tests (per README: fusion math must be unit tested)
- [ ] Waypoint/altitude control tuning
- [ ] Image stitching / mapping pipeline
- [ ] GPS-only vs. fused jitter comparison — the core spec 3 demo

## Done — historical

- [x] Repo scaffolding + module READMEs
- [x] Target specs defined (docs/specs.md)
- [x] Research paper comparison + FAA regulatory mapping
- [x] Week 1 meeting notes with Ayush
