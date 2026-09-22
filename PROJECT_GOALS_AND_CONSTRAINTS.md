# Project Goals & Constraints

## Stakeholders & Users

1. **Who is this system for?** Campus facilities? Research? Student projects? External commercial use?
2. **Who makes decisions** about deployment, flight routes, maintenance? (Admin? Facilities? Department?)
3. **Who operates it day-to-day?** Students? Staff? Contractors?
4. **Who maintains it** after the semester/project ends?

---

## Problem Statement & End Goal

1. **What problem are we solving?** (Gap analysis, cost reduction, capability we don't have?)
2. **What is the measurable outcome?** (Accurate map of campus? Detect infrastructure issues? Autonomous inspection?)
3. **What does "done" look like?** Prototype? Field-deployable system? Handoff to another team?
4. **How will this be used in 6 months? 2 years?**

---

## Constraints & Reality

### Timeline
1. **Semester length?** Semester project or multi-year effort?
2. **When does it need to fly?** Hard deadline for a demo/deliverable?
3. **How much iteration time do we have?** One field test or multiple cycles?

### Budget
1. **Hardware budget?** Are we buying a $500 drone or building a $5k airframe?
2. **Compute budget?** Jetson Orin (~$300) or Nano (~$100)?
3. **One-time or recurring?** Do we build one or manufacture multiples?

### Team & Expertise
1. **How many people?** Solo? Team of 3? 10?
2. **What expertise exists?** Who knows embedded systems? Controls? Vision? Mechanical design?
3. **Who owns each subsystem?** Does one person do airframe + controls + perception, or is it split?
4. **Advisor/mentor availability?** Weekly? On-demand? Hands-off?

### Environmental & Regulatory
1. **Where does it fly?** Open field? Cluttered campus? Urban airspace?
2. **GPS availability?** GPS-rich or GPS-denied zones?
3. **Regulatory constraints?** FAA Part 107 (VLOS only) or Part 108 waiver (BVLOS)?
4. **Safety requirements?** Parachute? Geofence? Kill-switch? Insurance?

---

## Success Metrics

Pick 2–3 that matter most:

- **Mapping Accuracy:** ±X meters ground truth vs. output map?
- **Coverage:** What percentage of target area must be mapped?
- **Autonomy Level:** Waypoint following? BVLOS? Obstacle avoidance?
- **Flight Endurance:** How long must it stay airborne?
- **Reliability:** What's acceptable failure rate? (99% success / 50 flights?)
- **Handoff Readiness:** Can someone else operate/maintain it without you?

---

## Deliverables

1. **Hardware:** Flying prototype? CAD files? Fabrication drawings?
2. **Software:** Open-source? Internal tool? Research paper?
3. **Documentation:** Operator manual? Maintenance guide? Architecture docs?
4. **Data:** Datasets? Maps? Performance logs?
5. **Knowledge Transfer:** Do you train the next team?

---

## Known Unknowns & Risks

1. **What could derail this?** (Weather? Hardware delays? Team turnover?)
2. **What's the backup plan** if the primary approach fails?
3. **Is this exploratory** (research / prove feasibility) **or production** (make it work reliably)?

---

## Relationship to Campus Mapping Mission

1. **What will the final map be used for?** (Planning? Asset inventory? Emergency response?)
2. **Is there an existing map** we're improving or replacing?
3. **Who approves flight operations** on campus?
4. **Are there stakeholders** (facilities, admin, safety) who need to sign off?
