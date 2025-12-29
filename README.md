# Mastering INA219 & INA226

## Accurate Power & Current Measurement for DIY Electronics

In DIY electronics, precise power insights decide whether a battery runs for months or drains in hours. The INA219 and INA226 current-sensing ICs deliver accurate voltage, current, and power data over I²C, making lab-grade measurements accessible to hobbyists.

## Hardware Architecture Overview

Both devices measure:
- Voltage drop across a shunt resistor
- Bus voltage
- Power internally

### INA219 — The Workhorse
- 12-bit ADC (4096 steps)
- 0–26 V bus voltage
- ±320 mV shunt range
- Ideal for 5 V / 12 V hobby projects

### INA226 — The Precision King 👑
- 16-bit ADC (65,536 steps)
- 0–36 V bus voltage
- ±81.92 mV shunt range
- Built-in averaging (up to 1024 samples)
- Programmable ALERT pin

## Shunt Resistor Physics (Critical!)

These ICs infer current via the shunt resistor:

```
Current = Shunt Voltage / Shunt Resistance
```

### Why R100 (0.1 Ω) Fails

| Current | Voltage Drop | Power Loss |
|---------|--------------|------------|
| 100 mA  | 10 mV        | 1 mW       |
| 10 A    | 1 V ❌      | 10 W 🔥     |

Results:
- INA226 saturates
- Resistor overheats
- Module fails

## High-Side vs Low-Side Sensing

- ✅ High-Side (recommended): Battery + → INA → Load +
- ❌ Low-Side (problematic): Ground lift, USB/I²C errors, ground loops
- ⚠️ INA ground must still tie to system ground

## Known Limitations and Mitigations

- Thermal drift at high current → Use larger shunts or external sensing
- Absolute voltage limits → INA219: 26 V; INA226: 36 V
- PWM aliasing → Enable hardware averaging, add RC filtering
- Inductive loads → Add flyback diodes

## Custom INA226 PCB (Why It Matters)

Reasons generic modules fail:
- Poor layout and thermal handling
- Fixed shunt values that limit measurement range

Key design improvements:
- Kelvin sensing with dual parallel shunts
- Differential RC filtering
- Large exposed copper with via stitching (top ↔ bottom)
- Clear silkscreen labeling for wiring clarity

## PCB Fabrication — JLCPCB

### Project Fabrication: Made Possible by JLCPCB

This project became reality thanks to [JLCPCB](https://jlcpcb.com/?from=DPM), a leading quick-turn PCB manufacturer trusted by hobbyists and engineers alike. Their rapid turnaround and consistent quality made them an ideal partner for fabricating the custom boards used here.

### Why We Chose JLCPCB

- Reliable production with professional finishes ready for immediate testing
- Accessible pricing — standard 1–4 layer PCBs start at just $2 for five boards
- High-quality outputs featuring sharp silkscreens, aligned solder masks, and clean drill hits

### Beyond Basic PCBs

[JLCPCB](https://jlcpcb.com/?from=DPM) has grown into a one-stop prototyping platform with:

- Advanced PCB options including high-frequency, flexible, and rigid-flex stacks
- SMT assembly services that accept BOM and pick-and-place files for turnkey builds
- Industrial-grade 3D printing (SLA, SLS, MJF) and CNC machining for mechanical parts

The collaborative experience deliver boards that worked on the first assembly pass, keeping the build schedule on track.

### How to Order This PCB

1. **Get the design files**: Download the Gerber ZIP archive from the project repository (look for Gerber_ProjectName).
2. **Upload to JLCPCB**: Visit [JLCPCB](https://jlcpcb.com/?from=DPM) and click Instant Quote, then drag the ZIP file into the uploader. Their tooling auto-detects dimensions and layer count.
3. **Choose settings**: We used the defaults — 2 layers, auto dimensions, green solder mask (fastest), and a quantity of 5 boards.
4. **Review and checkout**: Open Gerber Viewer to confirm the render, save to cart, and complete payment.
5. **Track delivery**: [JLCPCB](https://jlcpcb.com/?from=DPM) typically finishes fabrication within 24 hours, with shipment arriving in a few days depending on carrier.

Watch the [ordering process walkthrough](https://youtu.be/2KFvhB9MCYs) for a visual guide to the [JLCPCB](https://jlcpcb.com/?from=DPM) workflow.

### Quick Facts

- 👉 Order: [JLCPCB](https://jlcpcb.com/?from=DPM)
- $2 for 5 PCBs with quick-turn production
- Fast turnaround backed by crisp silkscreens and solder mask alignment
- SMT assembly, advanced PCB options, and mechanical fabrication under one portal

## Implementation

### Wiring

| Pin   | Connection        |
|-------|-------------------|
| VCC   | 3.3 V / 5 V       |
| GND   | Common ground     |
| SDA/SCL | MCU I²C         |
| VIN+  | Battery +         |
| VIN-  | Load +            |

### Libraries

- INA219: Adafruit_INA219
- INA226: INA226_WE, RobTillaart/INA226

### INA226 Libraries

- INA226_WE (by Wolfgang Ewald)
- RobTillaart/INA226

Usage examples:

```cpp
// INA226_WE: provide shunt resistance (Ohms) and expected max current (Amps)
ina226.setResistorRange(shuntResistance, maxCurrent);

// RobTillaart/INA226: same values, swapped argument order
ina.setMaxCurrentShunt(maxCurrent, shuntResistance);
```
## Conclusion

The INA226 delivers lab-grade accuracy when:
- The shunt value suits the measurement range
- Thermal effects are managed through layout and copper area
- The PCB follows analog best practices

Executed well, it becomes a professional-grade power measurement tool for DIY systems.

## Further Reading

- Hackaday Project: https://hackaday.io/project/204686-mastering-the-ina219-ina226
- Instructables – The Definitive Guide to Precision Power: https://www.instructables.com/The-Definitive-Guide-to-Precision-Power-Mastering-/
