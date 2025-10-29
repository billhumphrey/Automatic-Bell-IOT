
# Automatic College Bell System Using Arduino

This project automates the traditional college bell system using an **Arduino**. It allows you to configure the number of class periods and their durations through a **16x2 I2C LCD** and **push buttons**. At the end of each period, a **buzzer** automatically rings — offering an efficient and reliable solution for managing school or college schedules.


## Components Required

| Component     | Quantity | Description                     |
| ------------- | -------- | ------------------------------- |
| Arduino Board | 1        | Main microcontroller            |
| 16x2 I2C LCD  | 1        | Display for settings and timing |
| Push Buttons  | 3        | For UP, DOWN, and SET controls  |
| Buzzer        | 1        | Audio alert for bell            |
| Breadboard    | 1        | For connections                 |
| Jumper Wires  | —        | For wiring components           |

---

## ⚡ Circuit Connections

### **I2C LCD**

| LCD Pin | Arduino Pin |
| ------- | ----------- |
| VCC     | 5V          |
| GND     | GND         |
| SDA     | A4          |
| SCL     | A5          |

### **Buttons**

| Button | Arduino Pin |
| ------ | ----------- |
| UP     | D6          |
| DOWN   | D5          |
| SET    | D4          |

### **Buzzer**

| Component | Arduino Pin |
| --------- | ----------- |
| Buzzer    | D3          |

---

##  Features

* **Set Number of Periods:** Use UP/DOWN buttons to configure total periods.
* **Custom Duration:** Assign different durations for each period.
* **Automatic Bell:** The buzzer rings at the end of every period.
* **User-Friendly Interface:** Simple LCD and button-based navigation.

---

##  Usage Instructions

1. **Hardware Setup**

   * Connect all components as per the circuit diagram above.
   * Ensure proper power connections.

2. **Upload Code**

   * Open the provided Arduino code in the Arduino IDE.
   * Select your board and COM port, then upload the sketch.

3. **Set Number of Periods**

   * Press **UP/DOWN** to adjust.
   * Press **SET** to confirm.

4. **Set Period Durations**

   * Use **UP/DOWN** to set duration (in seconds or minutes).
   * Press **SET** to confirm each period.

5. **Start the Schedule**

   * The system automatically tracks and rings the bell at the end of each period.

---

##  Code Overview

### **LCD Initialization**

```cpp
LiquidCrystal_I2C lcd(0x27, 16, 2); // Adjust address if needed
```

### **Pin Configuration**

```cpp
const int UP_BUTTON = 6;
const int DOWN_BUTTON = 5;
const int SET_BUTTON = 4;
const int BUZZER_PIN = 3;
```

### **Initialize Pins**

```cpp
void initializePins() {
  pinMode(UP_BUTTON, INPUT);
  pinMode(DOWN_BUTTON, INPUT);
  pinMode(SET_BUTTON, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(UP_BUTTON, HIGH);
  digitalWrite(DOWN_BUTTON, HIGH);
  digitalWrite(SET_BUTTON, HIGH);
}
```

### **Set Number of Periods**

```cpp
void setNumberOfPeriods() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Set Periods:");

  while (true) {
    lcd.setCursor(0, 1);
    lcd.print(numPeriods);
    if (digitalRead(UP_BUTTON) == LOW) {
      delay(300);
      numPeriods++;
    } else if (digitalRead(DOWN_BUTTON) == LOW) {
      delay(300);
      numPeriods--;
    } else if (digitalRead(SET_BUTTON) == LOW) {
      delay(300);
      break;
    }
  }
}
```

### **Start the Periods**

```cpp
void startPeriods() {
  for (currentPeriod = 1; currentPeriod <= numPeriods; currentPeriod++) {
    lcd.clear();
    lcd.setCursor(1, 0);
    lcd.print("Period:");
    lcd.setCursor(10, 0);
    lcd.print(currentPeriod);

    for (int elapsed = 0; elapsed <= periodTimes[currentPeriod]; elapsed++) {
      lcd.setCursor(0, 1);
      lcd.print(elapsed);
      delay(1000);
      if (elapsed == periodTimes[currentPeriod]) {
        tone(BUZZER_PIN, 200);  // Activate buzzer
        delay(3000);
        noTone(BUZZER_PIN);     // Deactivate buzzer
      }
    }
    lcd.clear();
  }
}
```



##  Future Enhancements

* Add **Real-Time Clock (RTC)** module for accurate time-based scheduling.
* Include **EEPROM** storage for saving configurations.
* Integrate a **Relay Module** to ring a real electric bell.


