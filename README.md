
# HODOR - Highly flexible Open-source Door Operation Regulator

## Overview

HODOR ("Holds your door") is a universal controller for sliding door motor systems. The device is designed to operate brushed DC motors ranging from 12 to 24V, providing a comprehensive control solution that can be configured via Wi-Fi for smart home integration.

The hardware design files were created in KiCAD 9.0.

## System Architecture

The HODOR system consists of four main subsystems:

1. **Controllerboard** - Central processing unit based on the ESP32-S3
2. **Power Supply** - Handles voltage regulation and power distribution
3. **Motor Control** - Manages motor drive with H-bridge driver and feedback systems
4. **Peripherals** - Provides additional I/O and integration capabilities

### Block Diagram

```
+---------------+    +----------------+
|    Power      |    |                |
|   Terminals   |----| Controllerboard|
|   12-24V      |    |   (ESP32-S3)   |<--->[SPI BUS]
+---------------+    |                |
                     |                |----+
                     +----------------+    |
                            ^              |
                            |              v
                            |        +----------------+
                            |        |  Peripherals   |
                            |        |  I/O Expansion |
                            |        +----------------+
                            v
                     +----------------+
                     |  Motor Control |
                     |   H-Bridge     |
                     |   Encoder I/F  |
                     +----------------+
```

## Technical Specifications

| Input Voltage | Standby Current |
|---------------|-----------------|
| 12 V          | tbc             |
| 24 V          | tbc             |

## Subsystems

### Controller Board (ESP32-S3)

The central controller is based on an ESP32-S3 microcontroller that handles:
- Motor control with PWM outputs
- Safety monitoring
- Encoder feedback processing
- SPI communication with peripherals
- Wi-Fi connectivity for configuration and smart home integration

The controller connects to both the motor control subsystem and peripheral I/O expansion modules through a combination of digital signals and an SPI bus.

### Power Supply

The power subsystem converts the 12-24V input to the required voltages for the control circuits and provides:
- Input voltage sensing (`V_BUS_SENSE`)
- DC-link voltage monitoring (`V_DC_SENSE`)
- Protection circuitry

### Motor Control

The motor control subsystem features:
- H-bridge driver with PWM inputs (`DRV_PWM1`, `DRV_PWM2`)
- Driver enable control (`DRV_EN`)
- Fault monitoring (`DRV_~SF`)
- Safety trigger and clear mechanisms (`SAFETY_TRIG`, `SAFETY_~CLR`)
- Current sensing (`ADC_I_SENSE`, `ADC_DRV_FB`)
- Quadrature encoder interface (`ENCODER_A`, `ENCODER_B`)
- An independet brake chopper, that burns voltage peaks above 27 V, to protect the driver IC.

The H-bridge allows bidirectional control of the DC motor with variable speed control via PWM.

### Peripherals

The peripheral subsystem provides additional inputs and outputs for:
- Inputs (Potential free or 12..24 V DC)
- Outputs (Switched supply voltage)
- Status indicators
- Relay outputs

It communicates with the main controller via SPI and provides interrupts for event notifications (`IO_INT`).

## Key Features

- Supports brushed DC motors from 12 to 24V
- H-bridge driver with current sensing for motor protection
- Quadrature encoder interface for precise position feedback
- Configurable inputs and outputs
- Programmable relay outputs
- Wi-Fi connectivity for configuration and smart home integration
- Safety mechanisms for door operation
- Open-source hardware and software design

## Applications

The HODOR controller is primarily designed for:
- Sliding door automation
- Other motor control applications with optional position feedback

## Project Status

This project is currently in development.

## Safety Features

HODOR incorporates several safety features:
- Current monitoring to detect obstructions
- Position feedback via quadrature encoder
- Hardware watchdog safety trigger 
- Fault detection in the motor driver

