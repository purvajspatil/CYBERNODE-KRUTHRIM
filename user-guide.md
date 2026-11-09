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
