# stZ80 MKII Hardware

**A real Z80, Amstrad CPC 464/6128 compatibility, and an FPGA platform for future cores such as the ZX Spectrum.**

The stZ80 MKII uses a physical Z80 CPU and a reprogrammable FPGA for video, memory banking, bus control, and peripherals. The board design stays fixed so FPGA improvements can run on boards people have already built.

## Hardware at a glance

- **512 KiB SRAM + 512 KiB flash**, with 16 KiB paging and Dk'tronics-style RAM banking.
- **5 V Z80 bus and 3.3 V peripheral bus**, bridged by FPGA-controlled transceivers.
- **RGB332 VGA**, PS/2 keyboard input, and Atari-style DB9 joystick input.
- **YM2149F audio** with an audio jack and an LM386 internal speaker amplifier.
- **CompactFlash storage**, USB serial via FT232RL, and an I²C header for a DS3231 RTC module.
- **5 V USB-C power input** and a 50-pin CPC-style expansion header.

## CPC compatibility

The CPC core supports **CP/M and SymbOS**, USB serial auto-type, and CompactFlash-backed floppy emulation for **Standard DSK images**, including interleaved Vendor and Data layouts. Extended DSK and cassette are not supported. The ZX Spectrum is a future core target, not a feature of this release.

## Build and license

Use the **Gerber and drill files** for fabrication and the **schematic PDF** for reference. Editable PCB and schematic files are not included. Match your FPGA build to your board revision.

You may build and sell unchanged boards with attribution: **“stZ80 MKII hardware designed by Sorin Strizu.”** FPGA improvements are welcome as long as they work without hardware changes. Please submit hardware fixes for review rather than publishing a modified board.

See license.md for the full terms.
