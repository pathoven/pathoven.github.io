# Proje Genel Bakış

## Pathoven Drive Nedir?

Pathoven Drive, **ROS2 (Robot Operating System 2)** kullanarak geliştirilmiş, otonom haritalama ve navigasyon yeteneğine sahip bir RC araç projesidir. Bu proje, robotik programlama, SLAM (Simultaneous Localization and Mapping), ve otonom navigasyon konularında pratik deneyim kazanmak isteyenler için kapsamlı bir öğrenme platformudur.

## 🎯 Proje Hedefleri

### 1. Eğitim ve Öğrenme
- ROS2 ekosistemini anlamak
- Robotik yazılım geliştirme becerilerini artırmak
- SLAM ve navigasyon algoritmalarını uygulamalı öğrenmek
- Gerçek zamanlı sistemler geliştirmek

### 2. Teknik Yeterlilik
- Düşük maliyetli ama güçlü bir platform (Raspberry Pi 5)
- Modüler ve genişletilebilir mimari
- Production-ready kod kalitesi
- Dokümantasyonlu ve sürdürülebilir

### 3. Topluluk ve Paylaşım
- Açık kaynak proje
- Detaylı dokümantasyon
- Yeniden üretilebilir (reproducible)
- Katkıya açık

## 🏗️ Sistem Mimarisi

```
┌─────────────────────────────────────────────────────┐
│           Raspberry Pi 5 (ROS2 Jazzy)               │
│                                                      │
│  ┌────────────────┐        ┌──────────────────┐    │
│  │  Motor Control │◄───────┤   Arduino Uno    │    │
│  │     Bridge     │ Serial │  (Motor Driver)  │    │
│  └────────┬───────┘        └──────────────────┘    │
│           │                                          │
│  ┌────────▼────────┐       ┌──────────────────┐    │
│  │   Odometry      │       │   RPLidar A1     │    │
│  │   Publisher     │       │  (Laser Scanner) │    │
│  └────────┬────────┘       └────────┬─────────┘    │
│           │                          │              │
│           │      ┌───────────────────▼──┐           │
│           └──────►   SLAM Toolbox      │           │
│                  │   (Mapping)         │           │
│                  └──────────┬──────────┘           │
│                             │                       │
│                  ┌──────────▼──────────┐           │
│                  │    Nav2 Stack       │           │
│                  │   (Navigation)      │           │
│                  └──────────┬──────────┘           │
│                             │                       │
│                  ┌──────────▼──────────┐           │
│                  │  Socket.IO Bridge   │           │
│                  │   (WebSocket)       │           │
│                  └──────────┬──────────┘           │
└───────────────────────────┬─┬────────────────────────┘
                            │ │
                    ┌───────▼─▼────────┐
                    │   Web Browser    │
                    │  (Control GUI)   │
                    └──────────────────┘
```

## 🔧 Temel Bileşenler

### 1. Donanım Katmanı

**Raspberry Pi 5**
- İşlemci: Quad-core ARM Cortex-A76 @ 2.4GHz
- RAM: 4GB+ (8GB önerilir)
- İşletim Sistemi: Ubuntu 24.04 Noble
- Rol: Ana hesaplama birimi, ROS2 host

**Arduino Uno/Nano**
- İşlemci: ATmega328P
- Rol: Motor kontrolü, düşük seviye donanım arayüzü
- Görevler: PWM kontrol, enkoder okuma, seri haberleşme

**RPLidar A1**
- Tarama mesafesi: 0.15m - 12m
- Tarama hızı: 5.5Hz (360° / 5.5 = ~2000 noktalar/saniye)
- Çözünürlük: ~0.5°
- Arayüz: USB Serial
- Rol: Çevre algılama, SLAM için veri kaynağı

**RC Car Chassis**
- Direksiyon tipi: Ackermann
- Wheelbase: ~15cm
- Motor: DC motor + encoder
- Servo: Direksiyon kontrolü

### 2. Yazılım Katmanı

**ROS2 Jazzy Jalisco**
- LTS sürüm (Long Term Support)
- Ubuntu 24.04 için optimize
- Modern C++17 ve Python 3.10 desteği

**SLAM Toolbox**
- Graph-based SLAM
- Online ve offline mapping
- Loop closure
- Lifelong mapping desteği

**Nav2 (Navigation2)**
- Behavior Trees
- Regulated Pure Pursuit Controller
- Obstacle avoidance
- Dynamic replanning

**Socket.IO Bridge**
- Custom Python implementasyonu
- Düşük latency (~20-40ms)
- Data throttling ve compression
- WebSocket üzerinden gerçek zamanlı iletişim

## 📊 Performans Metrikleri

| Metrik | Değer |
|--------|-------|
| **CPU Kullanımı** | 25-40% (Raspberry Pi 5) |
| **RAM Kullanımı** | 1.5-1.7 GB |
| **Latency** | 20-40ms (Web GUI) |
| **Map Update Rate** | 1 Hz |
| **Scan Rate** | 10 Hz |
| **Odometry Rate** | 10-20 Hz |
| **Position Accuracy** | ±5cm (laser matcher ile) |
| **Heading Accuracy** | ±3° (laser matcher ile) |

## 🌟 Öne Çıkan Özellikler

### 1. Headless Operation
- SSH üzerinden tam kontrol
- X11 forwarding'e gerek yok
- Düşük CPU ve bandwidth kullanımı

### 2. Modern Web GUI
- React + Vite
- Responsive tasarım
- Gerçek zamanlı harita görselleştirme
- Klavye ve touch kontrol

### 3. Optimized Performance
- Custom Socket.IO bridge (%60 daha az CPU)
- Data throttling ve downsampling
- Async I/O
- QoS optimizasyonları

### 4. Production Ready
- Kapsamlı error handling
- Logging ve monitoring
- Performance profiling araçları
- Detaylı dokümantasyon

## 🎓 Öğrenme Yolu

Bu proje modüler bir yapıya sahiptir ve her modül bağımsız olarak öğrenilebilir:

```
Seviye 1: Temel
├── Arduino programlama
├── Motor kontrolü
└── Serial iletişim

Seviye 2: ROS2 Temelleri
├── ROS2 kurulumu
├── Topics ve Messages
├── Nodes ve Packages
└── Launch files

Seviye 3: Algılama
├── LiDAR entegrasyonu
├── Sensor fusion
├── Odometry
└── TF (Transform) sistemi

Seviye 4: SLAM ve Navigasyon
├── SLAM Toolbox
├── Harita oluşturma
├── Lokalizasyon (AMCL)
└── Path planning (Nav2)

Seviye 5: İleri Seviye
├── Web arayüzü geliştirme
├── Socket.IO bridge
├── Performance optimization
└── Custom ROS2 node geliştirme
```

## 🔄 Geliştirme Durumu

| Modül | Durum | Not |
|-------|-------|-----|
| **Motor Control** | ✅ Tamamlandı | Production ready |
| **Odometry** | ✅ Tamamlandı | Kalibrasyon araçlarıyla |
| **LiDAR Integration** | ✅ Tamamlandı | RPLidar A1 driver |
| **SLAM** | ✅ Tamamlandı | SLAM Toolbox, mapping mode |
| **Web GUI** | ✅ Tamamlandı | Socket.IO + React |
| **Navigation** | 🚧 Geliştirme aşamasında | Nav2 entegrasyonu |
| **Obstacle Avoidance** | 📅 Planlandı | - |
| **Multi-Robot** | 📅 Planlandı | - |

## 🚀 Sonraki Adımlar

Projeye başlamaya hazır mısınız?

1. [Gerekli Donanımlar](hardware.md) - Nelere ihtiyacınız var?
2. [Yazılım Gereksinimleri](software.md) - Yazılım kurulumları
3. [Proje Hedefleri](goals.md) - Detaylı öğrenme çıktıları

---

**💡 İpucu:** Bu dokümantasyon, hiçbir ROS bilginiz olmasa bile başlangıç yapabilmeniz için tasarlanmıştır. Her konuyu sırasıyla takip etmeniz önerilir.

