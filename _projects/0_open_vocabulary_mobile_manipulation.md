---
layout: page
title: Open-Vocabulary Mobile Manipulation
description: Language-guided task planning and execution with the Toyota HSR
img: assets/img/projects/hsr/open-vocabulary-inspection.jpg
importance: 0
related_publications: false
---

This project explores how a mobile manipulator can turn an underspecified natural-language request into a grounded, executable plan. Given a command such as *“Bring me something sweet,”* the system reasons about what the user might mean, where a suitable object could be found, and which navigation and manipulation skills are required to complete the task.

The work is being developed at the Humanoid Robots Lab, University of Bonn, with Sagar and Tanya, under the supervision of Rohit Menon. A project overview is available in our [presentation](https://docs.google.com/presentation/d/1MQOPJkqhrA6Yz6jfPd-d1G5fBkRaiQOr/edit?usp=sharing).

## Full system demo

<div class="embed-responsive embed-responsive-16by9 mt-3">
  <iframe
    class="embed-responsive-item"
    src="https://drive.google.com/file/d/1CpPTdyewxlbIn2D8yPQ635h8h13m4Tlc/preview"
    title="Open-vocabulary mobile manipulation demo"
    loading="lazy"
    allow="autoplay"
    allowfullscreen
  ></iframe>
</div>
<div class="caption">
  End-to-end execution of a language-guided mobile-manipulation task with the Toyota HSR. If the embedded player is unavailable, <a href="https://drive.google.com/file/d/1CpPTdyewxlbIn2D8yPQ635h8h13m4Tlc/view" target="_blank" rel="noopener noreferrer">open the demo in Google Drive</a>.
</div>

## Planning and execution

The system combines an LLM planner with a semantic scene graph and a description of the robot's available capabilities. The planner produces a structured sequence of grounded actions, which are passed to an execution loop built around navigation, manipulation, and sensor feedback. If grounding or execution fails, the resulting context is returned to the planner so that it can revise the plan.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/projects/hsr/planning-flow.jpg" title="Planning and execution flow" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  High-level planning, action grounding, and closed-loop execution using the scene graph and robot capabilities.
</div>

## Scene-graph grounding

My work focuses on constructing scene-graph representations of indoor environments and using them for language-guided task planning. We investigate two complementary pipelines: reconstructing an environment from iPad RGB-D scans and extracting objects with Mask3D, and building the scene graph directly from the Toyota HSR's RGB-D observations using BOXER.

The graph organizes rooms, furniture, and objects into a representation that the planner can query. Overlaying this information on the map connects semantic reasoning with locations the robot can physically navigate to.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/hsr/scene-graph-map.jpg" title="Scene graph overlaid on the robot map" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Semantic room, furniture, and object information overlaid on the robot's map.
</div>

## Exploration and replanning

When the requested object is not already represented in the scene graph, the planner identifies plausible search locations and orders them for exploration. The robot navigates to each location and inspects the current view. A vision-language model reasons about whether a suitable object is visible, while SAM3 provides object-level verification and segmentation.

Once a valid target is discovered, the system adds the new context to the action history and asks the planner for an updated plan.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/hsr/open-vocabulary-inspection.jpg" title="Open-vocabulary object inspection with the Toyota HSR" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  The robot inspects a candidate location, identifies relevant objects, and verifies the selected target before replanning.
</div>

## Manipulation and clarification

After selecting an object, the revised plan invokes segmentation, grasp-pose estimation, and pickup. Navigation and manipulation are executed through Nav2 and MoveIt. If the original request does not specify where the object should be delivered, the robot asks the user for clarification, incorporates the response, and completes the delivery.

This separation between high-level reasoning and grounded robot skills allows the system to respond flexibly to new objects, incomplete requests, and changes observed during execution.
