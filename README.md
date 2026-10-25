# Smart Airbag Jacket

An ESP32-based wearable safety research prototype that monitors motion,
detects potential impact events and supports emergency notification
through GSM and GPS modules.

## Project Status

Academic prototype.

This repository documents the project architecture, hardware,
firmware and available test evidence. It is not certified protective
equipment and must not be relied on for personal safety.

## Overview

The Smart Airbag Jacket combines motion sensing, embedded processing
and emergency communication in a wearable system.

An MPU6050 measures acceleration and angular velocity. An ESP32
processes the sensor data using a threshold-based detection algorithm.
When a potential impact event is detected, the controller can issue
a deployment request and initiate an emergency SMS containing the
available GPS coordinates.

The physical prototype includes a CO2-based airbag deployment mechanism.
Its mechanical design and measured performance must be documented
separately from the electronic detection system.

## Objectives

- Monitor motion using an accelerometer and gyroscope.
- Detect potential impact events using embedded firmware.
- Interface the controller with an airbag deployment subsystem.
- Retrieve location information through GPS.
- Send an emergency SMS through a GSM module.
- Evaluate detection behavior, communication and response timing.

## Hardware

| Component | Purpose |
|-----------|---------|
| ESP32 | Main controller and sensor-data processing |
| MPU6050 | Three-axis acceleration and angular-velocity measurement |
| SIM800L | GSM communication and SMS transmission |
| NEO-6M | GPS position data |
| Deployment interface | Connects the controller to the deployment subsystem |
| CO2-based inflation assembly | Physical airbag inflation mechanism |
| Power supply | Supplies the controller, sensors and communication modules |
| Jacket and airbag assembly | Wearable mechanical structure |

Exact board variants, power specifications and deployment-interface
components: [add verified details].

## Software and Tools

- Firmware language: Embedded C/C++ [confirm]
- Development environment: [add actual IDE and framework]
- Simulation: Proteus [describe what was simulated]
- Version control: Git and GitHub
- Additional tools: [add only tools actually used]

## System Architecture

```mermaid
flowchart TD
    A["MPU6050 motion sensor"] --> B["ESP32 controller"]
    B --> C["Impact detection logic"]
    C --> D["Deployment interface"]
    D --> E["Airbag assembly"]
    C --> F["Emergency notification"]
    G["NEO-6M GPS"] --> F
    F --> H["SIM800L GSM"]
    H --> I["Emergency contact"]
```

The diagram represents functional relationships.
It is not an electrical wiring diagram.

## Communication Interfaces

| Connection | Interface |
|------------|-----------|
| MPU6050 to ESP32 | I2C |
| NEO-6M to ESP32 | UART |
| SIM800L to ESP32 | UART |
| ESP32 to deployment interface | Implementation-specific control output |

GPIO assignments, baud rates and supply connections are documented
in `hardware/pin_connections.md`.

## Working Principle

1. Initialize the controller and connected modules.
2. Check sensor communication and perform calibration.
3. Read acceleration and angular-velocity measurements.
4. Process the readings using the implemented detection algorithm.
5. Identify whether the configured event conditions are satisfied.
6. Record the event and issue the appropriate control request.
7. Retrieve the available GPS position.
8. Attempt to send an emergency SMS.
9. Record communication outcomes and system faults.

Actual ordering, fault handling and timing must match the firmware.

## Detection Algorithm

The prototype uses threshold-based impact detection.

Document the following from the actual implementation:

- Sensor sampling frequency: [value]
- Accelerometer measurement range: [value]
- Gyroscope measurement range: [value]
- Calibration method: [method]
- Filtering method, if used: [method]
- Detection thresholds and units: [values]
- Time-window or persistence checks: [details]
- Event-latching and reset behavior: [details]

Do not treat arbitrary example thresholds as validated crash criteria.

## Firmware

Firmware source code is stored in `firmware/`.

The firmware documentation must explain:

- Controller initialization
- Sensor configuration and calibration
- Motion-data acquisition
- Detection logic
- Deployment-interface behavior
- GPS data parsing
- GSM SMS handling
- Fault handling and event logging

## Setup and Reproduction

1. Identify the exact ESP32 board and hardware revisions.
2. Review the schematic and verified pin connections.
3. Install the development environment listed below.
4. Install the required libraries at the recorded versions.
5. Configure local settings using the supplied example configuration.
6. Build and upload the firmware using the documented board settings.
7. Run the sensor and communication checks.
8. Perform detection tests using a harmless indicator or dummy load.

### Build Environment

- IDE/framework: [name and version]
- ESP32 board package: [version]
- Target board: [board]
- Required libraries: [names and versions]
- Build/upload commands or IDE steps: [instructions]
- Serial-monitor baud rate: [value]

Real contact numbers and credentials must remain outside the public
repository.

## Testing

Begin electronic verification with the physical deployment mechanism
disconnected and use an indicator or dummy load.

| Test | Expected behavior | Observed result |
|------|-------------------|-----------------|
| Sensor initialization | Sensor is detected and readings are available | Not recorded |
| Stationary calibration | Bias and stationary readings are documented | Not recorded |
| Normal motion | No unintended deployment request | Not recorded |
| Simulated event input | Detection follows the configured logic | Not recorded |
| Sensor disconnection | Defined fault behavior occurs | Not recorded |
| GPS unavailable | Unavailable location is reported explicitly | Not recorded |
| GSM unavailable | Failed SMS attempt is reported | Not recorded |
| Emergency SMS | Configured contact receives the expected message | Not recorded |
| Event repetition | Repeated events follow documented latching behavior | Not recorded |
| Power restart | Startup behavior matches the specification | Not recorded |

Replace "Not recorded" with evidence from completed tests.

## Results

Add measured results with the test conditions:

| Metric | Measurement |
|--------|-------------|
| Sensor sampling frequency | [value and units] |
| Detection latency | [value and measurement method] |
| Output-command latency | [value and measurement method] |
| Physical inflation time | [value, or not measured] |
| GPS availability/fix time | [conditions and result] |
| SMS delivery time | [network conditions and result] |
| False triggers | [count / number of normal-motion tests] |
| Missed detections | [count / number of defined event tests] |

Electronic response time, inflation time and SMS delivery time are
separate measurements.

## Limitations

- Threshold-based detection can produce false triggers or miss events.
- GPS reception depends on the environment.
- GSM communication depends on network and SIM availability.
- Power interruptions can affect sensing and communication.
- Electronic detection alone does not establish protective performance.
- Controlled prototype tests do not establish real-world crash safety.




Examples to adapt only if completed:

- Integrated the motion sensor with the ESP32.
- Implemented threshold-based detection firmware.
- Integrated GPS parsing and GSM SMS communication.
- Designed or assembled the electronic prototype.
- Conducted tests and documented observations.

## Future Improvements

- Evaluate detection using labeled motion datasets.
- Improve false-trigger rejection.
- Add structured event logging.
- Improve sensor and communication fault diagnostics.
- Investigate power consumption and battery monitoring.
- Evaluate mechanical and wearable-system performance through
  appropriate professional testing.



