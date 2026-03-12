# Hardware

## Microcontroller - ESP32

The ESP32 was chosen over three common alternatives:

| Option | Reason rejected |
|--------|----------------|
| Arduino Uno | No native WiFi, PWM limited to specific pins |
| Raspberry Pi | Overkill for this use case, slow boot, unnecessary overhead |
| Pi Pico | Smaller community, fewer resources and libraries |

The ESP32 hits the right balance: flexible PWM on almost any pin, built-in
WiFi for potential future extensions (remote config, home automation), dual
cores, and a price point identical to an Arduino Uno. It has become the
modern maker standard for good reason.

---

## Motion Sensor - HC-SR501 (PIR)

PIR sensors detect *changes* in infrared radiation, not heat itself - meaning
they trigger on a moving warm body, not a static one. This distinction matters
for tuning sensitivity and avoiding false positives.

Two units are used: one at the top of the staircase, one at the bottom.
Either sensor can independently trigger the lighting, covering both directions
of travel.

Key characteristics:
- **Output**: digital signal (0V / 3.3V), directly readable by the ESP32
- **Power draw**: low, compatible with the 5V regulated supply
- **Physical adjustments**: two onboard potentiometers for sensitivity and
  delay duration
- **Trigger mode**: set to *repeat trigger* - the output stays HIGH as long
  as motion is detected, resetting the internal timer on each detection

---

## LED Strip - Warm White 12V

A warm white strip (2700–3000K) was chosen deliberately: cool white light
at night is visually aggressive. Warm white is close to candlelight, far
easier on dark-adapted eyes.

12V was preferred over 5V for two reasons: better voltage stability over
the length of the strip, and it is the standard for interior LED installations.
The strip is cuttable at marked intervals and reconnectable via connectors
or solder, making it easy to fit any staircase geometry. Strips are mounted
under each step for indirect, diffused light.

---

## MOSFET Driver - IRLZ44N

The ESP32's GPIO pins are current-limited to ~20mA - far below the 1–3A
drawn by the LED strip. A MOSFET bridges this gap.

The IRLZ44N acts as an electronically controlled switch: a low-power PWM
signal on the Gate controls a high-current path from Drain to Source. Think
of it as a tap - a finger's pressure (Gate signal) controls full water flow
(LED current).

The IRLZ44N was selected specifically because its Gate threshold voltage is
compatible with 3.3V logic, which is what the ESP32 outputs. Many MOSFETs
require 5V or more to switch fully on.

---

## Power Supply - 12V 3A + 7805 Regulator

A single 12V 3A wall adapter powers the entire system. This simplifies the
installation: one mains cable, one ground reference shared across all
components.

The 7805 linear regulator steps 12V down to a stable 5V rail for the ESP32
and PIR sensors. The adapter is rated for 100–240V AC input, making it
compatible with North American outlets (120V) without modification.