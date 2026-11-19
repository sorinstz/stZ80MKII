# stZ80 MKII

**A real Z80, Amstrad CPC compatibility, and room for the ZX Spectrum and beyond.**

stZ80 MKII is an FPGA-based retrocomputing platform built around a real Zilog Z80 CPU.

Its primary goal is to recreate the hardware environment required for Amstrad CPC compatibility, combining original processor hardware with FPGA-based logic and peripherals.

Rather than being a complete software emulation, stZ80 MKII takes a hardware-oriented approach: a real Z80 CPU, dedicated memory and I/O interfaces, and an FPGA providing the supporting hardware architecture.

## The Platform

The platform is designed around three main principles:

- **Real hardware:** A physical Z80 processor at the heart of the system.
- **Amstrad CPC compatibility:** The primary target, recreating the essential hardware environment of the original machine.
- **Expandability:** An FPGA-based architecture that leaves room for additional hardware capabilities, including ZX Spectrum compatibility and future extensions.

## stZ80 MKII v1.1 - Platform capabilities

CPU and FPGA
- Physical Z80 CPU paired with a reprogrammable FPGA development module
  on two 48-pin headers. The FPGA handles memory mapping and peripherals
  while the Z80 runs the application code.
- Buffered interfaces between the 5 V Z80 bus and 3.3 V FPGA logic.
- FPGA-generated CPU and sound clocks, reset sensing and drive, and
  control of Z80 wait states and maskable interrupts.

Memory and storage
- 512 KiB SRAM and 512 KiB parallel flash. Five FPGA-controlled bank
  lines (RAM0-RAM4) select among 32 physical 16 KiB pages per device.
  The FPGA configuration determines which pages the Z80 can see.
- FPGA-controlled flash write-enable for in-circuit ROM updates with
  suitable firmware.
- CompactFlash storage through a 40-pin IDE-style adapter connector.
  The FPGA can use CompactFlash for direct sector access or emulate a
  floppy drive using Standard DSK images, without a physical drive.
  Extended DSK (EDSK) images are not supported.

Display, input and sound
- FPGA-driven analog VGA with 3-bit red, 3-bit green and 2-bit blue
  (256 RGB combinations), plus separate horizontal and vertical sync.
  The FPGA can provide programmable display timings, indexed-color
  graphics, on-screen status overlays and scanline effects.
- Two independent PS/2 channels on one J5 socket for a keyboard and mouse
  via a correctly wired combo-port splitter. Both channels share power
  and ground but have separate clock and data lines.
- The FPGA can translate keyboard and joystick input into a key matrix
  for legacy software, or use keyboard shortcuts to trigger a reset.
- YM2149F three-channel programmable sound generator with a selectable
  clock divider, analog audio jack, and a mono LM386 amplifier with a
  volume trim and an internal speaker header. Automatic speaker
  muting when headphones are plugged.
- Atari-style DB9 joystick input and jumper-configurable second joystick
  support. The FPGA can use the YM2149F's other I/O port to select the
  active joystick through JGND0/JGND1. That port is also available on
  the 8-pin P4 header for other I/O when not used for joystick selection.

Connectivity and expansion
- FT232RL USB-to-UART for a serial terminal or firmware diagnostics.
  RDIN is FPGA receive; TDOUT is FPGA transmit. This is separate from
  the main USB-C power input and is not an RS-232-level port.
- FPGA I2C bus on P6 (SDA, SCL, power and ground), with SDA/SCL also
  broken out on J10. An external DS3231 module provides battery-backed
  time and date and a temperature sensor. The FPGA can make these
  readings available to Z80 software and gate writes behind an explicit
  unlock. Other compatible I2C devices can share the bus.
- 50-pin CPC-style expansion header (J4) carrying the Z80 address and
  data buses, clock, interrupt and bus-request/acknowledge signals,
  RAM/ROM disable and mono audio. Check the pinout against a specific
  CPC backplane before connecting it.

Power and case connections
- Power-only 5 V USB-C input with a fuse, an on/off switch and an onboard
  3.3 V regulator.
- Front-panel reset, power LED and disk activity LED headers, plus a
  dedicated current-measurement header.

Applications
- Amstrad CPC 464/6128: listed as implemented in the project notes.
- ZX Spectrum and custom Z80/FPGA systems: possible targets requiring
  their own FPGA logic and firmware.

  
## Hardware

The stZ80 MKII hardware is the foundation of the project.

This repository provides the manufacturing files and technical documentation required to understand and reproduce the published board revision, subject to the accompanying hardware license.

Hardware design files are proprietary and are not open source.

See [`hardware/`](hardware/) for available documentation and manufacturing files.

## FPGA Project

The FPGA implementation provides the programmable logic that supports the platform and its Amstrad CPC compatibility.

The FPGA project will be published separately within this repository.

Once released, the FPGA source will be open for inspection, modification and community contributions, provided that implementations remain compatible with the stZ80 MKII hardware platform and comply with the FPGA project's license.

See [`fpga/`](fpga/) for project information.



## Project Structure

- `hardware/` — Board documentation, manufacturing files and hardware licensing.
- `fpga/` — FPGA implementation and related documentation (coming later).
- `assets/` — Project images and visual resources.

## Project Status

The hardware platform is the current focus of development.

The FPGA implementation and Amstrad CPC compatibility work will be progressively published as the project develops.

## License

The hardware design is proprietary and distributed under its own license.

The FPGA implementation will have a separate open-source license when published.

See the respective project directories for applicable licensing terms.
