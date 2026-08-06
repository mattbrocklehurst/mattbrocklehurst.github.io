---
title: "OpenPnP contributions"
date: 2024-05-20
tags: ["Robotics", "C++", "Open Source", "Computer Vision"]
---

OpenPnP is open-source pick-and-place software, and getting sub-millimeter placement accuracy out of it means the vision pipeline and the motion control have to agree with each other almost perfectly — most of the failure modes are latency and jitter creeping in at the boundary between the two.

Contributed to the core alignment and feeder-calibration logic and worked through a handful of nasty edge cases in the motion profiles. 20+ feature branches over time, spanning fiducial homing, paste/glue dispensing, vision-based part alignment, and loose-part feeding — alongside an ongoing side project resurrecting old Zevatech hardware onto the same stack.
