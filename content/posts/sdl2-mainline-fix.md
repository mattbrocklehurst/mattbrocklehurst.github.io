---
title: "SDL2 mainline thread-sync fix"
date: 2021-06-15
tags: ["C++", "Kernel", "Debugging", "Open Source"]
---

Hit a deadlock in SDL2 that only showed up on Win32, only under high-frequency window resizing, and only sometimes — the worst kind of bug to chase. Standard debugging didn't catch it because the actual problem lived at the boundary between the Win32 message loop and SDL's internal thread-safety mutexes, not inside either one on its own.

Traced it with WinDbg down to the wait-state logic and a non-deterministic thread release order. Fixed the ordering, submitted it upstream, and it's been part of mainline SDL since — quietly keeping a lot of downstream games and industrial UIs from hanging.
