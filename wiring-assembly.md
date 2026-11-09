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
