# RC Plane

## Project Overview

This project implements a custom remote control system for an RC plane, featuring direct Wi-Fi communication between an ESP32 transmitter and an ESP8266 receiver. The system is optimized for low latency control with specialized features designed for a payload delivery mission.

## Mission Requirements

- Carry and deploy a 50-gram golf ball payload to a designated target area
- Maintain stable flight during all phases of operation
- Provide precise control for positioning and payload deployment
- Ensure failsafe operation in case of signal loss

## Hardware Components

### Transmitter
- ESP32 microcontroller
- 2 dual-axis joysticks (connected to GPIO 32, 33, 34, 35)
- 2 push buttons (connected to GPIO 25, 26)
- Power supply

### Receiver/Plane
- ESP8266 microcontroller
- Servos for control surfaces:
  - Elevator (D1)
  - Rudder (D0)
  - Left wing (D3)
  - Right wing (D4)
- BLDC motor with ESC (D5)
- Frame and mechanical components
- Golf ball release mechanism
- 12V 2200mAh LiPo battery
- 10-inch propeller
- 1000KV BLDC motor

## Control Layout

### Left Joystick
- Y-axis: BLDC motor speed (throttle)
- X-axis: Rudder control (70° to 130°)

### Right Joystick
- Y-axis: Elevator control (80° to 150°)
- X-axis: Wing control (left wing: 30° to 110°, right wing: 20° to 100°)

### Buttons
- Right button: Synchronized mode (links elevator and wings for easier maneuvers)
- Left button: Golf ball drop mechanism

## Special Features

### ESC Calibration
Move both joysticks to extreme upper-right position to initiate ESC calibration.

### Synchronized Control Mode
Hold the right button to enable synchronized mode, which coordinates wing and elevator movement for smoother flight.

### Failsafe Operation
If signal is lost for more than 1 second, all controls return to neutral positions, and motor power gradually decreases.

### Signal Processing
- Input smoothing reduces jitter
- Dead zone handling for improved stability
- Gradual BLDC speed changes for safety

## Communication Specifications

- Direct Wi-Fi connection between transmitter and receiver
- The ESP32 acts as an access point, and the ESP8266 connects as a client
- 50Hz update rate (20ms per update)
- UDP protocol for minimal latency
- Compact data packet structure

## Setup Instructions

1. Upload the transmitter code to the ESP32
2. Upload the receiver code to the ESP8266
3. Power on the transmitter
4. Power on the receiver (it will automatically connect to the transmitter)
5. For first-time use, perform ESC calibration

## Calibration Procedure

1. Turn on the transmitter
2. Move both joysticks to the extreme upper-right position
3. Power on the receiver while holding the joysticks in this position
4. The ESC will receive maximum throttle signal for 2 seconds
5. The ESC will then receive minimum throttle signal to complete calibration
6. Return joysticks to neutral position to resume normal operation

## Control Range

| Control | Min | Mean | Max |
|---------|-----|------|-----|
| Elevator | 80° | 80° | 150° |
| Rudder | 70° | 105° | 130° |
| Left Wing | 30° | 70° | 110° |
| Right Wing | 20° | 68° | 100° |

## Payload Deployment

The golf ball release mechanism is triggered by the left button. Upon activation, the servo controlling the release mechanism will move to drop the golf ball. Position the aircraft carefully before releasing the payload to ensure accurate delivery.

## Development Notes

- The system is optimized for low latency with a direct Wi-Fi connection
- Signal filtering helps stabilize control inputs
- Comprehensive serial monitoring is available on the transmitter
- The system includes safety features to prevent accidental motor activation

## Troubleshooting

- If controls appear reversed, check pin assignments and mapping in the code
- If the servo range is incorrect, adjust the mapping values in the transmitter code
- For connection issues, ensure both devices are powered and within range
- If motor doesn't respond, verify ESC calibration and connections

## Future Improvements

- Battery voltage monitoring
- Flight data logging
- GPS position holding
- Automatic return-to-home functionality
- Flight stabilization system
