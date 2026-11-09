# Comprehensive Project Documentation Package

This document contains the complete, consolidated Markdown source code for all project files, combined into a single continuous code block so you can easily copy and paste it or split it into your repository structure.

---

## 1. README.md

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

---

## 2. docs/bill-of-materials.md

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

---

## 3. docs/wiring-assembly.md

# Hardware Wiring and Assembly Guide

This document outlines the pin connections, power requirements, and assembly steps for the ESP32 Smart Room Automation project.

---

## Safety and Power Requirements

* Ensure your ESP32 and peripheral modules share a common ground.
* Inductive loads (such as the DC gear motor and relay coils) draw higher current during startup; ensure your power supply can handle peak loads without sagging below operational voltage thresholds.
* Use a 4.7k ohm pull-up resistor on the DS18B20 1-wire data line to ensure stable communication.

---

## Pin Connection Summary

### Microcontroller and Sensors
* **DS18B20 Temperature Sensor:** Connected to GPIO 4 (Data line with 4.7k pull-up resistor to 3.3V).
* **IR Remote Receiver:** Connected to GPIO 15.
* **Physical Night Switch:** Connected to GPIO 32 (configured with internal pull-up).
* **Piezo Buzzer:** Connected to GPIO 26.

### Relays and Lighting Indicators
* **Fan Relay:** Connected to GPIO 18 (Active-LOW). Fan Indicator LED on GPIO 13.
* **Tube Light Relay:** Connected to GPIO 33 (Active-LOW). Tube Light Indicator LED on GPIO 12.
* **Bed Lamp PWM:** Connected to GPIO 2 (LEDC hardware PWM, 5 kHz, 8-bit). Bed Lamp Indicator LED on GPIO 14.
* **System Status LED:** Connected to GPIO 23.
* **Night Mode LED:** Connected to GPIO 25.
* **Movie Mode LED:** Connected to GPIO 27.

### Motor Driver (L298N) and Blinds
* **Motor Speed Enable (ENA):** Connected to GPIO 5 (LEDC PWM speed regulation).
* **Motor Direction 1 (IN1):** Connected to GPIO 16.
* **Motor Direction 2 (IN2):** Connected to GPIO 17.

### Display Bus
* **I2C LCD Display (16x2):** Connected to standard ESP32 SDA and SCL pins at I2C address 0x27.

---

## 4. docs/software-firmware.md

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

---

## 5. docs/user-guide.md

# User Operation and Control Guide

This document outlines how to interact with the ESP32 Smart Room Automation system using the IR remote, the Bluetooth mobile app, and physical switches.

---

## Control Interfaces

### 1. IR Remote Control
The system decodes NEC-protocol infrared commands to let you manage room functions from a distance:

* **Button 1:** Activates the system (Turns status LED ON, turns tube light ON, and resets blinds to Step 1).
* **Button 3:** Master Shutdown (Deactivates system, turns off all relays, sets blinds to Step 5, and turns off all status LEDs).
* **Button 0:** Toggles the main tube light ON or OFF.
* **Button 2:** Cycles through the 4-stage cinematic "Movie Mode" profiles.
* **Button 7 / Button 9:** Decreases or increases the bedside lamp PWM brightness level (levels 1 through 5).
* **Button 8:** Toggles the bedside lamp ON or OFF at the current brightness level.
* **Star Key (*):** Toggles manual override for the cooling fan.
* **Hash Key (#):** Toggles Night Mode ON or OFF.
* **Up / Down Arrows:** Adjusts window blinds position step-by-step (Steps 1 through 5).
* **Left / Right Arrows:** Decreases or increases the target climate temperature by 0.5°C increments.

### 2. Bluetooth Low Energy (BLE) Smartphone App
Using any standard Nordic UART compatible BLE scanner or companion app, connect to the device broadcasting as `CYBERNODE-KRUTHRIM.SIMPLI.CO`. 

* **Clock Synchronization:** Send strings formatted as `T1231A` (12:31 AM) or `T1231P` (12:31 PM) to update the real-time clock.
* **Alarm Configuration:** Send strings formatted as `S0600A` (6:00 AM) or `S0730P` (7:30 PM) to set the daily wake-up routine.
* **Alarm Toggle:** Send a single character `A` to enable or disable the daily alarm.

### 3. Physical Controls
* **Night Mode Switch:** A tactile push-hold switch connected to GPIO 32 allows you to quickly engage night mode locally, dimming lights and adjusting window blinds instantly.

---

## 6. docs/troubleshooting.md

# Troubleshooting and Diagnostics

If you encounter issues during assembly, flashing, or operation, review the common troubleshooting steps below.

---

## 1. Hardware and Sensor Issues

* **DS18B20 Temperature Sensor reads `-127.0°C` or `85.0°C`:**
  * **Cause:** A reading of `-127.0°C` indicates a communication disconnection, while `85.0°C` is the power-on reset default state (indicating the sensor is receiving power but failing to complete conversion).
  * **Fix:** Verify that the 4.7k ohm pull-up resistor is correctly wired between the data line (GPIO 4) and the 3.3V power rail. Check all solder joints and wiring continuity.

* **Motorized Blinds move erratically or fail to stop:**
  * **Cause:** Incorrect timing calibration or PWM signal loss on the L298N driver.
  * **Fix:** Ensure the `blindStepTimeDown` and `blindStepTimeUp` millisecond intervals match your specific motor gearbox and mechanical load. Verify that the `ENA` pin is connected to GPIO 5 for proper PWM speed control.

* **LCD Display is blank or shows black boxes:**
  * **Cause:** Incorrect I2C address, contrast potentiometer misadjustment, or inverted SDA/SCL lines.
  * **Fix:** Confirm your I2C module address using an I2C scanner sketch (commonly `0x27` or `0x3F`). Verify that SDA and SCL are connected to the correct default pins on your ESP32 board.

---

## 2. Firmware and Compilation Errors

* **Compilation error regarding missing libraries:**
  * **Cause:** Required external libraries are not installed in your Arduino IDE environment.
  * **Fix:** Open the Arduino Library Manager (`Ctrl+Shift+I` or `Cmd+Shift+I`) and install `LiquidCrystal_I2C`, `DallasTemperature`, `OneWire`, and `IRremote`. Ensure the ESP32 board package is updated to support the built-in `BLEDevice` core libraries.

---

## 3. Bluetooth (BLE) Connection Issues

* **Cannot find `CYBERNODE-KRUTHRIM.SIMPLI.CO` on your phone:**
  * **Cause:** The ESP32 BLE advertising routine timed out or the microcontroller needs a reset.
  * **Fix:** Press the physical EN/RST button on the ESP32 to restart the service. The firmware includes an automatic 10-second advertising refresh timer to handle dropped connections. Ensure your phone's Bluetooth is toggled on and using a Nordic UART compatible app.

---

## 7. docs/license-usage.md

# License and Terms of Use

This document details the usage terms, rights, and restrictions governing the ESP32 Smart Room Automation System repository, including all source code, schematic configurations, and written documentation.

---

## License Overview

This project is released under a **Custom Source-Available (Use-Only) License**. 

The code and documentation are made publicly available so that individuals can view, compile, flash, and deploy the system for their own personal use. However, modified versions, derivative works, and unauthorized redistributions are strictly prohibited.

---

## Rights and Permissions

Under this license, you are explicitly granted permission to:

* **Download and Clone:** Download, clone, or mirror the repository files for personal study and setup.
* **Compile and Flash:** Build the firmware using the Arduino IDE (or compatible tools) and flash it onto your own ESP32 microcontroller hardware.
* **Run Unmodified:** Operate the complete smart room system as designed, including the BLE companion interface, IR remote controls, and physical switch configurations.

---

## Explicit Restrictions

Under this license, you are **NOT** permitted to:

* **Modify or Adapt:** Change, modify, refactor, or adapt any part of the source code, hardware pinout mappings, or documentation to create customized variants.
* **Create Derivative Works:** Use any portion of this codebase or documentation as the baseline for a separate or derivative software/hardware project.
* **Redistribute Modified Copies:** Publicly host, re-upload, sell, or distribute modified copies or forks of this repository on GitHub or any other platform.
* **Commercial Exploitation:** Sell, sublicense, lease, or package this software or documentation for commercial products or paid installation services.

---

## Summary Table

| Permitted Action | Status | Notes |
| :--- | :--- | :--- |
| Personal deployment and flashing | **Allowed** | Full permission for personal use |
| Viewing and studying source code | **Allowed** | Open access for educational review |
| Modifying code or hardware logic | **Prohibited** | No derivative works allowed |
| Redistributing modified code/forks | **Prohibited** | Original codebase must remain intact |
| Commercial packaging or sale | **Prohibited** | Non-commercial restriction enforced |

---

## Copyright Notice

Copyright (c) 2026 Purvaj S Patil. All rights reserved.

Refer to the root LICENSE file in the main repository for the full legal text.

---

## 8. docs/ir-commands.md

# IR Remote Code Reference

This document maps the verified NEC-protocol hexadecimal codes received by the infrared demodulator to their respective system actions in the firmware.

---

## Remote Code Mapping Table

| Hex Code | Button / Input | System Action |
| :--- | :--- | :--- |
| `0xBA45FF00` | Button 1 | Powers on the system, activates status LED, turns on tube light, and resets blinds to Step 1. |
| `0xB946FF00` | Button 2 | Cycles through the 4-stage cinematic Movie Mode profiles. |
| `0xB847FF00` | Button 3 | Master Shutdown: Deactivates system, shuts down all relays, moves blinds to Step 5, and turns off all status LEDs. |
| `0xF807FF00` | Button 7 | Decreases the bedside lamp PWM brightness level by 1 step (minimum level 1). |
| `0xEA15FF00` | Button 8 | Toggles the bedside lamp ON or OFF at the current brightness level. |
| `0xF609FF00` | Button 9 | Increases the bedside lamp PWM brightness level by 1 step (maximum level 5). |
| `0xE619FF00` | Button 0 | Toggles the main tube light relay ON or OFF. |
| `0xE916FF00` | Star Key (*) | Toggles manual override mode for the cooling fan relay. |
| `0xF20DFF00` | Hash Key (#) | Toggles Night Mode ON or OFF. |
| `0xE718FF00` | Up Arrow | Moves the motorized window blinds up by 1 step. |
| `0xAD52FF00` | Down Arrow | Moves the motorized window blinds down by 1 step. |
| `0xA55AFF00` | Right Arrow | Increases the target climate temperature setpoint by 0.5°C. |
| `0xF708FF00` | Left Arrow | Decreases the target climate temperature setpoint by 0.5°C. |

---

## Implementation Notes

* The IR receiver pin is mapped to **GPIO 15** in the firmware using the `IRremote` library framework.
* Every valid decoded infrared signal triggers an audible confirmation beep via the onboard piezo buzzer connected to GPIO 26.

---

## 9. docs/ble-protocol.md

# Bluetooth Low Energy (BLE) Protocol Specification

This document outlines the BLE service definitions, characteristic UUIDs, and command string protocols used by the ESP32 Smart Room Automation system to communicate with mobile companion apps.

---

## BLE Service and Characteristic Architecture

The firmware initializes an ESP32 BLE Server broadcasting under the local device name **`CYBERNODE-KRUTHRIM.SIMPLI.CO`**, utilizing the standard Nordic UART Service (NUS) layout:

* **Service UUID:** `6E400001-B5A3-F393-E0A9-E50E24DCCA9E`
* **RX Characteristic UUID:** `6E400002-B5A3-F393-E0A9-E50E24DCCA9E` (Used by mobile apps to *write* commands to the ESP32)
* **TX Characteristic UUID:** `6E400003-B5A3-F393-E0A9-E50E24DCCA9E` (Used by the ESP32 to *notify* connected clients)

---

## Incoming Command String Protocols

When writing strings to the RX characteristic, the firmware automatically trims whitespace and converts characters to uppercase. Three primary command formats are supported:

### 1. Real-Time Clock Synchronization
Updates the internal drift-free 24-hour timekeeping engine.
* **Format Structure:** `T[HH][MM][A/P]` (6 characters total)
* **Examples:**
  * `T1231A` -> Sets clock to 12:31 AM (00:31 24h)
  * `T0645P` -> Sets clock to 6:45 PM (18:45 24h)

### 2. Custom Alarm Configuration
Programs the daily wake-up routine time and automatically enables the alarm.
* **Format Structure:** `S[HH][MM][A/P]` (6 characters total)
* **Examples:**
  * `S0600A` -> Sets daily wake-up alarm to 6:00 AM
  * `S0930P` -> Sets daily wake-up alarm to 9:30 PM

### 3. Alarm State Toggle
Toggles the daily wake-up alarm between enabled and disabled states.
* **Format Structure:** Single character `A`
* **Behavior:** Flips `alarmEnabled` status and resets daily trigger tracking.

### 4. Direct Command Byte Injection
Any single-character string matching valid operational commands (such as numbers `0`-`3`, `7`-`9`, or direction characters) will be processed directly as if triggered via the infrared remote interface.

---

## Connection and Advertising Management

* **Advertising Interval:** If disconnected, the firmware executes an automatic background advertising refresh loop every 10 seconds (`advRefreshInterval = 10000ms`) to ensure the device remains discoverable.
* **Acoustic Feedback:** Successful execution of valid time sync or alarm commands triggers a double-beep audio confirmation via the piezo buzzer.

---

## 10. docs/system-architecture.md

# System Architecture and Data Flow

This document provides a high-level overview of the hardware signal connections, state machine transitions, and internal data flow across the ESP32 Smart Room Automation system.

---

## Core Execution and Task Scheduling

The system relies on a single-threaded, non-blocking asynchronous event loop within `loop()` using `millis()` timing guards:

* **Task 1: Time Engine (Every 1000ms)**
  * Increments system runtime seconds.
  * Updates hours, minutes, and AM/PM tracking.
  * Evaluates active alarm triggers against target schedule.

* **Task 2: Climate Watchdog (Every 2000ms)**
  * Requests temperature conversion from DS18B20 without delaying thread execution.
  * Compares current reading to target climate setpoint.
  * Toggles fan relay dynamically if auto-climate mode is active.

* **Task 3: Display Ticker Refresh (Every 300ms)**
  * Shifts operational status string by 1 character index.
  * Renders continuous scrolling message on Row 2 of the 16x2 LCD display.

* **Task 4: BLE Background Refresh (Every 10000ms)**
  * Checks Bluetooth connection state.
  * Restarts local advertising stack if connection drops or times out.

---

## Blinds Motor State Machine

The window blind stepper engine transitions through 5 discrete position steps, managing timing and direction:

* **Step Decrement (Opening):** Drives L298N IN1 high / IN2 low with ENA PWM enabled for 11.8 seconds per step-change.
* **Step Increment (Closing):** Drives L298N IN1 low / IN2 high with ENA PWM enabled for 6.91 seconds per step-change.
* **Safety Lockout:** Motor output pins are set to high-impedance LOW instantly upon step arrival to prevent coil overheating and power drain.

---

## Operational Profiles

| Profile Name | Status LEDs Active | Relays State | Bed Lamp Output | Blinds Position |
| :--- | :--- | :--- | :--- | :--- |
| **System Active** | System LED (GPIO 23) | Tube Light ON | Preserved | Step 1 (Open) |
| **Night Mode** | Night LED (GPIO 25) | All Relays OFF | Level 1 (Dim) | Step 5 (Closed) |
| **Movie Mode** | Movie LED (GPIO 27) | Tube Light OFF / Fan Auto | Off / Level 1 | Step 5 (Closed) |
| **System Off** | All LEDs OFF | All Relays OFF | OFF (0 PWM) | Step 5 (Closed) |
