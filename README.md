# XB3 Hardware Notes

Board-level analysis of the Xfinity XB3 gateway (Arris TG1682 family, Intel Puma 6).
All work performed on my own pre-paid unit.

## The device

- **Xfinity XB3** — Arris TG1682P (board silkscreen: TG1682/TG2472)
- **SoC:** Intel Puma 6 ("Cat Mountain D0" per the boot ROM banner)
- 256 MB DRAM, ~231 MB eMMC ("MMC256"), U-Boot 1.2.0 / PSPU-Boot 4.2.0.45, Linux 3.12.14 (ARM)
- U-Boot reports `Board-Type: harborpark-mg`, boots the `UBFI1` image from eMMC

## J3 UART bring-up

Opened the enclosure and located the unpopulated 4-pin **J3** header next to the
main SoC. Wired it to a [Glasgow Interface Explorer](https://glasgow-embedded.org/)
running the `uart` applet at 3.3 V — three jumpers on J3 pins 2/3/4 (RX, TX, GND) —
and captured the full boot log **read-only**:

```
glasgow run uart -V 3.3 -a --rx A0 tty
```

What the log shows: Boot ROM memory/parameter dump → U-Boot loading from eMMC →
kernel decompression → Linux 3.12.14 boot on the Puma 6 (`Machine: puma6`).
See [boot-log-excerpt.txt](boot-log-excerpt.txt) (MACs and host identifiers redacted).

**No console access was obtained.** U-Boot is configured with a 0-second autoboot
delay, and the UART never dropped to a shell — capture was strictly receive-only.

## Header map (as identified on the board)

| Header | Function |
|--------|----------|
| J1, J2 | USB-A ports |
| J3     | UART (boot log, read-only) |
| J4     | JTAG (identified, not exercised) |

JTAG via J4 was mapped but no JTAG session was ever established — don't read more
into it than that.

## Hardware factory reset

With the admin password unknown, I severed **pin 8 on U415** to force a factory
reset, then reconfigured the unit from defaults.

## Bridge mode

The XB3 was put into bridge mode via the admin panel at `http://10.0.0.1/` and
paired with an Apple AirPort Time Capsule (6th gen) as the actual router.
Motivation: even in bridge mode the XB3 kept broadcasting hidden SSIDs
(`xfinitywifi` hotspot, mesh backhaul) with no software toggle to kill the radios,
causing co-channel interference with the Time Capsule sitting next to it.

## Published Wi-Fi-disable mod (reference)

A published hardware mod for this exact problem: the board carries two shielded
Atheros Wi-Fi modules fed by an 8-pin **TPS54328** buck regulator ("54328").
Its pins 6–7 (Vout) feed both Wi-Fi chips; pin 1 is the enable line (3.3 V).
Grounding pin 1 shuts the regulator down and kills the radios while DOCSIS and
wired Ethernet keep working — reportedly dropping idle draw from ~14.9 W to
~12.5 W. Documented here as reference for the interference problem above; this
is a published mod, researched — not one I performed on my unit.

## Explicit non-claims

- No shell/console access was ever gained on this device.
- No JTAG debugging session was performed.
- No firmware was extracted, modified, or redistributed.
- This is **not** a power-optimization project — the Wi-Fi work was about
  eliminating radio interference, full stop.
- Nothing here bypasses ISP controls on anyone else's equipment. This was my
  own pre-paid modem, opened and probed on my own bench.
