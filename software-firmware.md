# Software and Firmware Documentation

This document describes the firmware architecture, background execution loops, state machines, and communication interfaces for the ESP32 Smart Room Automation project.

---

## Core Architecture and Libraries

The firmware is built on the Arduino core for the ESP32 and relies on the following external and built-in libraries:

* **Wire:** Manages I2C communication with the 16x2 LCD display.
* **LiquidCrystal_I2C:** Handles text rendering, cursor control, and backlight states for the status display.
* **OneWire and DallasTemperature:** Manages communication with the DS18B20 digital temperature sensor.
* **IRremote:** Decodes incoming NEC-protocol infrared signals from the remote control.
* **BLEDevice (ESP32 BLE Arduino Core):** Handles Bluetooth Low Energy server initialization, characteristic reading/writing, and advertising.

---

## Background Execution Loops

To prevent thread lockups and maintain responsiveness, the main loop relies on non-blocking millisecond tracking rather than delay functions:

* **Clock Engine:** Updates internal timekeeping variables every 1000 milliseconds (`millis() - lastSecondMillis >= 1000`), incrementing seconds, minutes, and hours while checking for daily alarm triggers.
* **Airport Scrolling Ticker:** Refreshes and shifts the 90-character operational status string across the second row of the LCD every 300 milliseconds.
* **Temperature Polling:** Uses asynchronous temperature conversion requests (`sensors.setWaitForConversion(false)`) to poll the DS18B20 sensor every 2 seconds without blocking execution.

---

## Motorized Blinds Control Engine

The blinds state machine translates abstract positional steps (1 through 5) into timed motor movements:

* **Travel Calibration:** Moving down takes approximately 6.91 seconds per step (`blindStepTimeDown`), while moving up takes approximately 11.8 seconds per step (`blindStepTimeUp`) to account for gravitational resistance.
* **Speed Regulation:** The L298N motor driver ENA pin receives a 5 kHz hardware PWM signal to regulate motor speed during position changes.

---

## Bluetooth Low Energy (BLE) Interface

The firmware includes a built-in BLE server that broadcasts under the local name `CYBERNODE-KRUTHRIM.SIMPLI.CO` using the standard Nordic UART service configuration:

* **Service UUID:** `6E400001-B5A3-F393-E0A9-E50E24DCCA9E`
* **RX Characteristic:** `6E400002-B5A3-F393-E0A9-E50E24DCCA9E` (Receives commands from mobile applications)
* **TX Characteristic:** `6E400003-B5A3-F393-E0A9-E50E24DCCA9E` (Transmits notifications back to connected clients)
* **Supported Command Formats:** 
  * Clock synchronization (`T1231A` or `T1231P`)
  * Custom alarm scheduling (`S0450A` or `S0450P`)
  * Direct alarm toggling (`A`)
