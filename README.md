# Gesture-controlled-robot-arm
A small project which is doing to improve the knowledge of machine learning and robotics
# 🦾 TinyML Gesture-Controlled Robot Arm

![Status: Work in Progress](https://img.shields.io/badge/Status-Work_in_Progress-yellow?style=for-the-badge)
![Hardware: STM32 & ESP32](https://img.shields.io/badge/Hardware-STM32_%7C_ESP32--C3-blue?style=for-the-badge)
![Software: TinyML](https://img.shields.io/badge/AI-Edge_Impulse_%7C_TinyML-orange?style=for-the-badge)

Welcome to the repo! This project is all about bridging the gap between embedded hardware and machine learning. 

The goal? Build a 3-DOF robot arm that perfectly mimics hand movements in real-time. Instead of relying on a heavy PC or a camera for computer vision, this project runs a Neural Network entirely on the edge (TinyML) using a custom wireless sensor glove. 

## 🧠 The Concept

Traditional robot arms use hard-coded kinematics or bulky external processing. This project takes a different approach:
1. **Read** raw 6-axis IMU data from a wearable glove.
2. **Process** the telemetry using a trained classification model directly on an ESP32-C3.
3. **Transmit** the recognized gesture via ultra-low-latency 2.4GHz radio.
4. **Execute** the physical movement using an STM32 "Black Pill" handling the heavy lifting and servo PWM generation.

No Wi-Fi lag, no giant computers, just pure embedded C++ and math.

---

## 🛠️ The Hardware Stack

To keep the latency near zero (around 1-2ms) and the compute power high, the system is split into two isolated units:

### 1. The Wireless "Smart Glove" (Transmitter)
Designed to be tiny, wearable, and entirely battery-powered.
* **Brain:** ESP32-C3 SuperMini (160MHz RISC-V) - Fast enough for ML inference.
* **Sensor:** MPU9250 (6-Axis Accelerometer & Gyroscope).
* **Radio:** NRF24L01+ (For instant, lag-free communication).
* **Power:** 3.7V LiPo Battery.

### 2. The Robot Arm (Receiver)
Built to handle high-current loads and precise servo timing.
* **Brain:** STM32F411CEU6 "Black Pill" (100MHz with an FPU for fast math).
* **Actuators:** 3x MG996R Metal Gear Servos.
* **Radio:** NRF24L01+.
* **Power Delivery:** XL4015 Buck Converter (Stepping down a 12V supply to a clean 6V for the servos, keeping the STM32 safely isolated).

---

## 💻 The Software Stack

* **Arduino IDE:** Used for all firmware development and hardware interfacing.
* **Edge Impulse:** For data collection, digital signal processing (DSP), and training the neural network.
* **TensorFlow Lite for Microcontrollers:** To quantize the trained model and run it locally on the ESP32-C3.

---

## 🚀 Roadmap & Progress

Here is where the project currently stands:

- [ ] **Phase 1: Hardware Prototyping**
  - [x] Select hardware architecture.
  - [ ] Wire the ESP32-C3 to the MPU6050.
  - [ ] Test NRF24L01 radio link latency between the ESP32 and STM32.
- [ ] **Phase 2: Data Collection & ML**
  - [ ] Stream clean IMU telemetry via serial over USB.
  - [ ] Build a robust dataset of gestures (Idle, Grab, Wave, etc.) using Edge Impulse.
  - [ ] Train and optimize the classification model.
- [ ] **Phase 3: The Arm Build**
  - [ ] Assemble the 3-DOF mechanical structure.
  - [ ] Solder the power distribution board (Buck converter + Common Ground).
  - [ ] Map model outputs to servo angles.
- [ ] **Phase 4: Final Polish**
  - [ ] 3D print a slick enclosure for the wireless glove.
  - [ ] Real-world testing and tuning.

---

## 🔌 Quick Wiring Reference (Glove)

If you are recreating this, be careful with the SuperMini pins! Here is the baseline I2C map:

| Component | ESP32-C3 SuperMini Pin | Note |
| :--- | :--- | :--- |
| MPU9250 SDA | GPIO 8 | Data Line |
| MPU9250 SCL | GPIO 9 | Clock Line |
| VCC | 3.3V | Do not use 5V for the IMU! |
| GND | GND | Common Ground |

---

> *"Hardware is just software crystallized."* Feel free to fork, open an issue, or just follow along with the build process!
