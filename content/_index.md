---
title: "Matt Brocklehurst"
layout: "index"
type: "page"
---
## Matt Brocklehurst

**R&D engineer. I end up wherever software meets hardware that won't cooperate.**

Most of what I do lives at that boundary — reverse-engineering protocols nobody documented, chasing kernel-level race conditions, keeping industrial hardware running long after the manufacturer stopped caring. If the fix needs an oscilloscope and a debugger open in the same afternoon, that's usually mine.

---

## 🛠️ What I actually do

* **Legacy hardware:** reverse-engineered and rebuilt control logic for equipment (Zevatech pick-and-place, legacy ECUs) with no remaining documentation or vendor support.
* **Low-level C/C++:** deterministic rendering, low-latency audio, kernel-adjacent synchronization bugs — the kind that only show up under load.
* **Firmware at scale:** logic running on 50,000+ deployed PIC-based units.
* **Full-stack hardware:** multi-layer PCB design (Altium/Pulsonix) through to ARM/RTOS firmware.

## 🚀 Recent work

* **SDL2 mainline fix:** tracked a Win32 thread-sync deadlock down to the kernel synchronization primitives, fix is upstream.
* **DIBThread rendering:** a rendering kernel built for deterministic 60fps output.
* **OpenPnP:** ongoing contributions to vision and motion-control logic for open-source SMT assembly.
* **Raspberry Pi kernel DMA debugging:** root-caused a Linux DMA-mapping bug live under gdb/QEMU across two SoCs (Pi 2/3), fixed a genuine NULL-deref race it exposed, and worked out with the actual kernel maintainer why raspberrypi/linux's own upstream-style devicetree diverges from its downstream one.

---

*Available for R&D/lead roles and consultancy.*
[Email](mailto:m@mattbrocklehurst.co.uk) | [GitHub](https://github.com/mattbrocklehurst)
