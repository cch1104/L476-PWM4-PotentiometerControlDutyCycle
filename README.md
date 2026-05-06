# L476-PWM4-PotentiometerControlDutyCycle
# 🌡️ STM32 Temperature-Based PWM Control System

## 📌 Overview

This project implements a **temperature-controlled PWM system** using an STM32 microcontroller.
The system reads analog data from a temperature sensor via ADC, converts it to Celsius, and adjusts PWM duty cycle accordingly. The current temperature and duty cycle are displayed on an LCD.

---

## ⚙️ Features

* 📥 ADC-based temperature sensing (12-bit resolution)
* 🌡️ Temperature conversion (mV → °C)
* 🔄 Automatic PWM duty cycle adjustment
* 📺 Real-time LCD display
* ⏱️ Periodic updates every 2 seconds

---

## 🧠 System Logic

### Temperature Calculation

```
mv = adc * 3300 / 4095
temperature = (mv - 500) / 10
```

### PWM Control Rule

| Temperature (°C) | Duty Cycle |
| ---------------- | ---------- |
| < 25°C           | 10%        |
| 25°C – 44°C      | 50%        |
| ≥ 45°C           | 100%       |

---

## 🧩 Hardware Requirements

* STM32 Microcontroller (e.g., STM32L4 series)
* Temperature Sensor (e.g., LM35 or equivalent)
* LCD (16x2, parallel interface)
* LED or Fan (PWM output device)
* Power Supply (3.3V)

---

## 🔌 Pin Configuration

| Function      | Pin      |
| ------------- | -------- |
| ADC Input     | ADC1_IN3 |
| PWM Output    | TIM2_CH1 |
| LCD Data Pins | PA6–PA11 |

---

## 🛠️ Peripherals Configuration

### ADC1

* Resolution: 12-bit
* Mode: Continuous Conversion
* Channel: ADC_CHANNEL_3

### TIM2 (PWM)

* Prescaler: 79
* Period (ARR): 999
* PWM Mode: PWM1
* Channel: TIM_CHANNEL_1

---

## 🚀 How It Works

1. Start ADC and PWM
2. Read analog voltage from sensor
3. Convert ADC value → millivolts → temperature
4. Determine duty cycle based on temperature range
5. Update PWM output
6. Display temperature and duty cycle on LCD
7. Repeat every 2 seconds

---

## 📂 Key Function

### Duty Cycle Control

```c
void DutyCycle(float Duty, int Channel){
    uint16_t AutoReload, SetDutyCycle;
    AutoReload = __HAL_TIM_GET_AUTORELOAD(&htim2);
    SetDutyCycle = AutoReload * Duty / 100.0;
    __HAL_TIM_SET_COMPARE(&htim2, Channel, SetDutyCycle);
}
```

---

## 📸 Example Output (LCD)

```
TMP: 32.50 C
duty:50.00
```

---

## ⚠️ Notes

* Ensure correct sensor calibration (TMP36 assumes 500mV offset)
* PWM frequency depends on clock + prescaler + ARR
* LCD must match the wiring defined in `lcd.c`

---

## 🧪 Possible Improvements

* Replace polling with ADC interrupt or DMA
* Smooth temperature readings (moving average filter)
* Add fan speed feedback (closed-loop control)
* Improve UI (custom LCD symbols)

demo Video:
https://youtu.be/9R7zbPLpAHA
