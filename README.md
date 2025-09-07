<!--# FPGA CAR

使用FPGA與verilog做出一台可以由手機藍芽遙控，運用藍芽模組接收遙控訊號的車子，另外的模式是可以讓車子自動行徑而不靠遙控，車子運用紅外線避障模組來感測前方障礙物並且及時轉彎再前行


![](https://github.com/zzzzz314314/fpga-bluetooth-car/raw/master/fpga_car.png)<br>

其中的兩種模式詳述如下：<br>
- Manual: 於手機控制上、下、左、右
- Auto: 車子自動前行，前方有障礙物時，後退、左轉、再前行。手機使用android studio寫app，介面如下

![](https://github.com/zzzzz314314/fpga-bluetooth-car/blob/master/fpga_car1.png)<br><br><br>
# Block Diagram
![](https://github.com/zzzzz314314/fpga-bluetooth-car/blob/master/fpga_car2.png)<br><br><br>
# 成品
![](https://github.com/zzzzz314314/fpga-bluetooth-car/blob/master/fpga_car3.png)<br>
更多細節請於report中瀏覽，另關於app控制端，請於fpga-bluetooth-car-control project中瀏覽-->


# FPGA Car 🚗 (Bluetooth-Controlled & Obstacle-Avoiding)

This project implements a small car using **FPGA (Verilog)** that supports two modes of operation:  
1. **Manual Mode** – controlled by a custom Android app via Bluetooth.  
2. **Auto Mode** – autonomously avoids obstacles using infrared (IR) sensors.

Developed as the final project of a hardware laboratory course.  

---

## 🎯 Goals
- Design and implement a **Verilog-based FSM** for car control.  
- Integrate **Bluetooth communication** between smartphone app and FPGA.  
- Implement **infrared-based obstacle detection** for autonomous navigation.  
- Control stepper motors via FPGA modules to achieve smooth movement.

---

## 📱 System Overview
- **Manual Control:**  
  - Android app (built with Android Studio) sends commands via Bluetooth (UP, DOWN, LEFT, RIGHT, STOP, AUTO).  
  - FPGA UART module receives and decodes signals, driving the motors accordingly.  

- **Auto Mode:**  
  - Car moves forward until an obstacle is detected.  
  - Executes a finite-state sequence: **reverse → turn left → forward again**.  
  - Uses IR sensors to detect objects within ~3cm.

---

## 🔧 Architecture (Modules)
- **`Bi_chang`**: Processes IR sensor input (inverts logic for FPGA).  
- **`Sequential & Combinational`**: Collects IR and Bluetooth inputs, outputs motor control signals.  
- **`UART_BaudRate_Generate`**: Clock divider for UART ticks.  
- **`UART_rs232_rx`**: UART receiver for decoding Bluetooth signals.  
- **`pmod_step_interface` & `pmod_step_driver`**: Stepper motor control (Moore FSM).  
- **Android App**: Sends specific codes (mapped via trial & error) to trigger FPGA actions.  

---

## 📷 Block Diagram
![Block Diagram](https://github.com/zzzzz314314/fpga-bluetooth-car/blob/master/fpga_car2.png)<br><br><br>

## 📸 Final Product
Here is the completed prototype:

![Final Product](https://github.com/zzzzz314314/fpga-bluetooth-car/blob/master/fpga_car3.png)

---

## 🚀 How It Works
- **Bluetooth Communication**  
  - App sends predefined strings (e.g., `"199"` → Forward).  
  - FPGA maps received values (e.g., `count_Rx = 9`) to motor control signals.  

- **Motor Control**  
  - `motor_en`: Enable signal.  
  - `motor_dir`, `motor_dir2`: Direction signals (adjusted for wheel orientation).  
  - Stepper motor driver uses a 4-state Moore machine for rotation control.  

---

## ⚡ Challenges & Lessons Learned
- **Hardware Wiring Issues** – used crocodile clips to improve connection stability.  
- **Voltage Compatibility** – FPGA outputs 3.3V, while some modules required 5V; solved using motor driver to boost voltage.  
- **UART Timing** – baud rate mismatch required alternative counter-based decoding strategy.  
- **Hands-on Growth** – learned debugging across hardware, software, and communication protocols.

---

## 📚 Future Work
- Improve UART decoding reliability with precise clock alignment.  
- Replace battery pack with a more compact power management system.  
- Enhance autonomous navigation with multi-sensor fusion (e.g., ultrasonic + IR).  
