# REVIEW — Technical Corrections, Flags, Open Questions

Review of the XB3 material extracted from the conversation archive. Corrections
applied in the docs; unresolved items listed below.

## Corrections made

1. **USB-A ports are NOT UART.** An assistant message in the source conversation
   claimed the TG1682's USB-A D+/D- lines are UART TX/RX, based on the Glasgow
   boot-log capture. The user corrected this in-conversation: the Glasgow was
   connected to the drilled-out **J3** pads, not to a USB-A port. `J1/J2 ≠ UART`
   is now stated explicitly in `usb-ports-analysis.md`.
2. **J1/J2 = JTAG is a hypothesis, not a finding.** No JTAG session, boundary
   scan, or pin identification was ever performed in the archive. The
   "screaming data" anecdote (UART-sniffing J1/J2 yielding garbage) appears in
   summaries but no primary capture of it was found in the conversations. The
   docs now label JTAG as "hypothesis only — TODO: verify".
3. **Cable continuity anomaly flagged, not smoothed over.** Micro-USB pin 2
   (D-) shows continuity to the VBUS group — abnormal. Recorded as suspect
   rather than presented as a valid mapping.
4. **J3/J4 disambiguation.** The user's messages use "J3" for both the drilled
   4-pin UART pads and (in one message) "the center chip". Docs use J3 = UART
   pad cluster, J4 = second 4-pin cluster (function unknown), per the majority
   of messages.

## AI-generated diagrams — accuracy notes

Image generation was available; 7 diagrams were produced under `docs/images/`
and referenced from the docs. They are **illustrative, not measured**. Known
deviations from source material:

- `board-diagram.webp`: WiFi module labels "Atheros AR9381" / "QCA9880" were
  invented by the image generator — the sources only say "Atheros WiFi chips".
  Treat module part numbers as placeholder. J3 pad order shown is illustrative.
- `u415-mod.webp`: the "5V input" annotation on the VIN trace was not in any
  source; input voltage is unverified. The "inferred — verify marking" and
  warning banner are correct and must stay.
- `cable-pinout.webp`: minor garbling in the USB-C pin-number rows; the
  measured mapping table and voltage readings are faithfully reproduced.
- `j3-uart-header.webp`, `jtag-vs-uart.webp`, `boot-sequence.webp`,
  `network-topology.webp`: no material deviations found.

## Unverifiable claims (flagged, not asserted)

- **"Secret Apple debug pinout config"** (gnd→gnd, d+→l0+, d-→l0-, vcc→id1):
  user's claim, no test or source in the archive.
- **U415 = TPS54328**: inferred from the reference gist's "54328" marking; the
  user never confirmed the U415 marking on their board.
- **Pin 8 = VIN**: inferred from the TPS54328 8-pin package pinout; never
  measured on this board.
- **nyan_satan tweet config** (UART-over-micro-USB method): contents not
  retrievable from the archive; applicability to the TG1682 unestablished.
- **"TG1682 manual" vs schematics**: correct that FCC schematics are
  confidential; no leak was found in the archive.

## Open questions / TODOs

- [ ] Photograph/confirm the U415 chip marking under magnification.
- [ ] Verify pin 8 = VIN by continuity to the input capacitor / datasheet.
- [ ] Buzz out the J3 pad order properly (GND → shield, VCC → 3.3 V, then
      identify TX by boot output; RX by trial at 115200).
- [ ] Scope J1/J2 pins during boot to determine JTAG vs dead-USB.
- [ ] Attempt the planned usb2Demon + eLabGuys breakout JTAG session.
- [ ] Identify J4's 4-pin cluster and the 5x2 cluster by the battery.
- [ ] Re-measure the micro-USB cable pin 2 (D-) continuity — likely probe slip.

## Hygiene

- No secrets, credentials, or personal names/emails in the repo. The only
  network-identifier strings in the source were already-redacted IP fragments
  (last octets masked) — none reproduced here.
- Hardware-mod docs carry explicit destructiveness warnings.
- All dates, chip IDs, and readings above come from the conversation records;
  anything inferred is marked as such.
