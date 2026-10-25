# Smart Airbag Jacket
### ESP32 motion sensing, GPS location and GSM notification - LED bench reference

![Architecture](images/system_architecture.png)

**Status:** New reference implementation prepared for Karthik's portfolio. Host software tests are included. Physical hardware, SMS delivery and protective performance have not been verified in this package. This is not a recovered copy of the original BE project firmware.

The original project description in Karthik's resume names an ESP32, MPU6050, SIM800L, NEO-6M and a CO2 airbag mechanism. This repository provides a reproducible **electronic bench demonstration** around those sensor and communication modules. It has **no inflation driver, valve-control output or mechanical deployment instructions**. GPIO25 is an LED indicator only.

## Features implemented

- MPU6050 register-level I2C acquisition: +/-8 g, +/-500 degrees/second; target polling interval 10 ms.
- Startup stationary gyro-bias calibration and explicit manual arming.
- Illustrative combined acceleration/rotation threshold with persistence, gap rejection and event latch.
- Checksum-validated GPRMC/GNRMC parsing and GPS fix-age checking.
- Optional nonblocking SMS state machine: SIM/network checks, prompt gating, timeouts and modem submission acknowledgement.
- SMS disabled by default; dry-run messages printed on USB serial.
- Native C++ tests, synthetic CSV replay, diagrams and a technical report.

## Start here

1. Read `docs/GETTING_STARTED.md`.
2. Connect **only ESP32 + MPU6050 + resistor + LED** for the initial bench test.
3. Open this folder (the one containing `platformio.ini`) in VS Code with PlatformIO.
4. Build and upload; open serial monitor at **115200**.
5. Keep the sensor stationary during startup calibration.
6. Type `s` for status, `t` for a manual LED demonstration, `r` to reset/disarm.
7. Type `a` only when ready for an LED-only sensor bench test. Do not strike or drop a person or the assembly to create an event; use synthetic replay for detector testing.

`a` arms only after successful calibration. `t` is a manual demonstration and works even without the IMU; it is not proof of sensor detection. After an I2C fault, fix the connection and reboot. Reset does not recalibrate. Never connect GPIO25 to an inflation mechanism.

## Build and upload

```bash
python -m pip install platformio==6.1.18
pio run
pio run --target upload
pio device monitor --baud 115200
```

PlatformIO downloads the pinned ESP32 platform on the first build. Confirm your board is a classic ESP32 DevKit-compatible board; the supplied pin map is not for ESP32-C3/S3 or boards with PSRAM on GPIO16/17.

## Run software checks without hardware

Requires Python 3 and a C++17 compiler named `g++` in PATH.

```bash
python tests/run_tests.py
```

The included run records **36 native checks passed** and one event at **230 ms in synthetic data**. This is an algorithm-fixture timestamp, not measured crash-detection latency. Both ESP32 build environments (SMS off/on) also compiled successfully. See `reports/software_test_log.txt`, `reports/esp32_build_log.txt` and `reports/VALIDATION.md` for verification scope.

## Bench connections

| ESP32 pin | Bench connection |
|---|---|
| 3V3 | MPU6050 breakout VCC, only if that exact breakout supports 3.3 V |
| GND | MPU6050 GND and LED cathode |
| GPIO21 | MPU6050 SDA |
| GPIO22 | MPU6050 SCL |
| GPIO25 | 330-ohm resistor -> LED anode |
| GPIO16 RX | Optional GPS TX (3.3 V compatible logic) |
| GPIO17 TX | Optional GPS RX, normally unnecessary |
| GPIO26 RX | Optional GSM TX through appropriate voltage translation |
| GPIO27 TX | Optional GSM RX through appropriate voltage translation |

See `hardware/WIRING.md` before adding optional modules. GSM requires a separate correctly specified power supply and level compatibility review. Do not power a SIM800L from ESP32 3V3. Module variants differ; a universal ready-to-fabricate schematic is not supplied.

## Detection rule

`A = sqrt(ax^2 + ay^2 + az^2)` in g, including gravity.

`G = sqrt(gx^2 + gy^2 + gz^2)` in degrees/second, after startup gyro-bias subtraction.

An event occurs only if `A >= 3.0 g` **and** `G >= 180 dps` persist for at least 30 ms, with no observed sample gap above 30 ms. These are arbitrary demonstrator settings, **not validated crash criteria**. No orientation estimation, Kalman filter, machine learning or RTOS tasks are implemented. The firmware is a cooperative Arduino loop.

## Optional GPS / SMS

GPS parsing runs whenever bytes arrive. Fixes older than 10 seconds are not placed in messages. A checksum-valid RMC sentence with void status invalidates the fix.

For SMS, copy `include/local_config.example.h` to `include/local_config.h`, enter a consenting test recipient in international format, set `ENABLE_SMS` to `1`, rebuild and upload. Keep that local file private. Every message starts `BENCH TEST`.

The modem must already be powered on, have its UART configured to 9600 baud, and use a working SIM with supported 2G service. Firmware does not operate PWRKEY. It makes one attempt per latched event with no automatic retry. A `+CMGS` response plus `OK` indicates modem submission, not recipient delivery. Network coverage and SMS charges depend on your provider.

## Repository contents

| Path | Purpose |
|---|---|
| `src/main.cpp` | ESP32 application and MPU6050 register driver |
| `include/` | Pins and example configuration |
| `lib/BenchCore/` | Portable detector, GPS parser and SMS state machine |
| `tests/` | Host checks and CSV replay |
| `data/` | Clearly labeled synthetic motion fixture |
| `hardware/` | Wiring, bill of materials and integration boundaries |
| `images/` | Editable SVG + PNG diagrams; not photographs |
| `photographs/README.md` | Required real-photo shot list; no fabricated photos |
| `docs/` | Setup, algorithm, GitHub publishing and interview guide |
| `reports/` | Technical report, software log and hardware result template |
| `.github/workflows/ci.yml` | Native test and ESP32 build jobs |

## Photographs and results

Real prototype photographs were not provided. Add your own following `photographs/README.md`. Do not describe the garment illustration as a photograph or a measured prototype. Hardware result fields intentionally remain `NOT MEASURED`.

## Authorship and attribution

Prepared for **Karthik Yadav P C**. This is newly generated reference code and documentation; Karthik's original BE implementation and team contributions must be documented separately. Review, understand and validate the code before presenting it as your work. Do not claim unperformed tests, independent authorship of team work, or measured protective performance.

No open-source license is selected automatically. See `LICENSE-NOTE.md`. References are listed in `docs/REFERENCES.md`.
