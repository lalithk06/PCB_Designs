#🖥 Relay Driver PCB (Single Channel)

	A compact and reliable single-channel relay driver circuit designed in KiCad, suitable for MCU-based automation projects. 

---

##📌 Overview

	This PCB allows an MCU (like Arduino, ESP32, or Raspberry Pi) to control high-voltage or high-current loads using an electromagnetic relay.

It integrates:

	-MCU-compatible input (logic level control)

	-Flyback diode protection

	-LED status indicator

	-Screw terminals for easy load connection

---

##🔄 Working
1. **Idle State (MCU LOW):**

	-The MCU output pin is LOW.

	-The transistor (BC547) remains OFF, preventing current flow through the relay coil.

	-The relay is de-energized, and the COM terminal is connected to NC by default.

	-The LED indicator is OFF.

2. **Active State (MCU HIGH):**

	-The MCU output pin goes HIGH (logic signal sent).

	-The base resistor (R1) limits current to the transistor base, turning it ON (saturation mode).

	-The transistor conducts, energizing the relay coil.

	-The relay switches from NC to NO, connecting the load between COM and NO.

	-The LED lights up, indicating relay activation.

3. **Flyback Diode Protection:**

	-When the MCU signal turns OFF, the coil de-energizes, creating a voltage spike.

	-The flyback diode (1N4007) safely dissipates this energy, protecting the transistor and MCU.

---

##🔧 Features

✅ Supports **5V or 12V relays** (SANYOU SRD series)

✅ **Transistor driver stage (BC547)** for MCU signal amplification

✅ **Flyback diode (1N4007)** for relay coil protection

✅ **LED indicator** for relay ON/OFF status

✅ **Screw terminal output** for load switching (COM, NO, NC)

✅ Compact **single-layer PCB** (ideal for hobby or educational use)

---

##🔌 Connections
###Power Header (J1)
	Pin 1: VCC (5V or 12V depending on relay)
	Pin 2: GND

###MCU Input Header (J2)
	Pin 1: MCU_IN (GPIO from MCU)
	Pin 2: GND (common reference with MCU)

###Relay Output Header (J3)
	Pin 1: COM (Common terminal)
	Pin 2: NC (Normally Closed)
	Pin 3: NO (Normally Open)

---

##🖼 Schematic
(In Progress)

---

##⚙ Specifications

	-**Relay Type:** SANYOU SRD (5V or 12V)

	-**Control Voltage:** MCU-compatible (3.3V or 5V logic)

	-**Load Voltage:** Supports AC (up to relay rating) or DC loads

	-**Transistor:** BC547 (NPN driver)

	-**Flyback Diode:** 1N4007 (across coil)

