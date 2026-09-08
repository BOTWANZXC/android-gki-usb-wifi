# v3.1 — MT7601U support (monitor + injection), GKI symbol-limit handling

Adds a second **hardware-verified** adapter — and it's the cheap one most people
already own.

## New

- **MT7601U support** (`driver: mt7601u`). Verified on real hardware: **monitor
  mode and injection both confirmed**. MT7601U is the chipset in a huge number
  of ~€5–10 USB dongles, so this is a far more accessible entry point than a
  dual-band ALFA.
- **Handling for GKI symbol trimming.** GKI kernels only export an approved
  symbol list (the KMI), so a driver can compile perfectly and still fail to
  load with `Unknown symbol …`. `mt7601u` hits exactly this
  (`firmware_request_cache`). The workflow now patches it automatically — the
  call is only a suspend/resume firmware cache, so neutralising it is safe.
  **This means "in-tree" is necessary but not sufficient on GKI**; see the new
  section in [TROUBLESHOOTING.md](TROUBLESHOOTING.md).
- **More adapters recognised by `detect.sh`**, including MT7601U variants and
  adapters that boot in **CD-ROM / driver-install mode** (e.g. `0bda:1a2b`),
  which need `usb_modeswitch` before they present as Wi-Fi at all.
- **Pre-GKI detection.** Kernels older than 5.10 are now identified explicitly,
  with honest guidance instead of a misleading branch guess. (Thanks to the
  first external report — a 4.14 custom kernel — for surfacing this.)

## Changed

- Both tested adapters are now confirmed for **injection**, not just monitor
  mode: `mt76x2u` (ALFA AWUS036ACM) and `mt7601u`.
- The Action button does **only** the monitor/managed toggle again; the optional
  compatibility-report link moved to a passive `report-link.txt` inside the
  module, so pressing Action never pulls you into a browser.
- Kernel-source download now verifies the archive and retries over HTTP/1.1
  (googlesource intermittently truncates HTTP/2 transfers).
- CI gained an early shell-syntax check, so a typo fails in seconds instead of
  40 minutes into a build.

## Known limitation

**Enable only one adapter module at a time.** Each adapter has its own flashable
module; with two enabled they conflict over `wlan1`. To switch adapters: disable
the current module, enable the other, reboot, plug that adapter in. The module
now warns in its log if it detects more than one installed.

## Notes

- Prebuilt modules here are for the **reference kernel**
  (`5.15.178-android13-8-…-g4ea0fcb5d130-ab13530115`, Teclast P30T / Unisoc
  UMS9230). On any other kernel the `module_layout` CRC won't match — run
  `detect.sh` and build your own (~35 min in CI).
- Tried it on another device or adapter? `detect.sh` prints a pre-filled report
  link at the end. Confirmed combos go into
  [COMPATIBILITY.md](COMPATIBILITY.md).
