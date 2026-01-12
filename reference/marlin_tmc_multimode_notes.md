# Marlin TMC Notes & MultiMode Guidance

## Firmware configuration touchpoints

- Enable UART/SPI with correct `TMC{driver}_CS_PIN` or `SERIAL_TX/RX` wiring; set `TMC_USE_SW_SPI` if needed.  
- `TMC_DEBUG` exposes live `M122` sub-commands (`S1/I1`) and verbose register dumps.  
- Optional features to toggle based on hardware: `HYBRID_THRESHOLD`, `STEALTHCHOP_XY`, `SENSORLESS_HOMING`, `MONITOR_DRIVER_STATUS`, `IMPROVE_HOMING_RELIABILITY`.  
- Save tuned values with `M500` after issuing `M906`, `M913`, `M914`, or `M920`.  

## Suggested Marlin_MultiMode.cps usage

- **TMC Driver Setup (startup block):** insert persistent current/mode lines such as `M906 X800 Y800 Z900` and `M569 S1 X Y` (`S1` = StealthChop, `S0` = SpreadCycle) after power-on, matching your machine’s cooling and torque needs.  
- **Homing:** add `M920` to drop current and `M914` for bump sensitivity before `G28/G38`.  
- **Dynamic TMC Adjustments:** keep conservative templates (e.g., `M906 X{X} Y{Y}` where `{X}/{Y}` are filled by the MultiMode dynamic thresholds, such as `X900`/`Y850`) with rate limiting enabled; prefer SpreadCycle for heavy cuts and StealthChop for travel/finishing moves.  
- **Header clarity:** echo the chosen TMC lines in the generated G-code header so operators can verify at a glance.  

## Integration proposals (beyond vanilla Marlin behavior)

| Rank | Idea | Benefit |
| --- | --- | --- |
| Basic | Emit a preflight block (`M115`, `M122`, `M503`) and halt on missing UART/SPI replies. | Early detection of wiring/overtemp before motion. |
| Standard | Auto-tune homing: temporarily apply `M920` + `M914` for homing moves, then restore saved currents via templated `M906`. | Safer sensorless homing with predictable recovery. |
| Advanced | Feed-aware current and mode shaping: scale `M906`/`M569` using planned feed/accel windows with minimum dwell between updates. | Higher torque for roughing, quieter travel, reduced heat and missed steps. |
| Advanced | Opportunistic chopper retune: issue guarded `M919` presets when coolant/ambient temp changes are detected via user variables. | Stabilizes noise/temperature across long CNC jobs. |
| Advanced | Fault-aware quickstop macro: combine `M410` with a re-home and state replay of last applied TMC settings. | Faster, safer recovery after OT or stall events. |
