# 🌡️ ESP32-S3 Environmental Monitoring System (IoT)

A complete IoT environmental monitoring system built around the **ESP32-S3** microcontroller and a **DHT11** sensor. This project provides local monitoring capabilities, cloud telemetry via **ThingsBoard** using the **MQTT** protocol, local data logging, and hardware alerts.

---

## 🚀 Key Features

* **Real-time Environmental Monitoring:** Continuous reading of temperature and humidity using the DHT11 sensor.
* **ThingsBoard Cloud Integration:** Periodic telemetry data transmission via MQTT protocol displayed on an interactive Dashboard.
* **Local Hardware Alert:** Automated buzzer activation when temperature exceeds a specified threshold.
* **Local Web Server:** Access sensor data locally via Access Point (AP) mode.
* **Data Logging:** Save historical readings to a `CSV` file inside the board's SPIFFS internal flash memory.

---

## 🛠️ Hardware Components

| Component | Pin / GPIO | Description |
| :--- | :--- | :--- |
| **ESP32-S3** | — | Main microcontroller with Wi-Fi capability |
| **DHT11 Sensor** | GPIO 20 | Temperature and humidity sensor |
| **Buzzer** | GPIO 40 | Alert buzzer for high-temperature warnings |
| **I2C LCD (16x2)** | SDA: GPIO 4 / SCL: GPIO 5 | Local display screen (Integration ongoing) |

---

## 💻 Software & Libraries

Developed using **Arduino IDE** and **VS Code**. Make sure to install the following libraries:

* `WiFi.h` (Built-in with ESP32 board package)
* `DHT sensor library` (by Adafruit)
* `PubSubClient` (by Nick O'Leary - for MQTT communication)
* `LiquidCrystal_I2C` (for I2C LCD control)

---

## 📁 Repository Structure

* `code`: Phase 1 code (Hardware testing & local web server).
* `code_two`: Phase 2 code (CSV data logging on internal storage).
* `code_three` / `sketch_oct25a.ino`: Final code integrated with ThingsBoard Cloud via MQTT.
* `download-csv.csv`: Sample of locally logged data.

---

## 🔧 Setup & Installation

1. **Hardware Setup:** Connect the sensor, buzzer, and display to the designated GPIO pins listed in the table above.
2. **ThingsBoard Setup:**
   * Create a new device on [ThingsBoard Cloud](https://thingsboard.cloud).
   * Copy your **Device Access Token**.
3. **Code Configuration:**
   * Open the project code and update your Wi-Fi credentials and Device Token:
     ```cpp
     const char* ssid = "YOUR_WIFI_SSID";
     const char* password = "YOUR_WIFI_PASSWORD";
     const char* token = "YOUR_DEVICE_ACCESS_TOKEN";
     ```
4. **Upload:** Flash the code to your ESP32-S3 board and open the Serial Monitor at a baud rate of `115200`.

---

## 👤 Author

* **bicho-dr** - [GitHub Profile](https://github.com/bicho-dr)