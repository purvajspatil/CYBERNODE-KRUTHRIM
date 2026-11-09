# CyberNode: ESP32 Smart Room Automation System

An integrated smart room automation system that centralizes environmental control, motorized window blinds, dimmable bed lighting, relay-switched appliances, and a real-time alarm clock.

---

## System Overview

* **Smart Climate Control:** Continuous temperature monitoring using a DS18B20 1-Wire sensor, with automated cooling fan relay switching based on target thresholds and hysteresis.
* **Motorized Window Blinds:** Positional stepper control driven by an L298N H-bridge motor driver, featuring PWM speed regulation and timed travel calibration profiles.
* **Lighting and Ambiance:** Five hardware-backed PWM brightness levels for bedside lighting, main room relay switches, and multi-stage cinematic movie mode profiles.
* **Real-Time Clock and Alarm:** Drift-free internal 12-hour/24-hour timekeeping engine with programmable daily wake-up routines for automated lighting and blinds.
* **Multiple Control Interfaces:** Responds to NEC-protocol IR remote inputs, a physical tactile night switch, and a Bluetooth Low Energy (BLE) smartphone companion service (CYBERNODE-KRUTHRIM.SIMPLI.CO).
* **Live Display Ticker:** Operational telemetry and status information displayed on an I2C 16x2 LCD screen via a scrolling status ticker.

---

## Hardware Pinout and Mapping

* **GPIO 2:** Bed Lamp PWM (LEDC PWM, 5 kHz, 8-bit)
* **GPIO 4:** DS18B20 Temperature Sensor (1-Wire Protocol)
* **GPIO 5:** L298N Motor ENA (PWM Speed Enable)
* **GPIO 12:** Tube Light Indicator LED
* **GPIO 13:** Fan Indicator LED
* **GPIO 14:** Bed Lamp Indicator LED
* **GPIO 15:** IR Receiver (NEC Protocol)
* **GPIO 16 / 17:** L298N Motor Direction Lines (IN1/IN2)
* **GPIO 18:** Fan Relay (Active-LOW)
* **GPIO 23:** System Status LED
* **GPIO 25:** Night Mode LED
* **GPIO 26:** Piezo Buzzer (Tone Generator)
* **GPIO 27:** Movie Mode LED
* **GPIO 32:** Physical Night Switch (Input Pullup)
* **GPIO 33:** Tube Light Relay (Active-LOW)
* **I2C (SDA/SCL):** 16x2 LCD Display (Address 0x27)

---

## Firmware Dependencies

To compile and flash this firmware to your ESP32, ensure the following libraries are installed in your Arduino environment:

* Wire
* LiquidCrystal_I2C
* OneWire
* DallasTemperature
* IRremote
* BLEDevice (ESP32 BLE Arduino Core)

---

## License

Copyright (c) 2026 Purvaj S Patil. 

This software and documentation are provided for use without modification. The software may not be altered, adapted, or used to create derivative works, nor redistributed in a modified form. Refer to the LICENSE file for full terms.
