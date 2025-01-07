---
title: "Electrical Theory"
description: ""
summary: "Basic electrical theory and formulas."
date: 2024-10-30T22:50:36-07:00
lastmod: 2024-10-30T22:50:36-07:00
weight: 999
toc: true
tagger: ["Electronics", "Theory"]
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Pipe Analogy

The basic components of electricity - voltage, current, and resistance - can be expressed using an analogy of water flowing through a pipe.

| Type       | Symbol | Unit       | Measure of                                                           | Pipe Analogy          |
| ---------- | ------ | ---------- | -------------------------------------------------------------------- | --------------------- |
| Voltage    | V      | Volt (V)   | Potential energy per unit charge                                     | Water pressure        |
| Current    | I      | Ampere (A) | Flow of electrical charge though circuit                             | Flow of water in pipe |
| Resistance | R      | Ohm (Ω)    | Opposition that a circuit presents to the flow of electrical current | Narrowness of pipe    |

### Voltage

Voltage is the measure of electric potential energy per unit charge that is present in a circuit. It is often compared to the pressure in a water pipe. Just as a high-pressure pipe can push water through a hose, a high voltage can push electrical current through a circuit.

The unit of voltage is the volt (V), and it is measured using a voltmeter.

```bash
Voltage = Current * Resistance
V = I * R
```

### Current

Current is the flow of electrical charge through a circuit. It is often compared to the flow of water in a pipe. Just as a high flow rate in a pipe can push more water through, a high current can push more electrical charge through a circuit.

The unit of current is the ampere (A), and it is measured using an ammeter.

```bash
Current = Voltage / Resistance
I = V / R
```

### Resistance

Resistance is a measure of the opposition that a circuit presents to the flow of electrical current. It is often compared to the narrowness of a pipe that restricts the flow of water. The higher the resistance in a circuit, the more difficult it is for current to flow through.

The unit of resistance is the ohm (Ω), and it is measured using an ohmmeter.

```bash
Resistance = Voltage / Current
I = V / R
```

### Ohm's Law

Ohm's law states that the current through a conductor between two points is directly proportional to the voltage across the two points. Mathematically, it can be expressed as:

```bash
I = V / R
```

Where I is the current in amperes, V is the voltage in volts, and R is the resistance in ohms.

This formula can be rearranged to solve for any of the three variables:

```bash
V = I * R
R = V / I
I = V / R
```

### Conclusion

Understanding the basics of electrical concepts such as voltage, current, and resistance is crucial to designing and maintaining effective circuits. By understanding how these concepts relate to each other through Ohm's Law, engineers can design circuits that are efficient and effective.

## Measurements and Formulas

{{< picture src="images/electrical/triangle-ohms-law.jpg" class="imgWidth40" alt="A bird flying through a field of tall grass" >}}

- Where `V` is volts, `I` is current/amps, `R` is resistance/ohms

### Quick Reference Chart

| Measurement | Unit          | Symbol | Denoted  | Unit of              | Formula             |
| ----------- | ------------- | ------ | -------- | -------------------- | ------------------- |
| Voltage     | Volt          | V      | V or E   | electrical potential | `V = I • R`         |
| Current     | Ampere        | I      | I or _i_ | electrical current   | `I = V / R`         |
| Resistance  | Ohm           | Ω      | R        | DC current           | `R = V / I`         |
| Conductance | Siemen or Mho | ℧      | G or S   | conductance          | `G = 1 / R`         |
| Power       | Watts         | W      | P        | power                | `P = V • I`         |
| Capacitance | Farad         | F      | C        | capacitance          | `C = Q / V`         |
| Inductance  | Henery        | H      | L        | inductance           | `VL = -L (di / dt)` |
| Impedance   | Ohm           | Z      | Z        | AC resistance        | `Z2 = R2 + X2`      |
| Charge      | Colomb        | Q      | Q        | electrical charge    | `Q = C • V`         |
| Frequency   | Hertz         | Hz     | _f_      | frequency            | `F = 1 + T`         |
| Period      | Second        | s      | T        | period               | `T = 1 / f`         |

## Voltmeter

### Select Input Port

- `INPUT` Port - Used for voltage, resistance, and other general measurements. This is the correct port for voltage measurements.
- `10A` Port: Used for measuring high currents (up to 10A). Not used for voltage measurements.
- `µA mA` Port: Used for measuring small currents in microamperes (µA) or milliamperes (mA). Not used for voltage measurements.

### Set Dial

| Measurement | Rotary Position  | Port     |
| ----------- | ---------------- | -------- |
| DC Voltage  | V⎓               | INPUT    |
| AC Voltage  | V~               | INPUT    |
| Current     | µA, mA, or A     | µA or mA |
| Resistance  | Ω                | INPUT    |
| Continuity  | Ω (Press Select) | INPUT    |
| Diode       | Ω (Press Select) | INPUT    |
| Transistor  | hFE              | INPUT    |
| Capacitance | -\|(-            | INPUT    |
| Frequency   | Hz               | INPUT    |

- With DC voltage, the `-` indicates polarity. When the `-` is absent, the red lead is connected to the positive.
- Do not test AC voltage on DC mode and vice versa - this can damage the meter or device.

Resistance

- If the resistance exceeds 1 MΩ, the meter may take several seconds to stabilize. This is okay.
- Do not change the resistance when taking readings.

### Buttons

**Select**

Selects a more specific function. Used for the rotary switch where there are multiple icons for one position.

**Relative**

Remove the resistance of the test leads to give a more accurate result. A small triangle will appear when activated and the reading should change to 0.

**Max/Min**

Press once to enter Max mode. Press again to enter Min mode. Press and hold to exit Max/Min mode.

**Range Button**

For AC/DC voltage, DC current, and resistance is measured automatically by default. Press button repeatedly to reach the desired range.
