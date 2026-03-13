# Staircase Auto-Lighting System

> Motion-triggered LED staircase lighting - built with an ESP32,
> two PIR sensors, and an LED strip.

---

## Demo

<!-- Photo or GIF of the system in action -->
*Coming soon*

---

## Overview

This project adds automatic lighting to an unlit interior staircase (~10 steps)
in an apartment. The core problem: navigating between rooms at night with no
visibility. The goal was a lighting system that activates on motion, stays on
briefly, then fades out - soft enough not to disturb sleep, automatic enough
to require zero interaction.

The system uses two PIR sensors (one at each end of the staircase) connected
to an ESP32 microcontroller. When motion is detected during nighttime hours,
the ESP32 drives a 12V warm white LED strip via a MOSFET, with smooth fade-in
and fade-out controlled through PWM. A single 12V power supply feeds the whole
system, with a 7805 linear regulator stepping down to 5V for the ESP32 and
sensors.

---

## Features

- Motion detection via two PIR sensors (top and bottom of staircase)
- Easily extensible to additional PIR sensors
- Automatic activation during configurable nighttime hours only
- LED strip lights up on motion, turns off after a set delay

---

## Hardware

| Component | Role |
|-----------|------|
| ESP32 | Main microcontroller |
| HC-SR501 PIR (×2) | Motion detection — top and bottom of staircase |
| MOSFET IRLZ44N | High-current driver for the LED strip |
| L7805CV regulator | Steps 12V down to 5V for ESP32 and sensors |
| 12V 3A wall adapter | Main power supply |
| Warm white LED strip 3000K (×2) | Indirect lighting under each step |
| AWG22 wire - 20m | PIR to main enclosure + inter-step runs |

→ Full component list and rationale in [`docs/hardware.md`](docs/hardware.md)

---

## Wiring

Quick wiring overview or thumbnail schematic.

→ Detailed diagrams in [`docs/wiring.md`](docs/wiring.md)

---

## Firmware

What the code does, in plain language.

→ Full explanation in [`docs/firmware.md`](docs/firmware.md)

---

## Getting Started

*To be completed once the project is finalized.*

---

## Project Structure

```
/README.md
/docs/
  hardware.md
  wiring.md
  firmware.md
/src/
/cad/
```

---

## License