# Required Hardware

This page lists all hardware components needed to build the Pathoven Drive project.

## 🎯 Essential Components

### 1. Raspberry Pi 5

**Why Raspberry Pi 5?**
- Powerful ARM Cortex-A76 processor (2.4GHz quad-core)
- Sufficient performance for ROS2 Jazzy
- USB 3.0 ports (for LiDAR and Arduino)
- GPIO pins (for future expansions)
- Active cooling support

**Specifications:**
- Model: Raspberry Pi 5
- RAM: 4GB minimum (8GB recommended)
- Storage: 32GB+ microSD card (Class 10 or UHS-I)
- Power: USB-C PD, 5V 3A minimum (5A recommended)

**Estimated Cost:** ~$60-80 (depending on RAM)

**Alternatives:**
- ✅ Raspberry Pi 4 (8GB) - Slightly slower but works
- ⚠️ Raspberry Pi 4 (4GB) - Limited performance
- ❌ Raspberry Pi 3 - Insufficient (ROS2 too heavy)

### 2. Arduino Uno / Nano

**Role:** Motor control, encoder reading, low-level I/O

**Specifications:**
- CPU: ATmega328P
- Clock: 16MHz
- GPIO: 14 digital, 6 analog pins
- Communication: USB Serial (115200 baud)

**Estimated Cost:** ~$5-25

**Alternatives:**
- ✅ Arduino Nano - Smaller, same functionality
- ✅ Arduino Mega - More pins (for future expansions)
- ⚠️ ESP32 - Requires different code but has WiFi advantage

### 3. RPLidar A1

**Role:** 360° laser scanning, environment sensing for SLAM

**Specifications:**
- Scanning range: 0.15m - 12m (on white surfaces)
- Angular resolution: ~0.5° (720 points/scan)
- Scan rate: 5.5Hz
- Interface: USB Serial
- Size: 98mm diameter x 39mm height
- Weight: ~190g
- Power: 5V, ~400mA

**Estimated Cost:** ~$100-120

**Note:** RPLidar A2/A3 are more advanced but A1 is sufficient for this project.

**Alternatives:**
- ⚠️ YDLIDAR X4 (~$90) - Similar performance
- ❌ Ultrasonic sensors - Insufficient for SLAM
- ❌ Camera (depth sensing) - Too much computational load

### 4. RC Car Chassis

**Requirements:**
- Steering type: **Ackermann steering** (car-like)
- Wheelbase: ~15cm (10-20cm range)
- Motor: DC motor (preferably with encoder)
- Servo motor: Standard size (SG90 or similar)
- Chassis size: Able to carry Raspberry Pi and LiDAR

**Recommended Features:**
- Metal chassis (plastic works but less durable)
- Adjustable suspension
- Wide mounting area
- Battery compartment

**Estimated Cost:** ~$30-60

**Examples:**
- 1/18 scale RC car chassis
- DIY robot car kit (4WD or Ackermann)

### 5. Motor Driver

**Role:** Amplifies PWM signals from Arduino to drive motors

**Recommended:** L298N Motor Driver or similar
- Dual H-bridge
- 5-35V input voltage
- Max 2A per channel
- Enable pins (for PWM control)

**Estimated Cost:** ~$3-5

**Alternatives:**
- L293D (lower current, <1A)
- TB6612FNG (more efficient, recommended)
- DRV8833 (for low voltage)

## 📦 Other Required Materials

### Cables & Connectors
- USB-A to USB-B cable (for Arduino) - ~$3
- USB-A to Micro-USB cable (for RPLidar) - ~$3
- Dupont jumper wires (M-M, M-F, F-F) - ~$5
- Power cables (silicone 22-24 AWG) - ~$5

### Power Supply
- **For Raspberry Pi:** 5V 5A USB-C adapter - ~$10
- **For Motor and Servo:** 7.4V LiPo battery (2S, 2000-3000mAh) - ~$15-25
- **Alternative:** 4x AA battery holder (6V) - ~$5

**⚠️ Important:** Raspberry Pi and motor system should be powered from **separate** sources!

### Mounting Materials
- Standoffs (M2.5, M3 sizes) - ~$5
- Double-sided tape / velcro - ~$3
- Plastic or metal brackets - ~$5
- Cable ties (zip ties) - ~$2

### Optional Cooling (Recommended)
- Active cooler for Raspberry Pi 5 (fan) - ~$5-10
- Heatsinks - ~$3

## 🔌 Electrical Connection Diagram

```
┌──────────────┐
│  Raspberry   │  USB ──────────► Arduino Uno
│    Pi 5      │  USB ──────────► RPLidar A1
└──────┬───────┘
       │ 5V 5A USB-C
       │
    Power Supply
```

```
┌──────────────┐
│  Arduino     │  PWM ──────────► Motor Driver ──► DC Motor
│    Uno       │  PWM ──────────► Servo Motor (Steering)
└──────┬───────┘  Digital ◄───── Encoder (Motor)
       │
    USB Power (from RPi)
```

```
┌──────────────┐
│ Motor Driver │◄── 7.4V LiPo Battery
│   (L298N)    │
└──────┬───────┘
       │
       ├─────────► DC Motor (+/-)
       └─────────► Servo 5V/GND (if driver has it)
```

## 💰 Total Cost Estimate

| Component | Price Range |
|-----------|-------------|
| Raspberry Pi 5 (8GB) | $80 |
| microSD Card (64GB) | $10 |
| Raspberry Pi Power Adapter | $10 |
| Arduino Uno | $25 |
| RPLidar A1 | $110 |
| RC Car Chassis | $50 |
| Motor Driver (L298N) | $5 |
| Servo Motor | $5 |
| DC Motor + Encoder | $10 |
| LiPo Battery (2S 2200mAh) | $20 |
| Cables & Connectors | $20 |
| Mounting Materials | $10 |
| **TOTAL** | **~$355** |

**💡 Cost-Saving Tips:**
- Use Raspberry Pi 4 (4GB) (-$20)
- Second-hand RC car chassis (-$20)
- Use Arduino Nano (-$15)
- Utilize existing materials (cables, screws, etc.)

**Minimum budget:** ~$250-280

## 🛒 Where to Buy?

### Online Stores (International)
- [AliExpress](https://www.aliexpress.com) - Budget-friendly (long shipping)
- [Amazon](https://www.amazon.com) - Fast shipping (higher price)
- [Adafruit](https://www.adafruit.com) - Quality, education-focused
- [SparkFun](https://www.sparkfun.com) - Quality, with tutorials

## ✅ Shopping Checklist

Print this list before shopping:

- [ ] Raspberry Pi 5 (4GB or 8GB)
- [ ] microSD Card (32GB+, Class 10)
- [ ] USB-C Power Adapter (5V 5A)
- [ ] Arduino Uno/Nano
- [ ] RPLidar A1
- [ ] RC Car Chassis (Ackermann)
- [ ] DC Motor + Encoder
- [ ] Servo Motor
- [ ] Motor Driver (L298N or TB6612)
- [ ] LiPo Battery + Charger
- [ ] USB Cables (2 pcs)
- [ ] Jumper Wires (40 pcs set)
- [ ] Screws/Nuts (M2.5, M3)
- [ ] Brackets and Mounting Materials
- [ ] Heatsink/Fan (for Raspberry Pi)

## 🔧 Pre-Assembly Preparation

Once you have all materials:

1. ✅ Test that each component works
2. ✅ Take dimension measurements (for layout planning)
3. ✅ Safety: Research LiPo battery safety
4. ✅ Tools: Prepare screwdriver set, pliers, multimeter

---

**Next Step:** [Software Requirements →](software.md)

