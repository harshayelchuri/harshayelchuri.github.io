---
layout: page
title: "RoboTwin: Robotic Teleoperation Using Digital Twins"
description: Low-latency, safe teleoperation through synchronized digital twins
img: assets/img/projects/robotwin/teleoperation-testbed.png
importance: 0.2
related_publications: false
---

RoboTwin is a dual-digital-twin framework for teleoperating a remote robotic manipulator. Instead of relying only on delayed video, the operator controls a local digital twin and receives immediate visual and haptic feedback. A second twin at the remote site synchronizes the physical robot, acts as a safety layer, and communicates changes in the real environment back to the operator.

I designed and implemented the intercity teleoperation system for controlling a UR3 manipulator over approximately 400 km, developed the real-time digital twin, and designed discrepancy-detection and state-correction methods for synchronizing the simulated and physical environments. This work was carried out at the Indian Institute of Science, Bengaluru, with Diwakar Kumar Singh, Nithish Krishnabharathi Gnani, Prof. T. V. Prabhakar, and Prof. Chandramani Singh.

[Paper](https://arxiv.org/abs/2506.01027){:target="_blank" rel="noopener noreferrer"} &nbsp;·&nbsp; [Demo videos](https://drive.google.com/drive/folders/1edv9f1q7ICg6KwpZm7NtlHhCvB6ozzLT?usp=drive_link){:target="_blank" rel="noopener noreferrer"}

## Full system demo

<div class="embed-responsive embed-responsive-16by9 mt-3">
  <iframe
    class="embed-responsive-item"
    src="https://drive.google.com/file/d/1Z7j6FfNS3TSklCIcxmZ7n7JosQrYZFH6/preview"
    title="RoboTwin teleoperation demo"
    loading="lazy"
    allow="autoplay"
    allowfullscreen
  ></iframe>
</div>
<div class="caption">
  End-to-end RoboTwin teleoperation with a haptic device, synchronized digital twins, and a physical UR3 manipulator.
</div>

## Dual-digital-twin architecture

The operator-side twin follows commands from a Geomagic Touch haptic device with negligible local delay. End-effector poses are sent to the remote-side twin, where RMPflow and ROS 2 generate commands for the physical UR3. Contact forces from the simulated environment are returned through the haptic device, while motion scaling supports precise micro- and macro-movements.

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/robotwin/twin-loop-architecture.png" title="RoboTwin dual-digital-twin architecture" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Two coupled feedback loops connect the operator, simulated environments, and physical robot.
</div>

## Environment synchronization

A RealSense RGB-D camera observes the remote workspace, while a matching virtual camera renders the expected scene in Isaac Sim. The system compares the real and synthetic observations to detect discrepancies. Known objects are localized with YOLOv8 and rendered in the twin; unexpected objects are extracted as point clouds from RGB-D differences and transmitted to update the operator-side environment.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/robotwin/data-flow.png" title="RoboTwin communication and data flow" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Compact pose and environment updates flow between the haptic interface, both twins, and the real robot.
</div>

<div class="row">
  <div class="col-sm-6 mt-3 mt-md-0">
    <div class="embed-responsive embed-responsive-16by9">
      <iframe
        class="embed-responsive-item"
        src="https://drive.google.com/file/d/11TbKF5_aZAQgXL59PnJs3XerrAIdgiJ9/preview"
        title="Known-object synchronization demo"
        loading="lazy"
        allow="autoplay"
        allowfullscreen
      ></iframe>
    </div>
    <div class="caption">Known objects detected in the real workspace and rendered in the digital twin.</div>
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    <div class="embed-responsive embed-responsive-16by9">
      <iframe
        class="embed-responsive-item"
        src="https://drive.google.com/file/d/1qkrkkc9tDGA5NzOrMnSnrUzwAaOeO7On/preview"
        title="Unknown-object discrepancy detection demo"
        loading="lazy"
        allow="autoplay"
        allowfullscreen
      ></iframe>
    </div>
    <div class="caption">Previously unknown objects transferred to the digital twin as discrepancy point clouds.</div>
  </div>
</div>

## Evaluation

We evaluated RoboTwin through a spiral-tracing study with 17 participants under 1 ms and 100 ms round-trip network delays. Compared with conventional video feedback, the digital-twin interface reduced reported workload across the NASA-TLX dimensions and enabled more stable tracing under high latency. Transmitting compact object updates instead of a continuous video stream also reduced the data rate for operator-side object identification by approximately 25×.
