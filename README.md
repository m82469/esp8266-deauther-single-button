# ESP8266 Deauther — Single-Button Port

Fork of [SpacehuhnTech/esp8266_deauther](https://github.com/SpacehuhnTech/esp8266_deauther) v2.6.1, adapted to run on a cheap "ESP8266 V3 + 0.96&Prime; OLED" dev board (CH340) that has **one physical button instead of four** and wires its display to non-standard I2C pins. Stock firmware just watchdog-reset-looped on this hardware; this fork fixes that and folds the UP/DOWN/A/B control scheme onto a single tap/hold gesture.

All credit for the actual deauther engine — scanning, attack logic, the menu/CLI/web framework — goes to [Spacehuhn](https://github.com/spacehuhntech) and contributors. See `LICENSE` (MIT).

## What's different from upstream

- **New board config** — `esp8266_deauther/A_config.h`, `ANYX_ESP8266_V3_OLED`: SSD1306 over bit-banged I2C on GPIO14 (SDA) / GPIO12 (SCL), flipped orientation, single button on GPIO0.
- **Single-button navigation** — `esp8266_deauther/DisplayUI.cpp`: tap advances the highlighted menu item (what UP/DOWN used to do), hold (800ms) opens/selects it (what A used to do). There's no dedicated back button — every submenu's own `[BACK]` entry is selected the same way, same as any other item.
- **RANDOM ROUTERS menu item** — `esp8266_deauther/SSIDs.cpp` (`addRandomRouterNames`), wired into the SSIDs submenu. Adds up to 20 beacon-only decoy SSIDs styled like common router defaults (`TP-Link_4F2A`, `Keenetic_C019`, …) to clutter nearby WiFi scanners. Cosmetic WPA2 flag only — these aren't backed by a real access point, nothing can actually connect to them or hand over a password.

## Hardware

| Pin | Function |
|---|---|
| GPIO14 (D5) | OLED SDA |
| GPIO12 (D6) | OLED SCL |
| GPIO0 (D3, FLASH button) | the only input — tap / hold |

## Controls

| Gesture | Does |
|---|---|
| Tap | Next item in the current list |
| Hold ≥800ms | Open / select the highlighted item |

## Password

The default config-AP password for `pwned` is `deauther` — change it (`set password <new>` over serial, or in the web UI) before relying on this for anything.

## Disclaimer

Proof-of-concept firmware for testing and education. Neither the ESP8266 nor its SDK was built for this — bugs can occur.

**Use it only against your own networks and devices, or with explicit written authorization.** Check the legal regulations in your country before transmitting. We take no responsibility for what you do with this program.