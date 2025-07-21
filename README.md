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

