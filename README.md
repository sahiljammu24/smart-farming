# 🌱 Smart Farm Control System

![Project Status](https://img.shields.io/badge/Status-State%20Level%20Competition-orange?style=for-the-badge&logo=fire)
![Category](https://img.shields.io/badge/Category-AgriTech-green?style=for-the-badge&logo=leaf)
![Platform](https://img.shields.io/badge/Platform-Web%20%2B%20Arduino-blue?style=for-the-badge&logo=arduino)

## 🏆 Science Fair Representation
**State Level Science Fair Entry - Agricultural Innovation**

<p style="font-size: 20px;">
    <strong>Created by:</strong> Sahil Jammu<br>
    <strong>Represented by:</strong> Samuel Bhatti
</p>

---

## 📖 Overview
The **Smart Farm Control System** is a modern, web-based IoT dashboard designed to revolutionize small-scale farming. By leveraging the **Web Serial API**, it connects directly to an Arduino controller directly through the browser—eliminating the need for complex cloud backends or internet connectivity.

This project empowers farmers with:
*   **Precision Agriculture:** Real-time soil moisture monitoring.
*   **Remote Control:** Manual toggling of irrigation pumps and sprayers.
*   **Automation:** Intelligent scheduling and threshold-based auto-watering.
*   **Accessibility:** A plug-and-play solution that works on standard web browsers.

## ✨ Key Features

### 🖥️ Smart Dashboard (Web Interface)
*   **Real-time Visualization:** Live gauge bars for Left/Right soil moisture sensors.
*   **Voice Command:** Hands-free control using Web Speech API.
*   **Weather Intelligence:** Local weather integration to advise on irrigation (uses Open-Meteo).
*   **Data Analytics:** Interactive charts tracking moisture trends over time (exportable to CSV).
*   **Smart Scheduler:** Timer-based execution for routine tasks.
*   **Crop Presets:** One-click configurations for specific crop needs.
*   **Theming:** Auto-detects system Dark/Light mode preferences.

### 🤖 Hardware Integration
*   **Direct Control:** Manages relays for water pumps and pesticide sprayers.
*   **Sensor Feedback:** Reads analog data from capacitive/resistive soil sensors.

---

## 🏗️ System Architecture

The system follows a **Client-Hardware** architecture using Serial communication.

![System Architecture](assets/architecture_diagram.svg)

```mermaid
    *   **Action:** Valid commands are sent to the Arduino via Web Serial.
    *   **Visual:** A stackable toast notification appears.
    *   **Audio:** The system uses **Text-to-Speech (TTS)** to verbally confirm the action (e.g., "Pump started") or explain errors (e.g., "Disable auto mode first").
graph LR
    User((User)) -->|Clicks/Voice| Dashboard[Web Dashboard]
    Dashboard <-->|Web Serial (USB)| Arduino[Arduino Uno]
    
    subgraph "Browser Environment"
        Dashboard
        Logic[JS Controller]
        Voice[Speech Recog]
        Weather[Weather API]
    end
    
    subgraph "Field Hardware"
        Arduino
        Relay1[Pump Relay]
        Relay2[Sprayer Relay]
        Sensors[Soil Sensors]
    end
    
    Logic --> Dashboard
    Weather -.-> Logic
    Arduino --> Relay1
    Arduino --> Relay2
    Sensors -->|Analog| Arduino
    
```

### 🎤 Voice Commands
The system leverages the **Web Speech API** for advanced hands-free control. This eliminates the need for external hardware like Alexa or Google Home, as the processing happens directly in the browser.

#### How it Works:
*  **Speech-to-Text:** When activated, the browser's native `webkitSpeechRecognition` engine listens to microphone input and converts spoken audio into a text string.
*  **Feedback Loop:**
    *   **Action:** Valid commands are sent to the Arduino via Web Serial.
    *   **Visual:** A stackable toast notification appears.
    *   **Audio:** The system uses **Text-to-Speech (TTS)** to verbally confirm the action (e.g., "Pump started") or explain errors (e.g., "Disable auto mode first").

| Command Group | Phrases (Case-insensitive)              | Action                                          |
| :--- |:----------------------------------------|:------------------------------------------------|
| **Pump Control** | "Turn on / off pump", "Start/Stop pump" | Activates / Deactivates Water Pump Relay        |
| **Spray Control** | "Turn on / off spray"                   | Activates / Deactivates Pesticide Sprayer Relay |
| **Auto Mode** | "Auto mode on / off"                    | Enables / Disables threshold-based watering             |

## 🖼️ Dashboard Preview
![Dashboard Mockup](assets/dashboard_mockup.png)

---

## 🎮 Usage Guide

### 🤖 Auto Mode Logic
When enabled, the system automatically manages irrigation based on soil moisture levels.
*   **Trigger Condition:** `Avg Moisture < Low Threshold` → **Pump ON**
*   **Stop Condition:** `Avg Moisture > High Threshold` → **Pump OFF**
*   **Note:** Manual pump controls are disabled while Auto Mode is active to prevent conflicts.

### 🌾 Crop Presets
One-click settings for common crops:
*   **🍅 Tomato:** 60% - 80% (Moist)
*   **🌶️ Chili:** 50% - 70% (Moderate)
*   **🍚 Rice:** 70% - 90% (High Moisture)
*   **🌾 Wheat:** 80% - 100% (Very High)

---

## 🔌 Hardware Setup

### 🛠️ Bill of Materials (Components)

| Component | Image | Description |
| :--- | :---: | :--- |
| <h2>Arduino Uno R3</h2> | <img src="assets/img.png" width="500" /> | <p style="font-size: 20px">The central microcontroller unit (ATmega328P).</p> |
| <h2>Soil Sensor</h2> | <img src="assets/soil_sensor.png" width="500" /> | <p style="font-size: 20px">Capacitive/Resistive Soil Moisture Sensor v1.2.</p> |
| <h2>Relay Module</h2> | <img src="assets/relay.png" width="500" /> | <p style="font-size: 20px">5V Relay module to control high-voltage pumps.</p> |
| <h2>Breadboard</h2> | <img src="assets/anatomy of breadboard.png" width="600" /> | <p style="font-size: 20px">Used for prototyping and connecting components.</p> |

### ⚡ Circuit & Wiring

#### Circuit Diagram
The following diagram illustrates the complete wiring of the system:
![Circuit Diagram](assets/circuit_diagram.svg)

#### Wiring Reference
**Note:** Use the red numbers in parentheses as your reference. Analog pins are addressed as `A0` through `A5`.

| Component | Arduino Pin | Type | Notes |
| :--- | :---: | :--- | :--- |
| **Relay 1 (Pump)** | `3` | Digital Output | Connected to external pump |
| **Relay 2 (Sprayer)** | `4` | Digital Output | Connected to sprayer unit |
| **Moisture Sensor L** | `A0` | Analog Input | Left field sensor |
| **Moisture Sensor R** | `A1` | Analog Input | Right field sensor |

### ⚙️ Technical Specifications (Arduino Uno R3)
*   **Microcontroller:** ATmega328P
*   **Operating Voltage:** 5V
*   **Input Voltage:** 7-12V (Recommended)
*   **Digital Pins:** 14 (of which 6 provide PWM)
*   **Analog Pins:** 6

---

## 🧠 Calibration Logic
The system reads raw analog values (0-1023) from the moisture sensors. Since soil sensors return high values for dry soil and low values for wet soil (resistive nature), the code inverts and maps this to a percentage:
```cpp
// 1023 (Dry) -> 0%
// 0 (Wet)    -> 100%
int percent = map(rawReading, 1023, 0, 0, 100);
```

### ⚠️ Firmware Protocol Note
The repository contains two firmware concepts. 
1.  **`uno_code/uno_code.ino`**: The basic serial firmware.
2.  **Web-Compatible Firmware**: The web dashboard sends text commands (`PUMP_ON`, `STATUS`). **Use the code below** for full dashboard functionality.

<details>
<summary><b>Click to view Compatible Arduino Sketch</b></summary>

```cpp
// Flash this to your Arduino for the Dashboard to work!

const int PUMP_PIN = 3;
const int SPRAY_PIN = 4;
const int SENSOR_L_PIN = A0;
const int SENSOR_R_PIN = A1;

String pumpState = "OFF";
String sprayState = "OFF";

void setup() {
  Serial.begin(9600); // Matches Web Dashboard Baud Rate
  pinMode(PUMP_PIN, OUTPUT);
  pinMode(SPRAY_PIN, OUTPUT);
  
  // Initialize states
  digitalWrite(PUMP_PIN, LOW);
  digitalWrite(SPRAY_PIN, LOW);
}

void loop() {
  if (Serial.available() > 0) {
    String command = Serial.readStringUntil('\n');
    command.trim(); // Remove whitespace

    if (command == "PUMP_ON") {
      digitalWrite(PUMP_PIN, HIGH);
      pumpState = "ON";
    } else if (command == "PUMP_OFF") {
      digitalWrite(PUMP_PIN, LOW);
      pumpState = "OFF";
    } else if (command == "SPRAY_ON") {
      digitalWrite(SPRAY_PIN, HIGH);
      sprayState = "ON";
    } else if (command == "SPRAY_OFF") {
      digitalWrite(SPRAY_PIN, LOW);
      sprayState = "OFF";
    } else if (command == "STATUS") {
      sendData();
    }
  }
}

void sendData() {
  // Read Sensors
  int rawL = analogRead(SENSOR_L_PIN);
  int rawR = analogRead(SENSOR_R_PIN);
  
  // Map 1023-0 (Dry-Wet) to 0-100%
  int pctL = map(rawL, 1023, 0, 0, 100); 
  int pctR = map(rawR, 1023, 0, 0, 100);
  
  // Constrain to 0-100
  pctL = constrain(pctL, 0, 100);
  pctR = constrain(pctR, 0, 100);

  // Format: DATA|Left|Right|PumpState|SprayState
  Serial.print("DATA|");
  Serial.print(pctL); Serial.print("|");
  Serial.print(pctR); Serial.print("|");
  Serial.print(pumpState); Serial.print("|");
  Serial.println(sprayState);
}
```
</details>

---

## 🚀 Getting Started

1.  **Hardware Prep:**
    *   Connect your Arduino via USB.
    *   Upload the **Compatible Arduino Sketch** (above) using Arduino IDE.
2.  **Launch Dashboard:**
    *   Open `index.html` in **Chrome** or **Edge** (Web Serial is not supported in Firefox/Safari).
3.  **Connect:**
    *   Click **"Connect Device"** on the dashboard.
    *   Select your Arduino COM port from the popup list.
    *   Status should turn **Connected ✅**.
4.  **Operate:**
    *   Use the toggle switches or Voice Commands.
    *   Enable **Auto Mode** to let the system manage watering based on the slider thresholds.

## ❓ Troubleshooting

| Issue | Possible Cause | Solution |
| :--- | :--- | :--- |
| **"Connect Device" button does nothing** | Browser incompatibility | Use **Chrome** or **Edge**. Firefox/Safari do not support Web Serial API. |
| **Status says "Connected" but no data** | Wrong Baud Rate | Ensure Arduino code has `Serial.begin(9600)`. |
| **Sensors stuck at 0% or 100%** | Loose wiring or Calibration | Check connections to A0/A1. Try dipping sensor in water to test range. |
| **Voice commands not working** | Microphone permission | Allow microphone access in browser settings. |

## 🔮 Future Roadmap
*   **Hardware:** Add NPK sensors for nutrient analysis.
*   **Mobile:** Wrap the web app into a PWA (Progressive Web App) for mobile installation.
*   **AI:** Train a model on the CSV data exports to predict optimal harvest times.

