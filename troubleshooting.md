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
