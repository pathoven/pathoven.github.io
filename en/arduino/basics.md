# Arduino Basics

The Arduino Nano Every drives the BTS7960 motor driver and the steering servo. A minimal firmware is enough to get started.

## Flashing the Firmware

```bash
git clone https://github.com/pathoven/pathoven_firmware.git
cd pathoven_firmware
arduino-cli compile --fqbn arduino:megaavr:nona4809 .
arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:megaavr:nona4809 .
```

## Serial Protocol

| Field | Description | Example |
| --- | --- | --- |
| `CMD` | Mode (`MANUAL`, `AUTO`, `STOP`) | `CMD:MANUAL` |
| `STR` | Steering PWM (1000-2000 µs) | `STR:1500` |
| `THR` | Throttle PWM | `THR:1520` |
| `HB` | Heartbeat counter | `HB:1234` |

Publish commands from ROS2 by writing the formatted line to `/dev/ttyUSB0`. Each packet ends with `\n`.

## Safety Features

- Servo limits are clamped to 1100–1900 µs.
- A watchdog drops throttle to zero if no command arrives within 200 ms.
- Manual override switch on the receiver can bypass Arduino output.

## Next Steps

Once firmware is running, go back to the Raspberry Pi and continue with the [setup guide](/en/raspberrypi/setup).
