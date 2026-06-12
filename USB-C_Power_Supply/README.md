# USB-C Power Supply (5V / 9V / 12V / 20V / 28V)

A compact USB Type-C Power Delivery (USB-PD) trigger board capable of requesting fixed PDO voltages from a USB-C PD charger and providing a regulated **3.3V rail** for onboard logic. The board is based on the **CH224A USB PD Sink Controller** and supports both **DIP-switch voltage selection** and **I²C control**.

---

## Features

- USB Type-C Power Delivery Sink
- Selectable Output Voltages
  - 5V
  - 9V
  - 12V
  - 20V
  - 28V (USB PD 3.1 EPR)
- CH224A USB PD Sink Controller
- MCP16331 Buck Converter (5V–28V → 3.3V)
- TVS Surge Protection
- PMOS Reverse Polarity Protection
- USB Data Line ESD Protection
- Output Voltage Indicator LEDs
- Power Good / Error Indicator
- I²C Interface
- Screw Terminal Output
- Mounting Holes

---

# Board Overview

```
                USB-C PD Adapter
                        │
                        ▼
            USB-C Receptacle (J1)
                        │
      ┌─────────────────────────┐
      │                                 │
      │      TVS + PMOS Protection      │
      │                                 │
      └─────────────────────────┘
                        │
                     +V_USB
                        │
        ┌───────────────┴───────┐
        │                              │
        ▼                              ▼
    CH224A PD Controller         MCP16331 Buck
        │                              │
        │                           +3.3V
        │                              │
        ▼                              ▼
 Voltage Selection              LEDs, MC14051,
   Negotiation                  Logic & I²C
        │
        ▼
  Output Screw Terminal
```

---

# Hardware Architecture

## USB-C Input

The board receives power from a USB Type-C PD compliant charger.

### Components

- USB Type-C Receptacle
- TVS Protection
- PMOS Reverse Polarity Protection
- USBLC6 ESD Protection
- CH224A PD Controller

The USB connector provides:

- VBUS
- CC1
- CC2
- D+
- D-

---

## USB Power Delivery Controller

The CH224A negotiates the requested voltage with the USB PD source.

Unlike traditional USB-C sink implementations, **no external 5.1kΩ Rd resistors are required** because the CH224A integrates the complete USB sink detection circuitry internally.

---

# Voltage Selection

The requested PDO is selected using the CFG pins.

## DIP Switch Mode

Three DIP switches control:

- CFG1
- CFG2
- CFG3

| Output Voltage | CFG1 | CFG2 | CFG3 |
|---------------|------|------|------|
| 5V | X | X | X |
| 9V | 0 | 0 | 0 |
| 12V | 0 | 0 | 1 |
| 20V | 0 | 1 | 1 |
| 28V | 0 | 1 | 0 |

Where

- X = Don't Care
- 0 = GND
- 1 = Pull-up

---

# I²C Mode

The CH224A also supports I²C control.

The I²C connector exposes:

- SDA
- SCL
- GND

This allows an external MCU to dynamically request different PDO voltages without changing the DIP switches.

---

# Output

The negotiated USB PD voltage is available at:

- Screw Terminal Connector

Supported outputs

- 5V
- 9V
- 12V
- 20V
- 28V

> **Output current depends entirely on the connected USB PD adapter and USB-C cable.**

---

# 3.3V Power Supply

The onboard electronics operate from a dedicated 3.3V rail.

Power conversion:

```
5V–28V
      │
      ▼
 MCP16331
 Buck Converter
      │
      ▼
   3.3V Rail
```

The buck converter powers:

- CH224A
- MC14051
- Indicator LEDs
- I²C Interface

---

# Output Voltage Indicators

Five LEDs indicate the negotiated USB PD voltage.

| LED | Voltage |
|------|---------|
| LED1 | 5V |
| LED2 | 9V |
| LED3 | 12V |
| LED4 | 20V |
| LED5 | 28V |

Only one LED is illuminated at a time.

---

# Error Indicator

The CH224A provides a **Power Good (PG)** output.

The PG output is **Active Low**.

## Normal Operation

```
PG = LOW
```

The NMOS remains OFF.

The Error LED remains OFF.

---

## Error Condition

```
PG = HIGH
```

The NMOS turns ON.

The Error LED turns ON.

Possible causes include:

- USB PD negotiation failure
- Unsupported PDO request
- Charger removed
- Fault condition

---

# Protection Features

## TVS Protection

A TVS diode protects the VBUS line against:

- ESD
- Cable transients
- Voltage spikes

The TVS is placed immediately after the USB-C connector.

---

## Reverse Polarity Protection

A PMOS transistor provides reverse polarity protection.

Advantages

- Very low voltage drop
- Automatic recovery
- No fuse replacement required

---

## USB Data Line Protection

USBLC6 protects:

- D+
- D-

against:

- IEC ESD
- EFT
- Transient overvoltage

---

## CC Line Protection

22Ω series resistors improve robustness against:

- ESD
- Ringing
- Cable transients

---

# eMarker Simulation Mode

USB PD 3.1 requires an electronically marked cable for voltages greater than 20V or powers exceeding 60W.

The CH224A provides an **eMarker Simulation Mode**.

To enable:

Connect

```
1kΩ

CC2 → GND
```

This allows requesting

- 28V PDO

using a standard USB-C connector.

Refer to the CH224A datasheet for further details.

---

# LEDs

| LED | Function |
|------|----------|
| D2 | 5V Indicator |
| D3 | 9V Indicator |
| D4 | 12V Indicator |
| D5 | 20V Indicator |
| D6 | 28V Indicator |
| D1 | Error Indicator |

---

# Connectors

## J1

USB Type-C Input

---

## J2

Output Connector

```
+V_USB

GND
```

---

## J3

I²C Interface

```
SDA

SCL

GND
```

---

# Applications

- USB PD Bench Supply
- Embedded Development
- Rapid Prototyping
- Battery Charging
- Robotics
- Industrial Electronics
- Portable Instruments
- USB PD Testing
- IoT Projects

---

# Operating Procedure

## DIP Switch Mode

1. Connect a USB PD charger.
2. Configure the DIP switches for the desired voltage.
3. Apply power.
4. The CH224A negotiates the selected PDO.
5. Verify the output voltage using the indicator LED.
6. Use the output at J2.

---

## I²C Mode

1. Connect an external MCU.
2. Initialize the CH224A over I²C.
3. Send the desired PDO request.
4. Wait for negotiation to complete.
5. Monitor the PG signal.
6. Use the negotiated output voltage.

---

# Notes

- The output voltage is available only when connected to a USB PD compliant source.
- 28V operation requires a USB PD 3.1 EPR compatible adapter.
- Maximum available current depends on the USB PD source and cable capability.
- Use high-quality USB-C cables for high-power operation.
- The board does not generate voltages internally; it only negotiates and passes through supported USB PD voltages.

---

# Specifications

| Parameter | Value |
|-----------|-------|
| Input Connector | USB Type-C |
| USB PD Version | USB PD 3.1 |
| Supported PDOs | 5V, 9V, 12V, 20V, 28V |
| Controller | CH224A |
| Logic Supply | 3.3V |
| Buck Converter | MCP16331 |
| Voltage Selection | DIP Switch / I²C |
| Output Connector | Screw Terminal |
| Reverse Polarity Protection | PMOS |
| Surge Protection | TVS |
| USB ESD Protection | USBLC6 |
| Mounting | 4 × Mounting Holes |

---

# License

This hardware design is released under the MIT License unless stated otherwise.