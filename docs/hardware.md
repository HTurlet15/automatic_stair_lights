# Hardware

## Component List

| Component | Qty | Est. Cost (CAD) | Role |
|-----------|-----|-----------------|------|
| ESP32 ELEGOO 38 pins | 1 | ~10$ | Main microcontroller |
| HC-SR501 PIR | 2 | ~3$ | Motion detection |
| MOSFET IRLZ44N | 1 | ~1$ | High-current driver for LED strip |
| L7805CV regulator | 1 | ~1$ | Steps 12V down to 5V |
| 12V 5A wall adapter | 1 | ~13$ | Main power supply |
| Warm white LED strip 3000K | 2 | ~15$ | Indirect lighting under each step |
| Clipper connectors 2-pin 10mm | 20 | ~10$ | Inter-step LED connections |
| AWG22 wire - 20m | 1 | ~4$ | PIR to enclosure + inter-step runs |
| Mini PCB prototype board | 2 | ~3$ | Permanent component mounting |
| Screw terminals 2-pin | 10 | ~2$ | Clean wire entry points on enclosure |
| **Total** | | **~62$ CAD** | |

### Tools (not counted in project cost)

| Tool | Role |
|------|------|
| Soldering iron + solder | Permanent connections on PCB and LED strips |
| Breadboard 830 pts | Prototyping phase |
| Jumper wires M-F | Prototyping phase |

---

## Physical Architecture
```
[Wall outlet]
      ↓ mains cable
[12V adapter] ←── 3D-printed wall pocket
      ↓ 5.5mm DC cable
[Main enclosure] ←── ESP32 + MOSFET + L7805 + screw terminals
      ↓ AWG22 wire
[LED strips under each step]

[PIR enclosure - top] ←── fixed at top of staircase
[PIR enclosure - bottom] ←── fixed at bottom of staircase
      ↓ AWG22 wire
[Main enclosure]
```

---

## Component Rationale

### Microcontroller - ESP32

The ESP32 was chosen over three common alternatives:

| Option | Reason rejected |
|--------|----------------|
| Arduino Uno | No native WiFi, PWM limited to specific pins |
| Raspberry Pi | Overkill for this use case, slow boot, unnecessary overhead |
| Pi Pico | Smaller community, fewer libraries |

The ESP32 hits the right balance: flexible PWM on almost any pin, built-in
WiFi for potential future extensions (remote config, home automation), dual
cores, and a price point identical to an Arduino Uno.

---

### Motion Sensor - HC-SR501 (PIR)

PIR sensors detect *changes* in infrared radiation, not heat itself - meaning
they trigger on a moving warm body, not a static one.

Two units are used: one at the top of the staircase, one at the bottom.
Either sensor can independently trigger the lighting, covering both directions
of travel.

Key characteristics:
- **Output**: digital signal (0V / 3.3V), directly readable by the ESP32
- **Power draw**: low, compatible with the 5V regulated supply
- **Physical adjustments**: two onboard potentiometers for sensitivity and
  delay duration
- **Trigger mode**: set to *repeat trigger* - output stays HIGH as long as
  motion is detected, resetting the timer on each new detection

---

### LED Strip - Warm White 3000K

A 3000K warm white strip was chosen deliberately: cool white at night is
visually aggressive. 3000K is close to candlelight - far easier on
dark-adapted eyes.

12V was preferred over 5V for two reasons: better voltage stability over
the length of the strip, and it is the standard for interior LED installations.
The strip is cuttable at marked intervals and reconnectable via clipper
connectors or solder, making it straightforward to fit any staircase geometry.
Strips are mounted under each step for indirect, diffused light.

---

### MOSFET Driver - IRLZ44N

The ESP32's GPIO pins are current-limited to ~20mA - far below the 1–3A
drawn by the LED strip. A MOSFET bridges this gap.

The IRLZ44N acts as an electronically controlled switch: a low-power PWM
signal on the Gate controls a high-current path from Drain to Source.
It was selected specifically because its Gate threshold voltage is compatible
with 3.3V logic - many MOSFETs require 5V or more to switch fully on.

---

### Power Supply - 12V 5A + L7805 Regulator

A single 12V 5A wall adapter powers the entire system: one mains cable, one
shared ground reference across all components.

The L7805 linear regulator steps 12V down to a stable 5V rail for the ESP32
and PIR sensors. The adapter accepts 100–240V AC input, making it compatible
with North American outlets without modification.

---

### Permanent Wiring - Mini PCB + Screw Terminals

The breadboard used during prototyping is replaced by a mini PCB prototype
board for the final installation. Components are soldered directly onto it,
eliminating loose connections.

Screw terminals provide clean, removable connection points for the AWG22 wires
running to the LED strips and PIR sensors - no soldering required when
connecting or disconnecting field wiring.