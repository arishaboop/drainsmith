# 🤖 Drainsmith: Sewer Cleaning Robot

> A WiFi-controlled ESP32 robot that enters sewer pipes, collects solid waste from the sewage with a conveyor, and uses a servo-mounted ultrasonic sensor to look for debris and obstacles ahead.

## 🎥 Project Demonstration
**YouTube video:** [https://youtube.com/shorts/10egf3e7s6U?si=kcGcTMeceQTyW7_x]

## 1. Project Description
Sewer pipes clog with solid waste such as plastic and debris, and cleaning them by hand means sending workers into manholes, where toxic gases and unsafe conditions are a real danger. Drainsmith is a remote-controlled robot that goes into the pipe in their place. The operator drives it from a phone, and a motor-driven conveyor lifts solid waste out of the sewage. An ultrasonic sensor mounted on a servo scans the area ahead for debris and obstacles, and a light sensor detects when the robot has entered the dark pipe.

### Features
- Web-based remote control from a phone or laptop (no app needed)
- Four DC motors: two drive the wheels, two run the waste-collecting conveyor
- PWM speed control for all four motors
- Servo that rotates the ultrasonic sensor through 0°–180° to look for debris
- Ultrasonic obstacle sensor with an automatic buzzer warning at 10 cm or closer
- Light sensor (LDR) that detects when the robot has entered the dark sewer
- Safety: the wheels stop automatically if the connection is lost, and a MASTER STOP button stops everything

## 2. Team Information
| Flying dutchman | team 8 |
|---|---|
| [Sk Adrit] | [e.g. Electronics, firmware, chassis, hands-on assembly] |
| [Arisha Siddiqua] | [e.g. CAD design, chassis, documentation, GitHub, hands-on assembly] |

Course: [bot engineering ]   Submission date: [20/11/26 ]

## 3. Individual Contributions

**[Sk Adrit]**
- Wired the main electronics: ESP32, motor drivers, motors, servo and buzzer
-  Programmed the ESP32 firmware: motor control and turning, PWM speed control, ultrasonic distance measurement, obstacle detection, light detection and servo control
- Worked on the chassis (building and editing) 
- Made YouTube video on the robot's functionality
- Set up the ESP32 as a WiFi access point (Robot_Control) with a web dashboard for remote control
- Tested the hardware and software, and fixed problems along the way
- Built the robot's mechanical assembly: wheels, caster wheel, four DC motors (two drive wheels, two conveyor), SG90 servo mechanism
- Provided technical details and firmware setup info for the GitHub documentation
**[Arisha Siddiqua]**
- Designed the complete CAD model of the robot
- Worked on the chassis (building and editing) with Adrit
- Set up the GitHub repository and wrote the full project documentation
- Built and wired the light sensor (LDR) circuit that detects when the robot enters the sewer
- Built the voltage divider (three 10 kΩ resistors) that protects the ESP32 from the ultrasonic sensor's 5 V echo signal
- Troubleshot the firmware upload failure: traced it to USB power leaking into the robot's power line through the VIN wires, and worked out a safe way to upload
- Tested the sensors and the web controls


## 4. How It Works
The ESP32 creates its own WiFi network (**Robot_Control**). The operator connects a phone or laptop, opens **http://192.168.4.1**, and controls the robot from a web page.

- **Wheels:** two DC motors drive the robot forward, backward, left and right.
- **Conveyor:** two DC motors lift solid waste out of the sewage (forward / reverse / stop).
- **Servo:** carries the ultrasonic sensor and rotates it from 0° to 180° so the robot can look for debris across a wide area. The angle is set from the web page.
- **Ultrasonic sensor (HC-SR04):** measures distance ahead; the buzzer beeps at 10 cm or less.
- **LDR:** dark = inside the sewer, light = outside.

## 5. Components
| Component | Qty | Purpose |
|---|---|---|
| ESP32 development board | 1 | Controller + WiFi |
| L298N motor driver | 2 | One for the wheels, one for the conveyor |
| TT gear DC motors  | 4 | 2 wheels, 2 conveyor |
| Servo [SG90] | 1 | Rotates the ultrasonic sensor to scan for debris |
| HC-SR04 ultrasonic sensor | 1 | Obstacle and debris detection |
| LDR + 10 kΩ resistor | 1 | Light detection |
| 10 kΩ resistors (3) | 3 | Voltage divider on the ultrasonic ECHO line (5 V → 3.3 V) |
| Buzzer | 1 | Warning sound |
| DC-DC step-down converter | 1 | Power the electronics |
| Battery [11.1 V] | 1 | Power |

## 6. Pin Map
| Function | ESP32 GPIO |
|---|---|
| Conveyor IN1 / IN2 / IN3 / IN4 | 15 / 25 / 4 / 16 |
| Left wheel IN1 / IN2 | 17 / 5 |
| Right wheel IN3 / IN4 | 18 / 19 |
| Servo | 21 |
| Buzzer | 14 |
| Ultrasonic TRIG / ECHO | 12 / 13 (ECHO through a voltage divider) |
| LDR | 34 |

## 7. Why We Chose These Motors (Justification)
**Wheel motors (yellow TT dc motors):** The robot weighs about [2.8 kg] and the wheel radius is [_3.25_ cm]. The torque needed per motor is roughly [_1.14 kg·cm_ kg·cm]. The motor's rated torque is [_2.5 kg·cm kg·cm], which is at least 2× what is needed, so it can move the robot through wet, slippery pipes. We chose these because they are inexpensive, widely available, operate at 3–6 V, and are easy to control in pairs using a standard L298N driver

**Conveyor motors (yellow TT dc motors):** They must lift wet waste of about [_300_g]. Required torque is about 0.45 kg·cm total (0.23 kg·cm per motor); the motor provides 2.5 kg·cm at 6 V, giving well over a 2× safety margin against sticky debris while keeping spare parts interchangeable with the wheel motors.

**Servo (SG90):** It only has to turn the light ultrasonic sensor, which needs about less than 0.05 kg·cm of torque. The servo's maximum (stall) torque is 1.8 kg·cm at 4.8 V, well above that requirement while keeping weight and power draw low.

*Rule of thumb used: choose an actuator with at least 2× the torque the task needs. Torque = force × distance.*

## 8. Repository Structure
```
drainsmith/
├── README.md
├── firmware/          <- ESP32 code
├── cad/               <- CAD design files (.step)
├── docs/              <- build process, actuator justification
└── images/            <- photos, wiring diagram, CAD screenshots
```

## 9. CAD Design
The complete CAD model of the robot was designed by [Arisha Siddiqua] in [Autodesk Fusion360]. The files are in the [`cad/`](cad/) folder as `.step` files.

<img width="1156" height="685" alt="image" src="https://github.com/user-attachments/assets/ad1803fe-0141-4af4-8a0e-806cd040a304" />


## 10. Setup Instructions

### Build the hardware
1. Build the chassis following the CAD files and wire the robot using the pin map above.
2. Put a voltage divider on the ultrasonic ECHO wire: ECHO → 10 kΩ → D13, and D13 → 10 kΩ + 10 kΩ → GND (the ESP32 pins are 3.3 V only).
3. Keep the jumper caps on the ENA and ENB pins of both L298N boards.

### Upload the firmware (PlatformIO)
1. Install **Visual Studio Code**, then install the **PlatformIO IDE** extension from the Extensions tab.
2. Open the project folder in VS Code (the folder containing `platformio.ini`). PlatformIO will automatically download the ESP32 platform and required libraries listed in `platformio.ini`, so no manual Boards Manager or Library Manager setup is needed.
3. Verify that `platformio.ini` is configured as follows:
   ```ini
   [env:esp32dev]
   platform = espressif32
   board = esp32dev
   framework = arduino
   monitor_speed = 115200
   upload_speed = 115200
   lib_deps =
       madhephaestus/ESP32Servo
   ```
4. Connect the ESP32 to your computer via USB.
5. Click the **Upload** button (right-arrow icon `→`) in the PlatformIO bottom toolbar, or run `pio run -t upload` in the terminal. If it gets stuck on *Connecting...*, hold down the **BOOT** button on the ESP32 until the upload begins.
6. To view debug logs, click the **Serial Monitor** button (plug icon) in the toolbar or run `pio device monitor`.

> **Upload tip:** If the upload fails with `"serial noise"` or packet errors, turn off the robot's main battery and unplug any wires connected to the ESP32 **VIN** pin (which can back-feed power from the motor drivers during USB flashing). Reconnect them after uploading. Keeping `upload_speed = 115200` in `platformio.ini` also helps prevent flashing issues.

### Run it
1. Power the robot. On your phone, join the WiFi network **Robot_Control**.
2. Open **http://192.168.4.1** in a browser and control the robot.
3. Test: cover the LDR (it should say "IN SEWER"), and put a hand about 10 cm in front of the ultrasonic sensor (the buzzer beeps).

## 11. Results
<img width="2480" height="3307" alt="image" src="https://github.com/user-attachments/assets/85a1bca9-982b-4e27-96ae-b097cdc8fa49" />


## 12. Possible Improvements
- Automatic mode: sweep the servo back and forth continuously and report where debris is detected
