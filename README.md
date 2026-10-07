# Object Avoidance Robot

Arduino sketch for a two-motor robot that uses an ultrasonic distance sensor to detect obstacles and steer around them.

## Requirements

- An Arduino-compatible board with the pins used below
- L298N motor driver and two DC motors
- Ultrasonic distance sensor supported by the NewPing library
- Servo motors
- Arduino `Servo` library and the `NewPing` library

## Pin assignments

| Component | Arduino pin |
| --- | --- |
| Left motor forward | 7 |
| Left motor backward | 6 |
| Right motor forward | 4 |
| Right motor backward | 5 |
| Ultrasonic trigger | A1 |
| Ultrasonic echo | A2 |
| Scanning servo | 10 |
| Auxiliary servo | 13 |

Check that the motor driver, servos, and board share an appropriate ground, and power motors and servos according to their specifications.

## Build and upload

1. Install the `NewPing` library using the Arduino IDE Library Manager.
2. Open `Avoid_bot.final.ino` in the Arduino IDE.
3. Select the board and port matching your hardware, then build and upload.

The sketch treats a zero distance reading as 250 cm and uses a 20 cm obstacle threshold.
