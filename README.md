# Smart-self-heating-lunchbox
A closed-loop, ESP32-controlled heating system for a self-heating lunch box, built as the **Feature Enhancement Design** project for the Innovation & Design Thinking Lab (LP205 / 1BIDTL258), Dept. of EEE, The National Institute of Engineering (NIE), Mysuru.

**Team S24** · 2nd Semester EEE · Supervised by Asst. Prof. Smrithi Vijiyan

![Block Diagram](<img width="1220" height="670" alt="Banner_B5_The_Climb_Uncut" src="https://github.com/user-attachments/assets/3deacd2c-4093-477f-a1f9-69bb2b23c143" />
)

## Overview

The system keeps food at a set temperature through automatic relay-based heating, using an ESP32 as the central controller:

1. **Read** — ESP32 reads temperature from a DS18B20 digital sensor over the 1-Wire protocol (GPIO 4).
2. **Compare** — the reading is checked against a user-defined setpoint stored in firmware.
3. **Control** — if temperature is below setpoint, ESP32 GPIO 23 drives a 5V relay ON, energizing a 12V cartridge heater; once the setpoint is reached, the relay turns off.
4. **Power** — an LM2596 buck converter steps the 12V adapter input down to a regulated 5V rail for the ESP32 and relay module.

The loop runs continuously with no manual intervention, and status/setpoint can be checked via the ESP32's serial monitor or built-in web server.

## Why this design

This is an evolution of an earlier prototype, **LunchLab S24** — an Arduino Nano + NTC thermistor + 18650 Li-ion + nichrome-coil steam-heating design. That approach had no digital sensing, no safety cutoff logic, and relied on generating steam directly inside the box.

![Design Comparison](<img width="692" height="461" alt="design-comparison" src="https://github.com/user-attachments/assets/cee7f991-6e5c-48a2-8a02-c681c6767ae8" />
)

The ESP32 + DS18B20 + cartridge-heater redesign trades the steam mechanism for direct thermal contact, adds a real closed-loop firmware cutoff instead of a mechanical water-level probe, and gains Wi-Fi monitoring — at a comparable or lower component cost.

## Circuit design

![KiCad Schematic](<img width="1229" height="820" alt="schematic-kicad" src="https://github.com/user-attachments/assets/21bb40f5-9845-4cff-8993-aef92a702739" />
)

| Component | Part | Function |
|---|---|---|
| Microcontroller | ESP32-WROOM-32 (DevKit) | Central controller — reads sensor, drives relay, hosts web server |
| Temperature sensor | DS18B20 | 1-Wire digital sensor, GPIO 4, 4.7 kΩ pull-up to 3.3V |
| Relay module | 5V, 1-channel, 30A | Switches the 12V heater circuit, driven by GPIO 23 |
| Buck converter | LM2596 | Steps 12V input down to a regulated 5V rail |
| Heating element | 12V cartridge heater | Primary heat source, switched via relay NO terminal |
| Power supply | 12V DC adapter, 2A+ | Main input power |

**Key wiring notes:**
- ESP32 and the relay module share a common ground — required for correct relay switching.
- LM2596 output must be trimmed to exactly 5V before connecting the ESP32/relay.
- The relay's NC terminal is unused; only NO (normally open) is wired to the heater.

Full pin-by-pin connections are in the project report.

## Bill of materials (~₹900)

| Part | Qty | Cost (₹) |
|---|---|---|
| ESP32 (38-pin dev board) | 1 | 350 |
| DS18B20 temperature sensor | 1 | 50 |
| LM2596 buck converter | 1 | 60 |
| 5V 1-channel relay module | 1 | 40 |
| 12V DC cartridge heater | 1 | 200 |
| 12V DC adapter (2A+) | 1 | 150 |
| SPST rocker power switch | 1 | 20 |
| 4.7 kΩ resistor + jumper wires | 1 | 32 |
| **Total** | | **902** |

## Hardware build

![Hardware build](assets/hardware-build.jpg)
![Demo](assets/demo-poster.jpg)

## My contribution

I wrote the ESP32 firmware — temperature sensing over 1-Wire and the threshold-based relay control logic for the heater. Hardware assembly and wiring were handled by other team members.

## Team S24

Tejaswini MK · Sameeksha Krishna · Prarthana RN · Poorna Teja · Pranav R Bhat · Shrikanth · Prithviraj Chavan

## Future enhancements

- Mobile app for remote monitoring/setpoint adjustment over Wi-Fi
- OLED display for on-device temperature/status readout
- Multiple programmable temperature profiles
- LiPo battery backup for portable, off-grid operation
- Custom PCB (currently breadboard prototype)
- Hardware over-temperature cutoff (thermal fuse) as a secondary failsafe

## References

- [ESP32 Technical Reference Manual](https://www.espressif.com/en/products/socs/esp32)
- [DS18B20 Datasheet](https://www.analog.com/en/products/ds18b20.html)
- [LM2596 Datasheet](https://www.ti.com/product/LM2596)
