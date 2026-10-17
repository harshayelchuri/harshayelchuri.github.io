---
layout: page
title: Active View Planning for Object Reconstruction
description: Informative camera-view selection for robot-assisted 3D reconstruction
img: assets/img/projects/active-view-planning/ur10-hardware.png
importance: 0.1
related_publications: false
---

This project develops an active-vision pipeline that uses a robotic manipulator to capture informative RGB observations for high-resolution 3D object reconstruction. The goal is to improve reconstruction quality while reducing the number of camera views required, particularly for objects with occluded or geometrically complex regions. My primary contribution is the **prior-based pose planner**, which uses an approximate 3D prior to generate informative and robot-feasible camera viewpoints.

The work is part of a collaborative research project at the University of Bonn. A more detailed overview is available in our [project presentation](https://canva.link/introductoryppt).

## Robotic imaging system

The experimental platform combines a UR10 robot with Basler RGB and Intel RealSense cameras. The system includes camera calibration, target-pose generation, collision-aware motion planning, synchronized image capture, reconstruction, and quantitative evaluation.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/projects/active-view-planning/ur10-hardware.png" title="UR10 robotic imaging setup" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  The UR10 imaging setup with RGB and depth cameras, calibration targets, and representative reconstruction objects.
</div>

The broader team system includes hand-eye calibration, kinematic validation, safety constraints, and a hierarchical motion-planning pipeline. Candidate camera poses are filtered for inverse-kinematics feasibility and collisions before execution, with deterministic and sampling-based planners used to reach valid viewpoints.

## Reconstruction pipeline

The reconstruction stack uses open-vocabulary detection and segmentation to isolate the target object, followed by Structure from Motion and Multi-View Stereo with COLMAP/pycolmap. We also investigate neural and Gaussian-based representations, including Gaussian Surfels, for surface reconstruction and novel-view rendering.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/active-view-planning/reconstruction-results.png" title="Comparison of reconstruction representations" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Example comparison of MVS and Gaussian-Surfel reconstructions using geometric accuracy and completeness metrics.
</div>

## Prior-based pose planning

Rather than following a fixed camera trajectory, the planner seeks views that are expected to reveal informative parts of the object. Starting from a small collection of reference images, a 3D diffusion model generates an approximate geometric prior. The planner analyzes this prior, proposes camera candidates around regions of interest, and produces a view path for image acquisition.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/active-view-planning/prior-generation-pipeline.png" title="Prior-based view-planning pipeline" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  A small reference-view collection is converted into a 3D prior, which the pose planner uses to generate an informative camera path.
</div>

My work investigates how camera candidates should be distributed around the generated prior. A single global viewing sphere can place cameras unnecessarily far from elongated or asymmetric objects, so I explored shape-aware alternatives based on the object's convex hull and voxelized geometry. These methods adapt the candidate-view envelope to the estimated object shape while maintaining a safe camera offset.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/active-view-planning/object-hull-candidates.png" title="Object-hull camera-candidate generation" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Shape-aware candidate generation based on a voxelized object prior and an expanded local viewing envelope.
</div>

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/active-view-planning/active-planning-cycle.png" title="Closed-loop active view planning cycle" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  The active-planning cycle connecting reconstruction, candidate-view generation, feasibility checks, robot execution, and model updates.
</div>

The generated candidates are evaluated for visibility and information value before being checked for reachability and collisions. This connects the high-level view planner to the robot's motion-planning stack and turns the selected viewpoints into an executable camera trajectory.

As a complementary direction, the project also explores online next-best-view planning. In this setting, uncertain or insufficiently observed surface regions are detected directly from the current reconstruction and used to guide the next observation.
