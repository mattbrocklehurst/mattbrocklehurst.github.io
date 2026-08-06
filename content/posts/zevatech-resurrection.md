---
title: "Zevatech PM-560 resurrection"
date: 2023-11-15
tags: ["Reverse Engineering", "Firmware", "C++", "Robotics"]
---

A decommissioned Zevatech pick-and-place machine, running on control boards and a communication protocol nobody documented and nobody at the company remembers anymore. No schematics, no source, no vendor support — just the original hardware and a logic analyzer.

Reverse-engineered the signal logic to work out what the original designers actually intended, then wrote a modern firmware layer to drive the original motor controllers and solenoids directly. End result: 1990s mechanics running under a modern PC-based control stack in C++, with none of the original closed-source logic left in the loop.
