---
title: "Armbian / Raspberry Pi 2 kernel DMA fix"
date: 2026-08-06
tags: ["Linux Kernel", "C", "GDB", "QEMU", "Embedded", "Open Source"]
---

Armbian (the Linux build framework a lot of SBC distros are built on) had zero support for the Raspberry Pi 2 (BCM2836), so I ported it in from scratch. Kernel compiled and packaged fine, then panicked on boot — `VFS: Unable to mount root fs`, no partitions ever showed up on the SD card. Not a config problem; something real was breaking mid-boot.

Built a full gdb/QEMU debugging setup to actually step through the running kernel — DWARF-enabled rebuild, live source-level breakpoints against QEMU's gdbstub — and traced it down to a missing devicetree `dma-ranges` entry: the SD host controller's own MMIO register sat outside the declared RAM-only DMA window, so the kernel's address translation silently failed and the driver never completed a transfer.

Along the way, found that the *existing* upstream kernel fix for this exact driver (already merged, closing a real GitHub issue) didn't actually cover this SoC. Posted the analysis directly to the raspberrypi/linux maintainers and opened a PR against armbian/build with the fix.
