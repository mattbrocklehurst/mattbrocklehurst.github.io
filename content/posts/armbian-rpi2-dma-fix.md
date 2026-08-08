---
title: "Armbian / Raspberry Pi kernel DMA debugging — and a fix that wasn't mine"
date: 2026-08-06
tags: ["Linux Kernel", "C", "GDB", "QEMU", "Embedded", "Open Source"]
---

Armbian (the Linux build framework a lot of SBC distros are built on) had zero support for the Raspberry Pi 2 (BCM2836), so I ported it in from scratch. Kernel compiled and packaged fine, then panicked on boot — `VFS: Unable to mount root fs`, no partitions ever showed up on the SD card. Not a config problem; something real was breaking mid-boot.

Built a full gdb/QEMU debugging setup to actually step through the running kernel — DWARF-enabled rebuild, live source-level breakpoints against QEMU's gdbstub — and traced it down to a missing devicetree `dma-ranges` entry: the SD host controller's own MMIO register sat outside the declared RAM-only DMA window, so the kernel's address translation silently failed and the driver never completed a transfer. Same gap turned up on BCM2837 (Pi 3) too, and that one I got to confirm on a real physical Pi 3 Model B, not just under QEMU — clean boot, DMA and MMC initializing properly, no warnings.

Ported both fixes and opened board-support PRs against `armbian/build`. A maintainer rejected them — bluntly, but not wrongly: kernel-level fixes belong upstream in `raspberrypi/linux`, not patched around indefinitely in a build framework. Fair point, and I went and checked properly rather than just accepting or arguing it.

That's where it got more interesting than the original bug. The actual `raspberrypi/linux` maintainer (Phil Elwell) pointed out something I'd got wrong: I'd assumed Raspberry Pi's firmware patches this devicetree property in dynamically at boot. It doesn't. What's really going on is that `raspberrypi/linux` carries *two parallel devicetree trees* — the one Raspberry Pi actually ships and maintains, and a separate upstream-style one that mirrors mainline Linux's own naming/structure. The downstream tree already had the fix I'd "found." I hadn't discovered anything — I'd just noticed the upstream-style file was missing what the downstream one already had, and copied it across.

Dug one layer further to understand *why* that gap matters at all, since on paper it sounded like a harmless naming difference. Traced the driver's git history and found the actual dependency: a downstream-only patch, authored by that same maintainer, that changed the MMC driver to require DMA address translation for its own register access. That patch is unconditional — every board using this driver depends on it, regardless of which devicetree variant it boots with. So the upstream-style tree isn't just cosmetically different from the downstream one; it's missing something the driver in the *same repository* actually needs. That's a real, useful piece of understanding nobody had written down before, even though the two-line fix itself wasn't mine.

Separately, and this part *is* an original find: fixing the DMA mapping exposed a second bug behind it. `bcm2835_finish_data()` could be called with `host->data` already NULL — a race between the DMA-completion workqueue and the interrupt/status-poll completion path, only reachable once a transfer actually completed successfully, which had never happened on this SoC before the mapping fix. Confirmed live via a kernel panic on the serial console, fixed with a guard.

Net result: one real, still-open bug fixed (the NULL-deref race). One devicetree "fix" that turned out to already exist elsewhere, and I said so once I found out — no point letting a wrong claim stand once you know it's wrong. And a maintainer interaction that, delivery aside, pointed me at a more accurate understanding of the actual problem than I'd have landed on alone.
