# 🌱 Environmental Monitoring & Obstacle Detection Rover

An advanced **environmental monitoring and obstacle detection system** designed to collect real-time data on **temperature, humidity, air quality, and soil moisture**, while ensuring safe navigation using an **ultrasonic obstacle detection sensor**. Data is visualized on an **interactive dashboard** for analysis and decision-making.  

---

## 📌 Overview
The rover integrates multiple sensors to monitor environmental parameters and navigate autonomously:

- Real-time **temperature, humidity, air quality, and soil moisture measurement**  
- **Obstacle detection** for safe movement in diverse terrains  
- **Interactive dashboard** for data visualization and analysis  

---

## 🛠️ Sensor Modules & Their Functions

### 🌡️ DHT11 Sensor – Temperature & Humidity
- Accurately measures surrounding temperature and humidity  
- Helps with **weather prediction** and **climate monitoring**  

### 🌬️ MQ135 Air Quality Sensor
- Detects harmful gases and evaluates **air pollution levels**  
- Ideal for **environmental monitoring applications**  

### 💧 Soil Moisture Sensor
- Measures moisture content in the soil  
- Supports **agriculture & irrigation optimization**  

### 🚧 Ultrasonic Sensor – Obstacle Detection
- Ensures safe navigation by detecting obstacles in the rover’s path  
- Useful for **autonomous or semi-autonomous movement**  

---

## 📊 Dashboard System
A **user-friendly interface** to visualize all collected data in real time:  

- **Live Temperature & Humidity Readings**  
- **Air Quality Index Visualization**  
- **Soil Moisture Levels**  
- **Obstacle Detection Alerts**  

### 🖼️ Dashboard Screenshots
![image](https://github.com/user-attachments/assets/62300e40-e0b6-4042-a4a5-2890f798458e)
![image](https://github.com/user-attachments/assets/8f6228f3-b51a-4f75-9a8c-d617b2e2f890)
![image](https://github.com/user-attachments/assets/799dea97-9a90-42d4-9378-a2bfd335019d)
![image](https://github.com/user-attachments/assets/435ccab6-a185-4d15-a678-59145fa310b8)

---

## ⚡ Applications
- **Environmental Monitoring:** Track air quality, temperature, and humidity trends  
- **Agriculture & Irrigation:** Optimize watering schedules using soil moisture data  
- **Autonomous Navigation:** Avoid obstacles for safe movement  
- **Research & Development:** Study climatic conditions and pollution patterns  

---

## 💻 Code Overview

### ESP32 Code – Data Collection & Transmission
```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const char* ssid = "Your_SSID";
const char* password = "Your_PASSWORD";
const char* serverUrl = "http://your-server-url.com/data";

void setup() {
    Serial.begin(115200);
    dht.begin();
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Connecting to WiFi...");
    }
    Serial.println("Connected to WiFi");
}

void loop() {
    float temperature = dht.readTemperature();
    float humidity = dht.readHumidity();
    if (isnan(temperature) || isnan(humidity)) {
        Serial.println("Failed to read from DHT sensor!");
        return;
    }

    HTTPClient http;
    http.begin(serverUrl);
    http.addHeader("Content-Type", "application/json");
    String jsonPayload = "{\"temperature\":" + String(temperature) + ",\"humidity\":" + String(humidity) + "}";
    int httpResponseCode = http.POST(jsonPayload);
    Serial.println("Data sent: " + jsonPayload);
    http.end();
```

## React.js Code – Dashboard Data Fetching
```cpp
import React, { useEffect, useState } from "react";

const Dashboard = () => {
    const [data, setData] = useState({ temperature: "--", humidity: "--" });

    useEffect(() => {
        const fetchData = async () => {
            const response = await fetch("http://your-server-url.com/data");
            const result = await response.json();
            setData(result);
        };
        fetchData();
    }, []);

    return (
        <div>
            <h2>Environmental Monitoring Dashboard</h2>
            <p>Temperature: {data.temperature}°C</p>
            <p>Humidity: {data.humidity}%</p>
        </div>
    );
};

export default Dashboard;
```

## Conclusion

This rover, combined with its **real-time dashboard**, is a complete solution for **environmental monitoring, research, agriculture**, and **autonomous navigation**.
It demonstrates the integration of **hardware sensors, IoT**, and **web-based visualization to provide actionable insights in real-time**.

    
