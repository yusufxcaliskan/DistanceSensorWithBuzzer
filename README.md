# Ultrasonic Distance Measurement with LCD Display

This project utilizes an ultrasonic sensor (HC-SR04) to measure the distance of an object and displays the result on an LCD screen using the LiquidCrystal library.

## Components Required
- Arduino Board
- Ultrasonic Sensor (HC-SR04)
- LCD (16x2)
- Resistors (if needed for contrast adjustment)
- Connecting Wires

## Code Explanation
The code uses the **LiquidCrystal** library to interface with a 16x2 LCD. It calculates the distance to an object based on the time taken for an ultrasonic pulse to reflect back to the sensor.

### Pin Configuration
- `trigPin` is connected to Pin 9.
- `echoPin` is connected to Pin 8.
- LCD pins are connected as follows:
  - RS → Pin 12
  - EN → Pin 11
  - D4 → Pin 5
  - D5 → Pin 4
  - D6 → Pin 3
  - D7 → Pin 2

### Key Variables
- `duration`: Time taken by the ultrasonic pulse to return.
- `distance`: Calculated distance based on the duration.

### Code Workflow
1. The trigger pin is set to LOW for 2 microseconds, then HIGH for 10 microseconds to send an ultrasonic pulse.
2. The `pulseIn()` function reads the time it takes for the echo to return.
3. The distance is calculated using the formula:  
   \[
   \text{distance} = \left(\frac{\text{duration}}{2}\right) \times 0.0344
   \]
4. If the distance is out of the sensor's range (less than 2 cm or greater than 400 cm), "Out of range" is displayed on the LCD.
5. Otherwise, the distance in centimeters is displayed.

### Code
```cpp
#include <LiquidCrystal.h>
#define trigPin 9
#define echoPin 8

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  float duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) * 0.0344;

  if (distance >= 400 || distance <= 2) {
    lcd.print("Out of range");
    delay(500);
  } else {
    lcd.print(distance);
    lcd.print(" cm");
    delay(500);
  }
  delay(500);
  lcd.clear();
}
