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

This board was fabricated through JLCPCB, a global PCB manufacturer.

- 👉 Order: https://jlcpcb.com/?from=DPM
- $2 for 5 PCBs
- Fast turnaround with strong silkscreen and solder mask quality
- SMT assembly and advanced board options available

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

### Example

```cpp
ina226.setResistorRange(0.005, 15.0);
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
