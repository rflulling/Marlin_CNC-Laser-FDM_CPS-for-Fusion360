# Marlin Command Catalog (TMC, CNC, General)

Notes collected from MarlinFW documentation and current Marlin sources (2026-01) to support the Fusion360 posts.

## TMC Driver G-codes

| Code | Purpose | Notes |
| --- | --- | --- |
| `M122` | TMC debug | Reports status; `S1/I1` offer live/verbose dumps when `TMC_DEBUG` is enabled. |
| `M906` | Motor RMS current | Per-axis current in mA. |
| `M569` | StealthChop/SpreadCycle | Switch stepping mode per axis. |
| `M911` | OT pre-warn threshold | Set temperature pre-warn trigger. |
| `M912` | Clear OT pre-warn | Clears flag after cooling. |
| `M913` | Hybrid threshold | Speed at which StealthChop swaps to SpreadCycle. |
| `M919` | Chopper timing | Fine-tune chopper frequency/timing. |
| `M914` | StallGuard bump sensitivity | Sensorless homing sensitivity. |
| `M920` | Homing current | Lower current during homing. |
| `M915` | Z StallGuard calibration | Align Z steppers via StallGuard. |
| `M910` | TMC reset (platform dependent) | Forces driver reset where supported. |
| `M917`/`M918` | PWM amplitude/gradient | Rarely exposed; fine-tunes StealthChop tone. |
| `M908`/`M909` | Legacy digipot | DAC current set/report for hybrid boards. |

## General motor/motion commands that affect TMC behavior

- `M17`, `M18`/`M84` – Enable/disable steppers (activates hold current).  
- `M350` – Set microstepping (UART/SPI required on TMC).  
- `M500` / `M503` – Save and report current settings, including TMC values.  
- `M80` – ATX power on (reinitializes drivers if power-cycled).  
- `M410` – Quickstop; use on TMC fault/OT alerts.  
- `G34` – Z auto-alignment (can leverage StallGuard feedback).  

## CNC-focused commands

- `M3`/`M4`/`M5` – Spindle or laser power direction/off.  
- `M7`/`M8`/`M9` – Coolant / mist / disable (when enabled in Marlin CNC mode).  
- `G38.x` – Probe moves for touch-off (common on CNC builds).  
- `G53` – Move in machine coordinates; helpful with fixed fixtures.  

## Other non-FDM / utility commands

- `M106`/`M107` – Fan control (often reused for laser power when mapped).  
- `M225` – Wait for finish (useful before power-off or TMC mode swaps).  
- `M115` – Firmware capabilities (reveals enabled TMC features).  
