# Glossary

**Buck regulator** — A DC-DC step-down converter. U415 (believed to be a TI
TPS54328) bucks the input rail down to the voltage the WiFi chips need. Killing
its input or enable line kills the WiFi power rail.

**DOCSIS** — Data Over Cable Service Interface Specification; the standard the
TG1682 uses to talk to the cable plant (coax/F-connector).

**eMMC** — Embedded MultiMediaCard; the TG1682's 231 MB flash storage
(`MMC256`), holding bootloader, kernel, rootfs, and NVRAM partitions.

**EN (enable) pin** — Active-high input on the TPS54328 (pin 1, 3.3 V). Pulling
it low shuts the regulator down — the reversible alternative to severing VIN.

**Glasgow** — Glasgow Digital Interface Explorer; an open-source multi-protocol
USB tool (UART, JTAG, I2C, logic analyzer). Used here for the J3 UART capture.

**JTAG (IEEE 1149.1)** — Synchronous serial debug bus: TCK (clock), TMS (mode
select), TDI (data in), TDO (data out), plus TRST. Synchronous to TCK — which is
why a UART sniffer sees garbage on JTAG pins (see `images/jtag-vs-uart.webp`).

**Puma 6 (DHCE2652)** — Intel's dual-core 1.2 GHz ARM SoC in the TG1682; boot
ROM codename "Cat Mountain". Notorious for latency/jitter issues (see the
"puma 6 chipset flaws" public reporting).

**PH (phase/switch node)** — The switching output of the buck regulator (pins
6–7 on the TPS54328); feeds the inductor that produces the regulated rail for
the WiFi chips.

**SQUASHFS** — Compressed read-only filesystem; the TG1682's rootfs on
`mmcblk0p5`.

**SWD** — Serial Wire Debug; ARM's 2-pin debug protocol (SWDIO + SWCLK).
Mentioned as a possibility for the 4-pin J3/J4 clusters, but J3 proved to be
UART.

**U-Boot** — The bootloader (v1.2.0 here, with PSPU-Boot 4.2.0.45 wrapper)
that loads the Linux kernel from eMMC.

**UART** — Universal Asynchronous Receiver/Transmitter; 2-wire async serial
(TX/RX, no shared clock). The J3 header exposes the SoC's primary console
UART (`ttyS0`, 115200 8N1 nominal, 3.3 V).

**VIN** — Regulator input-voltage pin (pin 8 on the TPS54328 8-pin package).
Severing it was the mod documented in `u415-mod.md`.

**Bridge mode** — Gateway configuration that disables routing/NAT/firewall/WiFi
so the unit acts as a plain DOCSIS modem, passing the public IP to a
downstream router (here, an AirPort Time Capsule 6th gen).

**NVRAM** — Non-volatile config partition (`mmcblk0p3`, ext3); holds gateway
settings. Survives reboots; factory reset restores defaults.
