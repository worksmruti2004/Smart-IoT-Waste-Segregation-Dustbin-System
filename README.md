# Smart-IoT-Waste-Segregation-Dustbin-System
The Smart IoT Dustbin System automatically segregates waste into wet and dry using sensors and a servo mechanism controlled by an ESP32. Bin levels are monitored and displayed in real time on the Blynk IoT dashboard, helping manage waste efficiently at Kalinga Institute of Industrial Technology and Silicon University.

# 🗑️ Smart IoT Waste Segregation Dustbin System

<div align="center">

![ESP32](https://img.shields.io/badge/ESP32-IoT-blue?style=for-the-badge&logo=espressif)
![Blynk](https://img.shields.io/badge/Blynk-IoT%20Dashboard-green?style=for-the-badge)
![Arduino](https://img.shields.io/badge/Arduino-C++-teal?style=for-the-badge&logo=arduino)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**Automatic waste segregation into wet and dry waste using ESP32, sensors, and real-time Blynk monitoring.**

*Deployed at KIIT University & Silicon University, Bhubaneswar*

</div>

---

## 📌 Overview

The **Smart IoT Waste Segregation Dustbin System** is an embedded IoT project that automatically classifies and separates waste into **wet** and **dry** categories using a moisture/rain sensor and a servo-controlled flap mechanism. Bin fullness is monitored in real time via the **Blynk IoT dashboard**, enabling efficient waste management across multiple locations.

---

## ✨ Features

- 🔍 **Automatic Waste Detection** — IR-based object sensor triggers the system when waste is inserted
- 💧 **Wet/Dry Segregation** — Rain/moisture sensor classifies waste type
- ⚙️ **Servo-Controlled Flap** — Directs waste to the appropriate bin compartment
- 📊 **Real-Time Bin Level Monitoring** — Ultrasonic/IR sensors detect when bins are full
- 📱 **Blynk Dashboard Alerts** — Color-coded indicators (🟢 Empty / 🔴 Full) via virtual pins
- 📡 **Wi-Fi Enabled** — ESP32 connects to cloud for remote monitoring
- 🏫 **Multi-Location Support** — Independent monitoring for KIIT and Silicon University

---

## 🏗️ System Architecture

```
                   ┌─────────────────────┐
                   │     ESP32 MCU        │
                   │                     │
  Object Sensor ──►│  GPIO 13 (KIIT)     │
  Wet Bin Full  ──►│  GPIO 33 (KIIT)     │──► Servo Motor (GPIO 23)
  Dry Bin Full  ──►│  GPIO 14 (KIIT)     │
  Rain Sensor   ──►│  GPIO 21 (KIIT)     │
                   │                     │
  Object Sensor ──►│  GPIO 27 (Silicon)  │
  Wet Bin Full  ──►│  GPIO 26 (Silicon)  │──► Servo Motor (GPIO 22)
  Dry Bin Full  ──►│  GPIO 25 (Silicon)  │
  Rain Sensor   ──►│  GPIO 19 (Silicon)  │
                   └──────────┬──────────┘
                              │ Wi-Fi
                              ▼
                   ┌─────────────────────┐
                   │   Blynk IoT Cloud    │
                   │  V0 - KIIT Wet Bin  │
                   │  V1 - KIIT Dry Bin  │
                   │  V2 - Sil. Wet Bin  │
                   │  V3 - Sil. Dry Bin  │
                   └─────────────────────┘
```

---

## 🔧 Hardware Requirements

| Component | Quantity | Purpose |
|-----------|----------|---------|
| ESP32 Development Board | 1 | Main microcontroller |
| IR Object Sensor | 2 | Detect waste insertion |
| Rain / Moisture Sensor | 2 | Classify wet vs dry waste |
| IR / Ultrasonic Bin Level Sensor | 4 | Detect bin full status |
| Servo Motor (SG90 or MG996R) | 2 | Control waste diverting flap |
| Jumper Wires | — | Connections |
| Power Supply (5V/2A) | 1 | Power the system |
| Dustbin Enclosure | 2 | Physical housing |

---

## 📌 Pin Configuration

### KIIT University Dustbin

| Sensor/Actuator | ESP32 GPIO Pin |
|----------------|----------------|
| Object Sensor | 13 |
| Wet Bin Full Sensor | 33 |
| Dry Bin Full Sensor | 14 |
| Rain/Moisture Sensor | 21 |
| Servo Motor | 23 |

### Silicon University Dustbin

| Sensor/Actuator | ESP32 GPIO Pin |
|----------------|----------------|
| Object Sensor | 27 |
| Wet Bin Full Sensor | 26 |
| Dry Bin Full Sensor | 25 |
| Rain/Moisture Sensor | 19 |
| Servo Motor | 22 |

---

## 📱 Blynk Dashboard — Virtual Pin Mapping

| Virtual Pin | Location | Bin Type | Color Logic |
|-------------|----------|----------|-------------|
| V0 | KIIT | Wet Bin | 🟢 Green = Empty / 🔴 Red = Full |
| V1 | KIIT | Dry Bin | 🟢 Green = Empty / 🔴 Red = Full |
| V2 | Silicon | Wet Bin | 🟢 Green = Empty / 🔴 Red = Full |
| V3 | Silicon | Dry Bin | 🟢 Green = Empty / 🔴 Red = Full |

---

## ⚙️ Servo Angle Reference

| Position | Angle | Action |
|----------|-------|--------|
| Center (default) | 90° | Idle / Ready |
| Wet Bin | 150° | Direct waste to wet compartment |
| Dry Bin | 30° | Direct waste to dry compartment |

---

## 🛠️ Software Setup

### Prerequisites

- [Arduino IDE](https://www.arduino.cc/en/software) (v1.8+ or v2.x)
- ESP32 board package installed
- Blynk account at [blynk.cloud](https://blynk.cloud)

### Required Libraries

Install the following libraries via Arduino IDE → **Library Manager**:

| Library | Version |
|---------|---------|
| `Blynk` | Latest |
| `ESP32Servo` | Latest |
| `WiFi` (built-in with ESP32 core) | — |

### Installation

1. **Clone this repository:**
   ```bash
   git clone https://github.com/your-username/Smart-IoT-Waste-Segregation-Dustbin-System.git
   cd Smart-IoT-Waste-Segregation-Dustbin-System
   ```

2. **Open the sketch:**
   Open `smart_dustbin.ino` in Arduino IDE.

3. **Configure credentials** in the sketch:
   ```cpp
   #define BLYNK_TEMPLATE_ID   "YOUR_TEMPLATE_ID"
   #define BLYNK_TEMPLATE_NAME "YOUR_TEMPLATE_NAME"
   #define BLYNK_AUTH_TOKEN    "YOUR_AUTH_TOKEN"

   char ssid[] = "YOUR_WIFI_SSID";
   char pass[] = "YOUR_WIFI_PASSWORD";
   ```

4. **Select Board:** Tools → Board → `ESP32 Dev Module`

5. **Upload** the sketch to your ESP32.

---

## 🔄 How It Works

```
1. Waste Inserted
        │
        ▼
2. Object Sensor Detects (GPIO LOW)
        │
        ▼
3. 500ms delay (stabilization)
        │
        ├── Rain Sensor LOW ──► Wet Waste ──► Servo → 150°
        │
        └── Rain Sensor HIGH ─► Dry Waste ──► Servo → 30°
                │
                ▼
        4. 2s delay (waste falls)
                │
                ▼
        5. Servo Returns to 90° (center)
                │
                ▼
        6. Bin Level Sensors polled
                │
        ┌───────┴────────┐
        │ FULL (LOW)     │ EMPTY (HIGH)
        │ Send RED to    │ Send GREEN to
        │ Blynk V0-V3   │ Blynk V0-V3
```

---

## 📂 Project Structure

```
Smart-IoT-Waste-Segregation-Dustbin-System/
├── smart_dustbin/
│   └── smart_dustbin.ino       # Main Arduino sketch
├── docs/
│   ├── circuit_diagram.png     # Wiring diagram
│   └── blynk_dashboard.png     # Dashboard screenshot
├── README.md                   # Project documentation
├── CONTRIBUTING.md             # Contribution guidelines
├── LICENSE                     # MIT License
└── .gitignore                  # Git ignore rules
```

---

## 🖼️ Blynk Dashboard Setup

1. Log in to [blynk.cloud](https://blynk.cloud)
2. Create a new **Template** named `SMART DUSTBIN`
3. Add 4 **LED / Colored Indicator** widgets:
   - V0 → KIIT Wet Bin
   - V1 → KIIT Dry Bin
   - V2 → Silicon Wet Bin
   - V3 → Silicon Dry Bin
4. Copy your **Template ID** and **Auth Token** into the sketch

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting a pull request.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👥 Authors

Developed as part of an IoT project initiative for smart waste management at:
- **KIIT University (Kalinga Institute of Industrial Technology)**, Bhubaneswar
- **Silicon University**, Bhubaneswar

---

## 🌟 Acknowledgements

- [Blynk IoT Platform](https://blynk.io)
- [ESP32 Arduino Core](https://github.com/espressif/arduino-esp32)
- [ESP32Servo Library](https://github.com/madhephaestus/ESP32Servo)
