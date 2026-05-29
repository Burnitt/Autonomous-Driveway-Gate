# Autonomous Driveway Gate

An Arduino-based proximity-activated gate system that detects 
approaching objects using an ultrasonic sensor and automatically 
opens via a servo motor sweep. Built to replicate the real-world 
function of an automated driveway gate without any manual input.

**Timeline:** May 2024  
**Platform:** Arduino Uno | Arduino C++  
**Type:** Solo project

---

## How It Works

The system continuously reads distance from an ultrasonic sensor. 
When an object is detected within 5 cm, the servo sweeps from 0° 
to 90° (gate opens), holds briefly, then returns to 0° (gate closes).

| Condition | Response |
|---|---|
| Object within 5 cm | Servo sweeps 0° → 90° (open) |
| Object cleared | Servo returns 90° → 0° (close) |

Rather than a hard `servo.write(90)` snap, a stepped for-loop 
increments position by 2° every 15 ms, producing smooth and 
realistic gate motion.

---

## Hardware

- Arduino Uno
- Parallax PING ultrasonic sensor
- Servo motor (`servo.attach(pin, 500, 2500)`)
- Breadboard + wiring

**Circuit diagram:** [TinkerCAD](https://www.tinkercad.com/things/frosQddG6vk/editel?sharecode=wQ1teoypBejiX6jHJvJXi9Mn2uGAWApf8aHuyRncw8c)

---

## Key Function

```cpp
long readUltrasonicDistance(int triggerPin, int echoPin) {
  pinMode(triggerPin, OUTPUT);
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  return pulseIn(echoPin, HIGH);
}
```

Abstracts all sensor timing logic into a reusable function, keeping 
the main loop clean and readable.

---

## Demo

![Circuit wiring](docs/circuit%20wiring.webp)
![TinkerCAD circuit diagram](docs/circuit%20diagram.webp)

> Demo video available on my [portfolio](https://bush-aurora-6f7.notion.site/Autonomous-Driveway-Gate-3303afb3d1d580e78652dfbd756dbc8b)

---

## Key Challenge

A hard `servo.write(90)` produced an instant mechanical snap rather 
than smooth gate motion. This was resolved by implementing a stepped 
for-loop incrementing 2° every 15 ms. The 5 cm proximity threshold 
also required physical tuning — too large caused false triggers, too 
small missed detections reliably.

---

## Future Improvements

- Add a second ultrasonic sensor on the exit side to prevent the 
  gate from closing on a vehicle mid-exit
- Implement adjustable trigger sensitivity via potentiometer, 
  eliminating the need to modify code for threshold changes
- Add a manual button override as a fallback if the sensor 
  malfunctions
