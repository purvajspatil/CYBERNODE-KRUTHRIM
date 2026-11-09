# Bill of Materials (BOM)

This document lists the required hardware components, microcontrollers, modules, and passive parts needed to build the ESP32 Smart Room Automation system.

---

## Microcontroller and Processing Core

* **ESP32 Development Board:** Dual-core Wi-Fi/BLE microcontroller unit (ESP32-WROOM-32 or compatible node).

---

## Sensors and Input Devices

* **DS18B20 Digital Temperature Sensor:** Waterproof 1-Wire sensor for ambient thermal monitoring.
* **4.7k Ohm Resistor:** Pull-up resistor required for the DS18B20 1-Wire data line.
* **IR Receiver Module:** Infrared demodulator compatible with standard NEC protocol remote controls.
* **Tactile Push Switch:** Momentary push-button switch used for the physical night mode toggle.

---

## Actuators and Motor Control

* **L298N Dual H-Bridge Motor Driver:** Controls speed and direction for the motorized window blinds DC gear motor.
* **DC Gear Motor:** High-torque gear motor (such as a 60 RPM cylindrical DC motor) configured for window blind actuation.
* **5V Relay Modules (2-Channel or Individual):** Active-LOW relay modules used to switch high-voltage/AC loads (cooling fan and main tube light).

---

## Lighting and Ambiance

* **Main Tube Light:** AC-powered fluorescent or LED tube switched via relay.
* **Bedside Lamp Circuit:** Dimmable LED lamp or strip driven via dedicated ESP32 PWM pin (GPIO 2) and power transistor/MOSFET stage.
* **Indicator LEDs:** Standard 3mm or 5mm LEDs for status monitoring (Tube Light, Fan, Bed Lamp, Night Mode, Movie Mode, and System Status).
* **Piezo Buzzer:** Active or passive buzzer for audible feedback and alarm alerts.

---

## Display and Interface

* **16x2 I2C LCD Display:** Character LCD module equipped with an I2C backpack adapter (operating at address `0x27`).

---

## Power Supply and Wiring

* **Power Supply Unit (PSU):** Regulated 5V and 12V power distribution rails to supply logic components, relays, and the motor driver.
* **Breadboards, Jumper Wires, and Terminal Blocks:** For prototyping, bus connections, and secure screw terminal terminations for high-current lines.
