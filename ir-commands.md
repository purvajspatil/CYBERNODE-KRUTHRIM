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
