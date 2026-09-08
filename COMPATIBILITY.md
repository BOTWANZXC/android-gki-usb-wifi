# Compatibility

Confirmed device + adapter combos, from testing and community
[compatibility reports](../../issues/new?template=compatibility.yml).

**Legend:** ✅ working · 🟡 partial · ❌ failed · (blank) untested

## Devices

| Device | SoC | Kernel string | Status | Prebuilt modules | Reporter |
|---|---|---|---|---|---|
| Teclast P30T | Unisoc UMS9230 (T615) | `5.15.178-android13-8-…-g4ea0fcb5d130-ab13530115` | ✅ | [release](../../releases/latest) | @markatsos |

## Adapters

Tested on a confirmed device above.

| Adapter | Chipset | USB ID | Driver | Monitor | Injection | Notes |
|---|---|---|---|---|---|---|
| ALFA AWUS036ACM | MT7612U | `0e8d:7612` | `mt76x2u` | ✅ | not tested | dual-band; needs `mt7662*.bin` |
| generic USB dongle | MT7601U | `148f:7601` | `mt7601u` | ✅ | ✅ | 2.4GHz only; very common cheap dongle; needs the GKI KMI patch (applied automatically by the workflow) |

## Known limitation: one adapter module at a time

Each adapter gets its own flashable module. **Enable only the module for the
adapter you're actually using.** Having two enabled at once doesn't work — to
switch adapters:

1. Magisk/KernelSU → Modules → disable the current one, enable the other
2. Reboot
3. Plug in that adapter

In practice this is fine, since you use one adapter at a time anyway.

---

## About the "Prebuilt modules" column

Modules only load on a kernel whose **`module_layout` CRC matches** the one they
were built against — in practice, the **exact same kernel string**. If your
`uname -r` matches a row above exactly, that row's prebuilt modules should load
on your device too; otherwise build your own (the workflow takes ~35 min).

Links point to the **reporter's own** release. Treat third-party kernel modules
with the caution you'd give any code running in kernel space: prefer building
your own, and only use someone else's binaries if you trust the source.

## Add your device

Run `scripts/detect.sh` — at the end it prints a **pre-filled issue link** with
your kernel and adapter already filled in. Nothing is sent automatically; you
open the link, review, and submit. Confirmed combos get added above.
