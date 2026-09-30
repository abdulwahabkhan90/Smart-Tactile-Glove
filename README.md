# 🧤 Smart Tactile Glove

### A Wearable Assistive Communication System Using Embedded Systems, Gesture Recognition & Haptic Feedback

<p align="center">
  <img src="https://img.shields.io/badge/Project-Final%20Year%20Project-blue" />
  <img src="https://img.shields.io/badge/Domain-Embedded%20Systems-orange" />
  <img src="https://img.shields.io/badge/ESP32--S3-Microcontroller-red" />
  <img src="https://img.shields.io/badge/BLE-Wireless%20Communication-purple" />
  <img src="https://img.shields.io/badge/Status-Completed-success" />
</p>

---

## 📌 Project Overview

**Smart Tactile Glove** is a wearable embedded-system prototype designed to provide an alternative method of communication through **finger gestures, hand movements, audio feedback, and tactile/haptic feedback**.

The system combines **flex sensors, an MPU6050 IMU, ESP32-S3, Bluetooth Low Energy (BLE), audio feedback, vibration feedback, and Morse-code-based tactile output** into a single wearable platform.

The glove allows a user to select letters through combinations of **hand tilt and finger bending**, construct words, perform editing commands, and receive feedback through audio and vibration.

The project was developed as a **Final Year Project in Computer Engineering** and was successfully demonstrated during the final project presentation and Open House.

---

## 🎯 Project Objectives

The main objectives of the project were to:

* Develop a wearable human-computer interaction system.
* Detect finger bending using flex sensors.
* Detect hand orientation and gestures using an MPU6050 IMU.
* Use an ESP32-S3 as the central embedded controller.
* Provide wireless communication through BLE.
* Convert selected gestures into letters and words.
* Provide audio feedback to the user.
* Provide tactile feedback using a vibration motor.
* Implement Morse-code-based vibration patterns.
* Develop different operating modes for letter selection and commands.
* Create a compact and practical wearable prototype.

---

# 🧠 System Concept

The Smart Tactile Glove uses two major interaction mechanisms:

### 1. Finger Detection

Five flex sensors are used to detect finger bending.

Each finger corresponds to a position within a selected alphabet group.

### 2. Hand Movement Detection

The MPU6050 detects changes in hand orientation.

Hand movements are used to:

* Select alphabet groups
* Switch operating modes
* Speak the constructed word
* Delete letters
* Add spaces
* Clear the complete word

This creates a **two-stage letter-selection mechanism**:

> **Hand Gesture → Alphabet Group → Finger Gesture → Letter**

For example:

```text
Hand Movement
      ↓
Select Alphabet Group
      ↓
Bend One Finger
      ↓
Select Letter
      ↓
Add Letter to Word
      ↓
Audio / Haptic Feedback
```

---

# ⚙️ Hardware Architecture

The prototype is based around an **ESP32-S3** microcontroller.

### Main Components

| Component        | Purpose                                |
| ---------------- | -------------------------------------- |
| ESP32-S3         | Main processing and control unit       |
| 5 × Flex Sensors | Finger-bending detection               |
| MPU6050          | Hand orientation and gesture detection |
| Audio Module     | Voice/audio feedback                   |
| Speaker          | Audio output                           |
| Vibration Motor  | Haptic feedback and Morse output       |
| Resistors        | Flex-sensor voltage-divider circuits   |
| Glove            | Wearable platform                      |
| BLE              | Wireless communication                 |

---

# 🔌 Pin Configuration

### Flex Sensors

| Flex Sensor | ESP32-S3 GPIO |
| ----------- | ------------: |
| Flex 1      |        GPIO 4 |
| Flex 2      |        GPIO 5 |
| Flex 3      |        GPIO 1 |
| Flex 4      |        GPIO 2 |
| Flex 5      |        GPIO 8 |

Each flex sensor is connected using a voltage-divider configuration.

```text
3.3V
 │
Flex Sensor
 │
 ├──── ESP32 ADC GPIO
 │
10kΩ
 │
GND
```

### MPU6050

The MPU6050 communicates with the ESP32-S3 using **I²C**.

| MPU6050 | ESP32-S3 |
| ------- | -------- |
| SDA     | GPIO 6   |
| SCL     | GPIO 7   |
| VCC     | 3.3V     |
| GND     | GND      |

### Vibration Motor

```text
ESP32-S3 GPIO 10
        ↓
 Vibration Motor
```

### Audio Module

The prototype uses a serial interface between the ESP32-S3 and the audio playback module.

The firmware configuration uses:

```text
ESP32 GPIO 19 → RX2
ESP32 GPIO 18 → TX2
```

---

# 🔤 Letter Selection System

The alphabet is divided into five groups:

| Group   | Letters |
| ------- | ------- |
| Group 1 | A – E   |
| Group 2 | F – J   |
| Group 3 | K – O   |
| Group 4 | P – T   |
| Group 5 | U – Y   |

The user first selects a group using a hand movement.

Then, while the group is active, one of the five fingers is bent to select a specific letter.

### Example

Suppose **Group 2 (F–J)** is selected:

```text
Finger 1 → F
Finger 2 → G
Finger 3 → H
Finger 4 → I
Finger 5 → J
```

This significantly reduces the number of individual gestures required to represent the alphabet.

---

# 🖐️ Operating Modes

The system contains two primary modes.

## 🟦 Letter Mode

Letter Mode is used to construct words.

The user:

1. Selects an alphabet group using hand movement.
2. Bends a finger.
3. The corresponding letter is identified.
4. The letter is added to the word buffer.
5. Audio and vibration feedback are provided.

Example:

```text
Select Group
     ↓
Select Finger
     ↓
Letter Detected
     ↓
Letter Added
     ↓
Feedback
```

---

## 🟥 Command Mode

Command Mode provides additional controls for manipulating the constructed word.

The available commands include:

* Speak the current word
* Delete the last letter
* Add a space
* Clear the complete word

The mode is controlled through MPU6050-based hand gestures.

---

# 📳 Haptic / Morse Feedback

One of the distinctive features of the project is the use of **vibration-based Morse output**.

Letters can be represented using:

* `.` → Short vibration
* `-` → Long vibration

For example:

```text
A = .-
B = -...
C = -.-.
D = -..
E = .
```

The system converts text into Morse patterns and drives the vibration motor accordingly.

This provides a tactile feedback mechanism in addition to conventional audio feedback.

---

# 🔊 Audio Feedback

Audio feedback is used to make the interaction more intuitive.

The system can provide audio feedback for:

* Mode changes
* Alphabet group selection
* Individual letters
* Recognized words
* System events

For predefined words, dedicated audio tracks can be played.

For other text, the system can process the word letter-by-letter.

---

# 📡 Bluetooth Low Energy

The ESP32-S3 provides BLE communication between the glove and an external device.

The BLE interface allows text information to be received wirelessly.

The system uses a custom BLE service and characteristic for communication.

This makes the glove more flexible and provides a foundation for integration with a mobile application or other wireless interface.

---

# 🧩 Software Architecture

The firmware is structured around several functional modules:

```text
                    ESP32-S3
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   Flex Sensors     MPU6050          BLE
        │              │              │
        └───────┬──────┴───────┬──────┘
                │              │
          Gesture Processing   │
                │              │
          State Machine        │
                │              │
       ┌────────┴────────┐     │
       │                 │     │
  Letter Mode      Command Mode│
       │                 │     │
       └────────┬────────┘     │
                │              │
          Word Buffer          │
                │              │
        ┌───────┴────────┐     │
        │                │     │
   Audio Feedback   Haptic/Morse
```

---

# 🔄 State Machine

The system uses a simple finite-state-machine approach:

```text
             ┌───────────────┐
             │  LETTER MODE  │
             └───────┬───────┘
                     │
              Mode Gesture
                     ↓
             ┌───────────────┐
             │ COMMAND MODE  │
             └───────┬───────┘
                     │
              Mode Gesture
                     ↓
             ┌───────────────┐
             │  LETTER MODE  │
             └───────────────┘
```

This separates normal letter entry from word-management commands and simplifies the interaction logic.

---

# 🛠️ Technologies Used

### Hardware

* ESP32-S3
* MPU6050
* Flex Sensors
* Vibration Motor
* Audio Playback Module
* Speaker
* I²C
* UART
* ADC
* BLE

### Software

* Arduino Framework
* C/C++
* ESP32 BLE
* Adafruit MPU6050 Library
* Adafruit Unified Sensor Library
* DFPlayer Mini Library
* I²C Communication
* UART Communication
* Finite State Machine

---

# 📊 Key Engineering Concepts Demonstrated

This project combines multiple areas of embedded and computer engineering:

* Embedded Systems
* Microcontroller Programming
* Sensor Interfacing
* Analog-to-Digital Conversion
* I²C Communication
* UART Communication
* Bluetooth Low Energy
* Human-Computer Interaction
* Gesture Recognition
* State-Machine Design
* Haptic Feedback
* Audio Feedback
* Wearable Technology
* Real-Time Embedded Processing

---

# 🧪 Testing & Validation

Individual components were tested during development before integration.

### Component-Level Testing

* ESP32-S3 communication and programming
* MPU6050 sensor output
* Flex sensor readings
* Audio playback
* Vibration motor operation
* BLE communication

### System-Level Testing

The integrated glove was tested for:

* Hand gesture recognition
* Finger-based letter selection
* Alphabet group selection
* Word construction
* Command-mode operations
* Audio feedback
* Haptic feedback
* Morse-code vibration
* BLE communication

---

# 📸 Project Demonstration

The final prototype was successfully demonstrated during the **Final Year Project presentation and Open House**.

The demonstration showcased the complete interaction between:

```text
User
 ↓
Hand Gesture
 ↓
Sensors
 ↓
ESP32-S3
 ↓
Gesture Processing
 ↓
Letter / Command
 ↓
Audio + Haptic Feedback
```

---

# 🚧 Challenges

Several engineering challenges were encountered during development, including:

### Sensor Calibration

Flex sensors produce analog values that vary according to bending angle and physical positioning. Thresholds therefore required testing and calibration.

### Gesture Reliability

The MPU6050 readings had to be interpreted carefully to distinguish intentional gestures from normal hand movement.

### Hardware Integration

Integrating multiple peripherals on a compact wearable platform required careful consideration of:

* GPIO availability
* Power requirements
* Communication interfaces
* Wiring
* Ground connections
* Sensor interference

### Real-Time Feedback

Audio, vibration, sensor processing, BLE communication, and gesture detection had to operate together without making the user interaction feel slow or unresponsive.

### Wearable Design

The electronics had to remain practical for mounting on a glove while maintaining reliable sensor connections.

---

# 🔮 Future Improvements

Potential future development includes:

* Improved gesture-recognition algorithms
* Machine-learning-based gesture classification
* Automatic sensor calibration
* Wireless configuration of gesture thresholds
* Improved wearable PCB design
* Smaller and more integrated electronics
* Rechargeable battery-powered operation
* Improved mobile application integration
* More extensive vocabulary
* Multilingual audio support
* Improved tactile feedback patterns
* Cloud-based data and configuration management
* More comprehensive user testing

---

# 📁 Repository Status

> **Note:** This repository currently serves as a **project showcase and technical documentation repository**.

The original development source files are not currently included in this public repository. The documentation describes the final prototype, system architecture, hardware configuration, interaction methodology, and implementation concepts.

Source code can be added in a future repository revision when the original development files are recovered and organized.

---

# 🎓 Academic Project

**Project:** Smart Tactile Glove
**Category:** Final Year Project
**Degree:** BS Computer Engineering
**University:** HITEC University, Taxila, Pakistan
**Project Duration:** September 2025 – June 2026
**Supervisor:** Engr. Tehseen Ahsan

The project was developed as an undergraduate engineering project combining embedded systems, wearable technology, sensor interfacing, wireless communication, and human-computer interaction.

---

# 👨‍💻 Project Team

This project was developed as part of the Final Year Design Project at HITEC University.

### Supervision

**Engr. Tehseen Ahsan**

---

# ⭐ Why This Project Matters

The Smart Tactile Glove demonstrates how multiple embedded technologies can be integrated into a single wearable system to create an alternative human-computer interaction mechanism.

Rather than relying on a conventional keyboard, touchscreen, or speech interface, the system uses:

> **Finger Bending + Hand Gestures + Audio + Vibration + Wireless Communication**

to create a wearable communication interface.

---

## 📌 Project Highlights

```text
🧤 Wearable Embedded System
⚡ ESP32-S3 Based Architecture
🖐️ 5-Finger Gesture Detection
📐 MPU6050 Motion Sensing
📡 Bluetooth Low Energy
🔊 Audio Feedback
📳 Haptic Feedback
•- Morse Code Output
🔄 Dual Operating Modes
🔤 Gesture-Based Alphabet Selection
```

---

## 📜 License

This repository is intended primarily for **academic, educational, and portfolio purposes**.

Please contact the project author before using the complete project concept or documentation for commercial purposes.

---

## 🚀 Project Status

**COMPLETED ✅**

Final Year Project successfully developed, demonstrated, and presented.

---

<p align="center">

### 🧤 Smart Tactile Glove

**Turning Hand Gestures into Communication**

*Embedded Systems • Wearable Technology • BLE • Haptic Feedback*

</p>
