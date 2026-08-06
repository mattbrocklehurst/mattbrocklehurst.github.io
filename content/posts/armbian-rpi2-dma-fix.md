---
title: "Technical Mission: Armbian RPi2 Kernel DMA Fix"
date: 2026-08-06
tags: ["Linux Kernel", "C", "GDB", "QEMU", "Embedded", "Open Source"]
---

## The Objective
Bring Raspberry Pi 2 (BCM2836) support to the Armbian Linux build framework, which had zero coverage for this SoC family, and validate it via QEMU boot testing.

## The Problem
The kernel compiled and packaged cleanly, but panicked at boot -- `VFS: Unable to mount root fs` -- with zero partitions ever detected on the SD card. No amount of config-level tweaking moved the needle; this was a live kernel bug, not a build misconfiguration.

## The Liquidation
* **Deep Debugging:** Built a GDB/QEMU kernel debugging environment from scratch -- DWARF-enabled kernel rebuild, live source-level stepping against the running kernel via QEMU's gdbstub.
* **Root Cause:** Traced the panic down to a missing devicetree `dma-ranges` window: the SD host controller's own MMIO register address fell outside the declared RAM-only DMA bus alias, silently failing the kernel's address-translation lookup.
* **Upstream Engagement:** Testing showed the *existing* upstream kernel fix for this exact driver was incomplete for this SoC. Posted the full analysis directly to the raspberrypi/linux maintainers and opened a pull request against armbian/build.

> "Real hardware never hit this bug -- it took full emulation and a live debugger to expose the gap between what the devicetree claimed and what the silicon actually needed."
