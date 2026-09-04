# Mapping

Turns collected images and fused position/orientation data into a map output.

## Purpose

Combine timestamped images from `sensing/` with the fused position/orientation
estimate from `fusion/` to produce a map in which ground features are
identifiable and aligned to within 3 ft of ground truth (spec 4 in
[../docs/specs.md](../docs/specs.md)).

## Planned interface

- Input: timestamped images (from `sensing/`) and the corresponding fused
  position/orientation estimate for each image (from `fusion/`).
- Output: a stitched map (e.g. georeferenced orthomosaic) plus a way to
  compare identified ground features against surveyed ground-truth
  positions.

## Open questions

- Whether stitching happens on-board (in flight) or offline (post-processed
  on a ground computer) is not yet decided — see
  [../docs/open-decisions.md](../docs/open-decisions.md). This affects
  whether this module needs to run in real time or can be a batch
  post-processing pipeline.
- Stitching approach/library not yet chosen.
