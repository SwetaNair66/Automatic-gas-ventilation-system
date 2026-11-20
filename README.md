# Gas Leak Detection & Auto-Ventilation System

An embedded system built on the **STM32 Nucleo-F401RE** that detects gas leaks using an MQ-6 sensor, raises local alarms, switches on an exhaust fan automatically, and uploads live readings to **ThingSpeak** through an ESP32.

## Features
- **Real-time sensing:** MQ-6 gas sensor read through the 12-bit ADC
- **Local alerts:** buzzer plus live status on a 16x2 I2C LCD
- **Auto-ventilation:** relay-driven exhaust fan turns on when a leak is detected
- **Remote monitoring:** readings sent over USART to an ESP32 and uploaded to ThingSpeak
- **Manual recalibration:** push button (EXTI interrupt) to reset the baseline

## How It Works
1. **Calibration:** at power-up, the system averages 50 clean-air samples to set a baseline.
2. **Sampling:** a TIM2 timer interrupt triggers an ADC reading every 300 ms.
3. **Detection:** a leak is flagged when the reading rises 100 counts above the baseline.
4. **Hysteresis:** the alarm clears only when the reading drops by 50 counts, which prevents flickering near the threshold.
5. **Drift handling:** if readings plateau at an elevated level for 20 s, the baseline is recalibrated automatically to avoid false alarms from sensor drift.
6. **Response:** on a leak, the buzzer sounds, the fan switches on, and the LCD shows the alert.
7. **Cloud upload:** the STM32 sends readings to the ESP32 over USART, which pushes them to ThingSpeak.

## Hardware
- STM32 Nucleo-F401RE
- MQ-6 gas sensor
- 16x2 LCD with I2C module
- Buzzer
- Single-channel relay and exhaust fan
- ESP32 (Wi-Fi gateway)
- Push button

## Tech Stack
C, STM32CubeIDE, ADC, TIM2 timer interrupt, EXTI, I2C, USART, ThingSpeak

## Repository Structure
```
/Core        # STM32 firmware source
/ESP32       # ESP32 ThingSpeak upload code
/docs        # Project report, circuit diagram
```

## Getting Started
1. Open the project in STM32CubeIDE and build.
2. Flash the firmware to the Nucleo-F401RE.
3. Add your Wi-Fi credentials and ThingSpeak API key in the ESP32 code.
4. Power up in clean air and let the calibration finish.

## Author
Sweta Nair C P
