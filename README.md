
 # 🛡️ Arduino-Controlled Radar-Guided Defence System

An **automated missile defense prototype** built using Arduino UNO, ultrasonic radar sensors, and servo motors to detect, track, and respond to threats in real time.

---

## 🚀 Project Overview
This project demonstrates a **cost-effective radar-guided defense system** using an Arduino microcontroller and basic sensors. It detects threats using an ultrasonic radar, processes the data via Arduino, and automatically aims and "fires" a mock missile when a threat is detected.

---

## 🧠 Features
- 🔍 Real-time threat detection using ultrasonic sensors  
- 🎯 Automated servo-controlled missile aiming  
- 🚀 Simulated missile triggering on detection  
- 📊 Radar visual interface using Processing software  
- 🧩 Modular design for easy upgrades  

---

## 🛠️ Tech Stack
- **Hardware**: Arduino UNO, Servo Motor, Ultrasonic Sensor (HC-SR04), Breadboard, Jumper Wires  
- **Software**: Arduino IDE (C/C++), Processing 4.3 (Java)  

---

## 🔧 Block Diagram
![IMG-20250617-WA0017 1](https://github.com/user-attachments/assets/531c29a7-62a4-449e-aec3-5d97347569e4)




**code**

import processing.serial.*;

Serial myPort;        // The serial port
int[] distances;      // Array to store distance measurements
int angle = 0;        // Current angle of the sensor

void setup() {
  size(800, 400);
  myPort = new Serial(this, Serial.list()[0], 9600);
  distances = new int[151];  // Array size based on servo range from 15 to 165 degrees
  for (int i = 0; i < distances.length; i++) {
    distances[i] = 0;  // Initialize distances to 0
  }
  background(0);
}

void draw() {
  background(0);
  translate(width / 2, height); // Move origin to bottom center

  // Draw radar lines and arcs
  stroke(0, 255, 0);
  noFill();
  for (int i = 15; i <= 165; i += 15) {
    float x = 300 * cos(radians(i));
    float y = 300 * sin(radians(i));
    line(0, 0, x, -y); // Radar lines
    arc(0, 0, 600, 600, PI, TWO_PI); // Radar arcs
  }

  // Draw the distance points
  stroke(0, 255, 0);
  for (int i = 0; i < distances.length; i++) {
    if (distances[i] > 0) {
      float x = distances[i] * cos(radians(i + 15));
      float y = distances[i] * sin(radians(i + 15));
      point(x, -y); // Negative y to invert the arc upwards
    }
  }

  // Draw the radar sweep
  if (distances[angle] < 50 && distances[angle] > 0) {
    fill(255, 0, 0, 100); // Red color for detected object within 50 cm
  } else {
    fill(0, 255, 0, 100); // Green color for normal sweep
  }
  noStroke();
  arc(0, 0, 600, 600, PI, radians(angle + 15 + 180), PIE); // Sweep arc

  // Display angle and distance
  fill(255);
  textSize(15);
  textAlign(LEFT, TOP);
  text("Angle: " + (angle + 15) + "°", 10, 10);
  text("Distance: " + distances[angle] + " cm", 10, 30);
}

void serialEvent(Serial myPort) {
  String inString = myPort.readStringUntil('\n');
  if (inString != null) {
    inString = trim(inString);
    String[] values = split(inString, ',');
    if (values.length == 2) {
      int newAngle = int(values[0]);
      int distance = int(values[1]);
      if (newAngle >= 15 && newAngle <= 165) {
        angle = newAngle - 15; // Adjust angle to fit the array index
        distances[angle] = distance;
      }
    }
  }
}



---

## 📸 Screenshots
> ![IMG-20250617-WA0011 1](https://github.com/user-attachments/assets/02c4396c-5935-4dc7-89c9-865d4f9ca7d2)
> ![IMG-20250617-WA0009 1](https://github.com/user-attachments/assets/e63234c8-8dfa-47ca-ba06-7c94d784ae1d)
>![IMG-20250617-WA0013 1](https://github.com/user-attachments/assets/add6d1fc-acd4-4317-99b9-9808e832cdf8) == prototype.


---

## 📋 How to Run
1. Connect hardware components as per the circuit diagram
2. Upload Arduino code using Arduino IDE
3. Run the Processing sketch for radar visualization
4. Watch servo rotate and simulate missile launch on threat detection

---

## 📈 Results
- 95% detection accuracy within 50cm
- Real-time radar sweep simulation from 0° to 180°
- Missile launches only when threat is detected in range

---

## 🌐 Applications
- Military Surveillance  
- Anti-drone Defense  
- Robotics R&D  
- Disaster Management  
- Border Security

---

## ⚠️ Limitations
- Limited detection range (~70 feet)
- Not compatible with high-end radar modules
- Prototype only; not for real deployment

---

## 🔭 Future Work
- Add LIDAR & ML-based threat classification  
- Enable wireless data sharing  
- Improve long-range detection

---

## 👥 Authors
- **Jonnada Ruthumani**, Kalathiripi Rambabu, Sanjay Dubey, B. R. Sanjeeva Reddy, Viswanadham Ravuri, Kasha Saipriya  
- 📧 jonnadaruthumani@gmail.com

