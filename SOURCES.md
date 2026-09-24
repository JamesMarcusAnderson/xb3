# Sources — XB3 project

Every factual claim in the README traces to James Anderson's own words
(REQUEST fragments) or his pasted terminal output in his DeepSeek chat archive
(`~/workspace/deepseek-archive/conversations.json`). Assistant responses were
never used as evidence. Conversation IDs below are the archive's `id` fields.

## 264001f2-ebfb-4184-ba86-5e5739a41f4e — "TG1682 USB Ports Hardware Purpose" (2026-04-04)
Primary source. James's terminal output + narration:
- Opened the enclosure; drilled out the J3 4-pin pads next to the center of the
  board by the main SoC ("no... i opened the enclosure and manually drilled out
  the J3 4pin pads next to th4 center of the mother board").
- Glasgow UART capture: `glasgow run uart -V 3.3 -a --rx A0 tty` → full boot log
  (Cat Mountain D0 Boot Ram v0.1.16, U-Boot 1.2.0, PSPU-Boot 4.2.0.45, 256 MB DRAM,
  eMMC "MMC256" 231.3 MB, Board-Type harborpark-mg, Linux 3.12.14, Machine: puma6).
- Pinout used: "i used the glasgow with 3 jumpers on pin 2, 3 and 4, (rx)(tx)(gnd)".
- Header map: "the usba ports both say J1 and J2 and J3 is uart and J4 is jtag".
- No console: "J3 is the 4pin i used for uart to obtain the logs", "man i just told
  you the J3 port wont finish the boot to give me access", "no, im talking about
  the j3 'just press enter bro'" (pressing enter yielded no shell).
- Bridge-mode motivation: XB3 in bridge mode "jams my network" when paired with
  his Apple router; he keeps the $30/mo pre-paid service and "i have to use their
  modem but i also get to keep it too".
- Pasted `usb2demon_puma6.py` JTAG-enumeration script (PyUSB VID/PID scan for
  Macraigor/FTDI/Intel probes) — drafted, **no confirmed JTAG session**; excluded
  from claims beyond "identified".

## e5339070-4867-4527-bcf5-27050b1efcc8 — "Xfinity XB3 login troubleshooting" (2026-04-13)
- "no you cant im pre-paid this modem is mine and i severed the pin8 on u415 so i
  factory reset now trying to enable bridge mode for my apple airport 6th gen"
  → U415 pin-8 factory reset + bridge-mode goal.
- "uart for j3 and j4 are set to 0sec u-bootloader" → 0-second autoboot (why no
  console interrupt was possible).

## 88bcea88-7422-4087-b6c6-8cf9c0fcd351 — "Hardware Hack Modem Bridge Mode Warning" (2026-03-31)
- "tell me to tell claude to provide me with step by step directions to hardware
  hack my xfinity arris modem to stop destroying my internet because its in
  bridge mode" → bridge-mode + interference context.
- "i need you to go fact check what exactly is happening over radio and how the
  xfinity modem is basically a jammer" → co-channel interference framing
  (his words; the "jammer" language is his, kept out of the README).

## 7d08ae91-5177-4809-896c-05591a9ad480 — (2025-08-14)
- James pasted the third-party writeup "Xfinity XB3 hardware mod: Disable WiFi
  and save 2 watts" (Arris TG1682P, board labeled TG1682/TG2472, two shielded
  WiFi modules, TPS54328 "54328" buck regulator, EN pin 1 grounding mod,
  14.9 W → 12.5 W). Used in the README **only** as a referenced published mod,
  explicitly marked as researched-not-performed.

## 9b166cae-49d7-471f-ae89-ae9a9b96a9ac — "Network Setup Troubleshooting" (2026-04-13)
- Corroborating Glasgow UART capture of the same boot log; "or sever pin8 on u415".

## Deliberately excluded
- "turn off the wifi jammers" / SSH-and-telnet-backdoor speculation
  (264001f2 #176): vague, unverified — left out.
- JTAG via J4 as an accomplished fact: mapped by James, never confirmed working.
- Any power-optimization framing: the Wi-Fi work was about RF interference.
- Full 33 KB boot log: trimmed to a redacted excerpt (MACs, hostname redacted).
- Assistant-drafted prompts (e.g. the Claude U.FL-antenna-disconnect instructions
  in 88bcea88 #56): never executed per the archive — left out.
