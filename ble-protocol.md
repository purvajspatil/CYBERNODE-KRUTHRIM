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
