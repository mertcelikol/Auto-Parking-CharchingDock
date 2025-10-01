# Autonomous Mobile Robot with precise auto parking to charging dock

## Overview
This project implements an **industrial-grade autonomous mobile robot** capable of:  
- Following a line or floor markers for navigation.  
- Detecting obstacles in real-time using robust sensors.  
- Monitoring battery level and autonomously docking for charging.  
- Operating reliably with real-time task management via **FreeRTOS**.

Designed for industrial environments, the system integrates **custom I²C drivers**, multiple sensor interfaces, and a modular architecture for reliability and scalability.

---

## Features
- **Autonomous Navigation:** PID-based line-following and obstacle avoidance.  
- **Battery Monitoring & Auto-Docking:** Automatically navigates to the charging station when battery is low.  
- **Sensor Fusion:** Combines line sensors, ToF/LiDAR, and IMU data for robust navigation.  
- **Motor Control:** High-precision speed and position control via PID loops.  
- **Optional Logging & Display:** Logs data and displays status on an industrial OLED or dashboard.

---

## Hardware Components
- **Controller:** STM32H7 or STM32F767ZI (FreeRTOS-capable MCU)  
- **Motors:** 24 V brushed DC or BLDC motors with encoders  
- **Motor Drivers:** Industrial-grade ESCs (e.g., Sabertooth 2x25 or ODrive)  
- **Sensors:**  
  - Line-following: QTRX I²C array or industrial photoelectric sensors  
  - Obstacle detection: VL53L1X ToF sensors or RPLIDAR  
  - Orientation: Industrial IMU (MPU-9250 or equivalent)  
  - Battery monitoring: INA219 or industrial BMS  
- **Power:** 24 V Li-ion/LiFePO4 battery, DC-DC converters for logic rails  
- **Optional:** OLED display (I²C) for real-time status  

---

## Software Architecture

The system runs on **FreeRTOS**, with the following tasks:

| Task | Responsibility |
|------|----------------|
| **Line Sensor Task** | Reads floor sensors over I²C, posts data to navigation queue |
| **Obstacle Sensor Task** | Reads ToF/LiDAR sensors and updates environment map |
| **Navigation Task** | PID-based path control and obstacle avoidance |
| **Motor Control Task** | Converts navigation commands into motor PWM/velocity outputs |
| **Battery Monitor Task** | Checks voltage/current, triggers docking mode when low |
| **Docking Task** | Guides robot to charging station, fine-tunes alignment using ToF |
| **Display / Logger Task (Optional)** | Shows system status or logs telemetry data |

**Inter-task Communication:**  
- Queues for sensor data passing  
- Semaphores/mutexes for safe access to I²C and shared resources  

---

## Custom I²C Driver
- Non-blocking, interrupt/DMA-driven  
- Supports multiple devices on the same bus  
- Integrates with FreeRTOS tasks using semaphores or notifications  

 
