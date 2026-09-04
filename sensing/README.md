# Sensing

Reads raw sensor data from the drone platform:

- GPS location
- IMU (accelerometer + gyroscope, and magnetometer if available)
- Altitude (e.g. barometer and/or rangefinder)
- Camera images, timestamped for later fusion/mapping use

## Purpose

Provide clean, timestamped sensor readings to `fusion/`. This module does not
estimate position or orientation itself — it only reads and packages raw
sensor data.

## Planned interface (subject to change once the autopilot stack is decided —
see [../docs/open-decisions.md](../docs/open-decisions.md))

- Output: a timestamped stream/log of `{gps, imu, altitude}` samples and
  `{image, timestamp}` captures, in a format `fusion/` and `mapping/` can
  consume independently of what hardware produced them.

## Open questions

- Exact sensor hardware (GPS module, IMU, barometer/rangefinder) depends on
  the flight controller / autopilot stack decision.
- Camera capture format and timestamp sync method depend on the camera +
  gimbal hardware decision.
