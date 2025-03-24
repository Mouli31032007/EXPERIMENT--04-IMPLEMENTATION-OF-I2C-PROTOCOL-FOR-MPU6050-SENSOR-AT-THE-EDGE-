 # EXPERIMENT-03-INTERFACING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME: S.MOULIDHARAN 
### **DEPARTMENT: AIML 
### **ROLL NO: 212224240095  
### **DATE OF EXPERIMENT: 24/03/2025

---

## **AIM:**  
To interface an **MPU6050 6-Axis Accelerometer & Gyroscope Sensor** with the **Raspberry Pi Pico** and display the sensor readings using MicroPython.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi Pico  
2. MPU6050 6-Axis Accelerometer & Gyroscope Sensor  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
### **About Raspberry Pi Pico:**  
Raspberry Pi Pico is a microcontroller board based on the **RP2040 chip**. It features:  
- Dual-core ARM Cortex-M0+ processor  
- 26 multi-function GPIO pins  
- Supports **MicroPython** and **C/C++**  
- Interfaces like **I2C, SPI, UART, and PWM**  
- Low power consumption, ideal for **IoT applications**  

### **About MPU6050 Sensor:**  
The **MPU6050** is a **6-Axis Inertial Measurement Unit (IMU)** that includes:  
- **3-axis accelerometer** and **3-axis gyroscope**  
- **I2C communication protocol** for easy interfacing  
- **Operating Voltage:** 3.3V – 5V  
- **Accelerometer Range:** ±2g, ±4g, ±8g, ±16g  
- **Gyroscope Range:** ±250°/s, ±500°/s, ±1000°/s, ±2000°/s  

The **accelerometer** measures linear acceleration in **X, Y, Z axes**, while the **gyroscope** measures rotational velocity. The sensor communicates with the Raspberry Pi Pico via **I2C protocol**.

---

## **WORKING PRINCIPLE:**  
1. The **MPU6050 sensor** is connected to the **Raspberry Pi Pico** using the **I2C communication protocol**.  
2. The **Pico reads acceleration and gyroscope values** from the sensor registers.  
3. The data is **processed and displayed on the serial monitor**.  
4. The readings can be used for **motion tracking, tilt sensing, or gesture recognition**.

---

## **CIRCUIT DIAGRAM:** 
![WhatsApp Image 2025-03-24 at 10 53 26_5c66a6fb](https://github.com/user-attachments/assets/adb976a2-cda2-4806-9e5b-f957fa09e23d)
### **Connections:**  

| MPU6050 Pin | Raspberry Pi Pico Pin |
|------------|----------------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GP20 |
| SCL | GP21 |

---

## **PROGRAM (MicroPython)**  
```
from machine import Pin, I2C
import utime
MPU6050_ADDR = 0x68
PWR_MGMT_1 = 0x6B
ACCEL_XOUT_H = 0x3B
GYRO_XOUT_H = 0x43

i2c = I2C(0, scl=Pin(1), sda=Pin(0), freq=400000)  
def mpu6050_init():
    i2c.writeto_mem(MPU6050_ADDR, PWR_MGMT_1, b'\x00')  

def read_raw_data(reg):
   
    data = i2c.readfrom_mem(MPU6050_ADDR, reg, 2) 
    value = (data[0] << 8) | data[1]  
    if value > 32767:  
        value -= 65536
    return value
mpu6050_init()
while True:
    ax = read_raw_data(ACCEL_XOUT_H) / 16384.0
    ay = read_raw_data(ACCEL_XOUT_H + 2) / 16384.0
    az = read_raw_data(ACCEL_XOUT_H + 4) / 16384.0
  
    gx = read_raw_data(GYRO_XOUT_H) / 131.0
    gy = read_raw_data(GYRO_XOUT_H + 2) / 131.0
    gz = read_raw_data(GYRO_XOUT_H + 4) / 131.0

    print(f"Accel: X={ax}, Y={ay}, Z={az} | Gyro: X={gx}, Y={gy}, Z={gz}")

    utime.sleep(10)
```

## **OUTPUT:**  
When the above program is executed, the output on the serial monitor will display real-time acceleration and gyroscope values, such as:

Accel: X=0.02g, Y=-0.01g, Z=1.00g | Gyro: X=0.05°/s, Y=-0.02°/s, Z=0.01°/s
Accel: X=0.03g, Y=-0.02g, Z=1.01g | Gyro: X=0.06°/s, Y=-0.03°/s, Z=0.02°/s

![WhatsApp Image 2025-03-24 at 10 53 26_12609b64](https://github.com/user-attachments/assets/63675358-ebc6-4418-bee2-52821be2c571)
![WhatsApp Image 2025-03-24 at 10 53 36_d9ab9372](https://github.com/user-attachments/assets/e662a763-e6d8-4e0f-8c87-0a2d273b1b8b)
![WhatsApp Image 2025-03-24 at 10 53 43_c535e855](https://github.com/user-attachments/assets/e2c9b70f-f139-4876-8b52-8ef1bbbe30df)
![WhatsApp Image 2025-03-24 at 10 53 53_c189ebbe](https://github.com/user-attachments/assets/c40df214-022f-45fe-8140-d79e630db9d6)


## **RESULT:**  
The **MPU6050 sensor** was successfully interfaced with the **Raspberry Pi Pico**, and real-time **acceleration and gyroscope data** were read and displayed. The sensor values can be used for **motion tracking, tilt detection, and gesture control applications**.


