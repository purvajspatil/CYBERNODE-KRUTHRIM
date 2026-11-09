# System Architecture and Data Flow

This document provides a high-level overview of the hardware signal connections, state machine transitions, and internal data flow across the ESP32 Smart Room Automation system.

---

## 1. Core Execution and Task Scheduling

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

## 2. Blinds Motor State Machine

The window blind stepper engine transitions through 5 discrete position steps, managing timing and direction:

* **Step Decrement (Opening):** Drives L298N IN1 high / IN2 low with ENA PWM enabled for 11.8 seconds per step-change.
* **Step Increment (Closing):** Drives L298N IN1 low / IN2 high with ENA PWM enabled for 6.91 seconds per step-change.
* **Safety Lockout:** Motor output pins are set to high-impedance LOW instantly upon step arrival to prevent coil overheating and power drain.

---

## 3. Operational Profiles

| Profile Name | Status LEDs Active | Relays State | Bed Lamp Output | Blinds Position |
| :--- | :--- | :--- | :--- | :--- |
| **System Active** | System LED (GPIO 23) | Tube Light ON | Preserved | Step 1 (Open) |
| **Night Mode** | Night LED (GPIO 25) | All Relays OFF | Level 1 (Dim) | Step 5 (Closed) |
| **Movie Mode** | Movie LED (GPIO 27) | Tube Light OFF / Fan Auto | Off / Level 1 | Step 5 (Closed) |
| **System Off** | All LEDs OFF | All Relays OFF | OFF (0 PWM) | Step 5 (Closed) |
