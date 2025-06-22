# Linux Kernel Module: Multiplier Driver for Zybo FPGA

This project is a simple character device driver developed for a custom hardware multiplier peripheral on a Zybo FPGA board. The goal is to integrate a custom multiplier IP into the Linux kernel running on the Zynq (ARM Cortex-A9) processing system

## Overview

Using Vivado, I built a Zynq-based SoC with a custom multiply peripheral and configured it to run a Linux system. I then wrote a Linux kernel module (`Multiplier.c`) that maps to the hardware peripheral using memory-mapped I/O and exposes it via a character device `/dev/multiplier`.

A test program (`DevTest.c`) in user space exercises the driver by writing two integers to the hardware, triggering the multiplication, and reading back the operands and result.

---

## File Descriptions

### `Multiplier.c`
- A Linux kernel module that:
  - Maps the physical address of the multiply hardware using `ioremap`.
  - Implements standard character device operations (`open`, `read`, `write`, `release`).
  - Accepts two 32-bit integers via `write()`, writes them to the hardware registers.
  - Reads the two operands and result from the hardware via `read()`.

### `DevTest.c`
- A user-space program that:
  - Opens `/dev/multiplier`.
  - Loops through all integer pairs from 0 to 16.
  - Writes the operands to the device and reads back the result.
  - Prints the multiplication result and verifies correctness.
  - Allows quitting early via user input (`q`).
