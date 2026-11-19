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
