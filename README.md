# ICU Smart Patient Monitoring System (Simulation)

A software-based ICU patient monitoring system built with Python and Flask that simulates real-time tracking of patient vital parameters — heart rate, SpO₂, and movement — with automatic alerts when abnormal conditions are detected.

## Overview

This project simulates real-time ICU patient monitoring using video analysis and simulated sensor data instead of physical hardware. The system continuously analyzes a patient video feed alongside generated vital-sign data. If it detects an abnormal condition — high heart rate, low SpO₂, or no movement — it triggers an alert that's displayed instantly on a web dashboard for doctors or attenders.

It's a low-cost, scalable approach to ICU monitoring that avoids the need for physical sensors during prototyping and testing.

## Objectives

- Simulate ICU patient monitoring using software only
- Analyze patient video and vital parameters together
- Automatically detect abnormal conditions
- Alert doctors/attenders instantly through a web interface

## System Architecture

```
Patient Video (MP4) + Simulated Vital Data
              ↓
     Analysis Engine (Python + OpenCV)
              ↓
          Flask Server
              ↓
        Web Dashboard
              ↓
     Doctor / Attender
```

## How It Works

### 1. Patient Data Simulation
- Heart Rate: 60–120 bpm
- SpO₂: 90–100%
- Generated using Python's `random` module

### 2. Video Analysis
- Uses a pre-recorded patient video in place of a live camera feed
- Motion detection applied via OpenCV
- No movement detected → flagged as a critical condition

### 3. Condition Detection

| Parameter | Condition | Status |
|---|---|---|
| HR > 100 | High heart rate | Alert |
| SpO₂ < 92 | Low oxygen | Critical |
| No movement | Motion failure | Emergency |

### 4. Backend (Flask)
Handles data processing, video streaming, and API communication between the analysis engine and the dashboard.

### 5. Web Dashboard
Displays the patient video feed, live vital parameters, and current alert status in real time.

## Results

**Emergency Alert — Low SpO₂**
![Low SpO2 Alert](assets/result-low-spo2-alert.jpeg)

**No Movement Detection**
![No Movement Alert](assets/result-no-movement-alert.jpeg)

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python |
| Video Analysis | OpenCV |
| Backend | Flask |
| Editor | Visual Studio Code |

## Advantages

- Low cost — no physical hardware required
- Real-time monitoring and alerting
- Remotely accessible via web dashboard
- Simple to set up and extend

## Limitations

- Uses simulated vital data rather than readings from real medical sensors
- Currently supports monitoring a single patient at a time
- No persistent database — alerts and readings aren't stored for later review
- Motion detection relies on a fixed camera angle and pre-recorded video

## Future Enhancements

- Integration with real IoT vital-sign sensors
- AI-based health prediction and anomaly detection
- Mobile application for on-the-go alerts
- Cloud database for persistent record-keeping
- Multi-patient monitoring support

## Applications

- ICU patient monitoring
- Remote healthcare and telemedicine
- Smart hospital systems

## Repository Structure

```
├── app.py                # Flask backend
├── analysis/              # Video + vital sign analysis logic
├── templates/              # Web dashboard HTML
├── static/                  # CSS/JS assets for dashboard
├── assets/                   # Result screenshots used in this README
└── README.md
```

## Conclusion

This project demonstrates a smart ICU monitoring system using simulation. By combining video analysis with simulated vital-sign monitoring, it detects abnormal conditions and alerts doctors in real time, with a clear path toward becoming a real-world healthcare solution through IoT and AI integration.

## Author

**Sweatha S**
[LinkedIn](https://www.linkedin.com/in/sweatha-s-39002039b) | [GitHub](https://github.com/Sweatha-2)
