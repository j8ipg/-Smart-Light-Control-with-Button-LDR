part1
# 🌞 Smart Light Control with Button & LDR

## 📌 Description

This Arduino project controls an LED using:
- A **button** for manual ON/OFF toggling (latching behavior).
- An **LDR (Light Dependent Resistor)** to turn the LED **on only in the dark**.

When the system is enabled via the button:
- The LED lights up in darkness (LDR detects low light).
- The LED turns off in light automatically.
- A quick LED blink indicates the system state:
  - 🔁 **1 blink** → system OFF
  - 🔁 **2 blinks** → system ON

## 🧰 Components

| Component          | Quantity |
|-------------------|----------|
| Arduino Uno        | 1        |
| Breadboard         | 1        |
| LDR (photoresistor)| 1        |
| Push button        | 1        |
| LED                | 1        |
| 10kΩ resistor      | 1        |
| 220Ω resistor      | 1        |
| Jumper wires       | as needed |

## 🔌 Circuit Diagram:

<img width="1920" height="848" alt="task3 elcter" src="https://github.com/user-attachments/assets/f7fe55fe-b6de-436b-8aa1-ada70817a1eb" />




### 🧠 Connections:

| Component | Arduino Pin         |
|----------|---------------------|
| Button   | D0 (INPUT_PULLUP)   |
| LDR      | D2 (INPUT_PULLUP)   |
| LED      | D4 (with 220Ω)      |

**LDR wiring note:**
- One leg of the LDR → GND  
- Other leg → D2 and to VCC via 10kΩ resistor



## 🖥 Code

```cpp
const int ledPin = 4;
const int ldrPin = 2;
const int buttonPin = 0;

bool circuitEnabled = false;

bool lastButtonReading = HIGH;
bool buttonState = HIGH;
unsigned long lastDebounceTime = 0;
const unsigned long debounceDelay = 50;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ldrPin, INPUT_PULLUP);
  digitalWrite(ledPin, LOW);
}

void loop() {
  bool currentReading = digitalRead(buttonPin);

  if (currentReading != lastButtonReading) {
    lastDebounceTime = millis();
  }

  if ((millis() - lastDebounceTime) > debounceDelay) {
    if (currentReading != buttonState) {
      buttonState = currentReading;

      if (buttonState == LOW) {
        circuitEnabled = !circuitEnabled;

        if (circuitEnabled) {
          blinkLED(1);
        } else {
          blinkLED(2);
        }
      }
    }
  }

  lastButtonReading = currentReading;

  if (circuitEnabled) {
    if (digitalRead(ldrPin) == LOW) {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  } else {
    digitalWrite(ledPin, LOW);
  }
}

void blinkLED(int times) {
  for (int i = 0; i < times; i++) {
    digitalWrite(ledPin, HIGH);
    delay(150);
    digitalWrite(ledPin, LOW);
    delay(150);
  }
}
  ```


---



## 🟢 How to Run

1. Connect the components as shown in the diagram.
2. Upload the code to your Arduino.
3. Press the button to enable or disable the system.
4. Cover the LDR to simulate darkness and trigger the LED.

---

## 📌 What You Learn

- How to debounce a button.
- How to toggle states (latch) using software.
- Using an LDR to detect light/dark conditions.
- Combining manual + automatic control logic.

---

## 📸 Preview

<img width="1920" height="848" alt="task3 elcter" src="https://github.com/user-attachments/assets/1754cbeb-4467-4835-b1c2-c1cb434a4d99" />

---



# part2
## Design and Programming of Digital and Analog Sensors

This project focuses on the design, interfacing, and programming of both digital and analog sensors using microcontroller platforms such as Arduino. It aims to demonstrate the fundamental differences between digital and analog signals and how to effectively read and process them in real-world applications.

---

## Importance of the Task

Understanding how to use digital and analog sensors is crucial in modern embedded systems, automation, robotics, and IoT applications. This task allows students and engineers to:

- Learn the difference between digital and analog signals.
- Gain hands-on experience in wiring and coding sensor modules.
- Interpret real-world physical parameters like temperature, light, and motion.
- Build foundational skills for larger embedded or automation projects.

---


## Applications

This project has many practical applications, including but not limited to:

- Smart home systems (e.g., temperature, motion, or light detection)
- Industrial automation and monitoring
- Environmental sensing (e.g., air quality, humidity)
- Robotics input and feedback systems
- Health and fitness monitoring devices


---


## Tools and Software Used

The following tools and platforms were used:

- Arduino IDE (for programming)
- Arduino Uno (microcontroller)
- Analog Sensors (e.g., potentiometer, LDR, temperature sensor)
- Digital Sensors (e.g., push button, IR sensor, motion sensor)
- Wires, breadboard, resistors, etc.

  ---
## 🔌 Circuit Diagram:



<img width="1536" height="640" alt="Fantabulous Juttuli-Albar (1)" src="https://github.com/user-attachments/assets/8c361bf6-7250-4c4d-b240-0ef8946f88b5" />




  ---
## Circuit Requirements

Typical connection:

- **Analog sensor**: VCC to 5V, GND to GND, Signal to A0
- **Digital sensor**: VCC to 5V, GND to GND, Signal to D2 or any digital pin

> Make sure to use pull-up or pull-down resistors when needed for digital sensors.



   ---


## Programming

The following Arduino sketch demonstrates how to read from an **analog LDR sensor** and a **digital ultrasonic sensor**, and trigger an LED based on specific conditions:

- If the **light level is low** (LDR < 500)
- And an **object is closer than 15 cm**

Then the **LED turns ON**, otherwise it remains OFF.

``` cpp
const int ledPin = 8;
const int ldrPin = A0;
const int trigPin = 7; // PING))) sensor
long duration, distance;
int ldrValue;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(trigPin, OUTPUT);
  digitalWrite(trigPin, LOW); // Ensure PING))) is off
  Serial.begin(9600);
}

void loop() {
  // Read from LDR
  ldrValue = analogRead(ldrPin);

  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigPin, LOW);

  // Receive echo and calculate distance
  duration = pulseIn(trigPin, HIGH);
  distance = duration / 58.2; // Convert duration to centimeters

  // Print sensor values
  Serial.print("LDR: ");
  Serial.print(ldrValue);
  Serial.print(" | Distance: ");
  Serial.println(distance);

  // If low light and object is close, turn LED on
  if (ldrValue < 500 && distance < 15) {
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(ledPin, LOW);
  }

  delay(300);
}
 ```

