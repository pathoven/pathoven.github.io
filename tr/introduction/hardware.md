# Gerekli Donanımlar

Bu sayfada Pathoven Drive projesini gerçekleştirmek için ihtiyaç duyacağınız tüm donanım bileşenlerini bulabilirsiniz.

## 🎯 Zorunlu Bileşenler

### 1. Raspberry Pi 5

**Neden Raspberry Pi 5?**
- Güçlü ARM Cortex-A76 işlemci (2.4GHz quad-core)
- ROS2 Jazzy için yeterli performans
- USB 3.0 portları (LiDAR ve Arduino için)
- GPIO pinleri (gelecekteki genişlemeler için)
- Aktif cooling desteği

**Özellikler:**
- Model: Raspberry Pi 5
- RAM: 4GB minimum (8GB önerilir)
- Storage: 32GB+ microSD kart (Class 10 veya UHS-I)
- Güç: USB-C PD, 5V 3A minimum (5A önerilir)

**Tahmini Maliyet:** ~$60-80 (RAM'e göre)

**Alternatifler:**
- ✅ Raspberry Pi 4 (8GB) - Biraz daha yavaş ama çalışır
- ⚠️ Raspberry Pi 4 (4GB) - Sınırlı performans
- ❌ Raspberry Pi 3 - Yetersiz (ROS2 ağır gelir)

### 2. Arduino Uno / Nano

**Rol:** Motor kontrolü, enkoder okuma, düşük seviye I/O

**Özellikler:**
- İşlemci: ATmega328P
- Saat Hızı: 16MHz
- GPIO: 14 dijital, 6 analog pin
- İletişim: USB Serial (115200 baud)

**Tahmini Maliyet:** ~$5-25

**Alternatifler:**
- ✅ Arduino Nano - Daha küçük, aynı işlevsellik
- ✅ Arduino Mega - Daha fazla pin (gelecek genişlemeler için)
- ⚠️ ESP32 - Farklı kod gerektirir ama WiFi avantajı var

### 3. RPLidar A1

**Rol:** 360° lazer tarama, SLAM için çevre algılama

**Özellikler:**
- Tarama mesafesi: 0.15m - 12m (beyaz yüzeylerde)
- Açısal çözünürlük: ~0.5° (720 nokta/tarama)
- Tarama hızı: 5.5Hz
- Arayüz: USB Serial
- Boyut: 98mm çap x 39mm yükseklik
- Ağırlık: ~190g
- Güç: 5V, ~400mA

**Tahmini Maliyet:** ~$100-120

**Not:** RPLidar A2/A3 daha gelişmiş ama bu proje için A1 yeterlidir.

**Alternatifler:**
- ⚠️ YDLIDAR X4 (~$90) - Benzer performans
- ❌ Ultrasonic sensors - SLAM için yetersiz
- ❌ Kamera (depth sensing) - Hesaplama yükü çok fazla

### 4. RC Car Chassis

**Gereksinimler:**
- Direksiyon tipi: **Ackermann steering** (araba benzeri)
- Wheelbase: ~15cm (10-20cm arası)
- Motor: DC motor (tercihen encoder'lı)
- Servo motor: Standard size (SG90 veya benzeri)
- Chassis boyutu: Raspberry Pi ve LiDAR'ı taşıyabilecek

**Önerilen Özellikler:**
- Metal chassis (plastik de olur ama dayanıklılık az)
- Ayarlanabilir süspansiyon
- Geniş montaj alanı
- Batarya bölmesi

**Tahmini Maliyet:** ~$30-60

**Örnekler:**
- 1/18 ölçek RC car chassis
- DIY robot car kit (4WD veya Ackermann)

### 5. Motor Sürücü

**Rol:** Arduino'dan gelen PWM sinyallerini motor için güçlendirir

**Önerilen:** L298N Motor Driver veya benzeri
- Çift H-bridge
- 5-35V giriş voltajı
- Maks 2A per kanal
- Enable pinleri (PWM kontrol için)

**Tahmini Maliyet:** ~$3-5

**Alternatifler:**
- L293D (daha düşük akım, <1A)
- TB6612FNG (daha verimli, önerilir)
- DRV8833 (düşük voltaj için)

## 📦 Diğer Gerekli Malzemeler

### Kablolar ve Bağlantılar
- USB-A to USB-B kablo (Arduino için) - ~$3
- USB-A to Micro-USB kablo (RPLidar için) - ~$3
- Dupont jumper kabloları (M-M, M-F, F-F) - ~$5
- Power kabloları (silikon 22-24 AWG) - ~$5

### Güç Kaynağı
- **Raspberry Pi için:** 5V 5A USB-C adaptör - ~$10
- **Motor ve Servo için:** 7.4V LiPo batarya (2S, 2000-3000mAh) - ~$15-25
- **Alternatif:** 4x AA batarya tutucusu (6V) - ~$5

**⚠️ Önemli:** Raspberry Pi ve motor sistemi **ayrı** güç kaynaklarından beslenmeli!

### Montaj Malzemeleri
- Vidalı destek (M2.5, M3 boyları) - ~$5
- Çift taraflı bant / velcro - ~$3
- Plastik veya metal braketler - ~$5
- Kablo bağları (zip tie) - ~$2

### Opsiyonel Soğutma (Önerilir)
- Raspberry Pi 5 için aktif soğutucu (fan) - ~$5-10
- Heatsink'ler - ~$3

## 🔌 Elektrik Bağlantı Şeması

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
       └─────────► Servo 5V/GND (eğer driver'da var ise)
```

## 💰 Toplam Maliyet Tahmini

| Bileşen | Fiyat Aralığı |
|---------|---------------|
| Raspberry Pi 5 (8GB) | $80 |
| microSD Card (64GB) | $10 |
| Raspberry Pi Güç Adaptörü | $10 |
| Arduino Uno | $25 |
| RPLidar A1 | $110 |
| RC Car Chassis | $50 |
| Motor Driver (L298N) | $5 |
| Servo Motor | $5 |
| DC Motor + Encoder | $10 |
| LiPo Battery (2S 2200mAh) | $20 |
| Kablolar ve Bağlantılar | $20 |
| Montaj Malzemeleri | $10 |
| **TOPLAM** | **~$355** |

**💡 Maliyet Düşürme İpuçları:**
- Raspberry Pi 4 (4GB) kullanın (-$20)
- İkinci el RC car chassis (-$20)
- Arduino Nano kullanın (-$15)
- Varolan malzemeleri kullanın (kablo, vida vb.)

**Minimum bütçe ile:** ~$250-280

## 🛒 Nereden Temin Edilir?

### Online Mağazalar (Türkiye)
- [robotistan.com](https://www.robotistan.com) - Robotik malzemeler
- [direnc.net](https://www.direnc.net) - Elektronik komponentler
- [hepsiburada.com](https://www.hepsiburada.com) - Raspberry Pi, Arduino
- [n11.com](https://www.n11.com) - Genel elektronik

### Uluslararası
- [AliExpress](https://www.aliexpress.com) - Uygun fiyat (uzun kargo)
- [Amazon](https://www.amazon.com) - Hızlı kargo (yüksek fiyat)
- [Adafruit](https://www.adafruit.com) - Kaliteli, eğitim odaklı
- [SparkFun](https://www.sparkfun.com) - Kaliteli, tutorial'lı

## ✅ Satın Alma Kontrol Listesi

Alışverişe çıkmadan önce bu listeyi yazdırın:

- [ ] Raspberry Pi 5 (4GB veya 8GB)
- [ ] microSD Card (32GB+, Class 10)
- [ ] USB-C Power Adapter (5V 5A)
- [ ] Arduino Uno/Nano
- [ ] RPLidar A1
- [ ] RC Car Chassis (Ackermann)
- [ ] DC Motor + Encoder
- [ ] Servo Motor
- [ ] Motor Driver (L298N veya TB6612)
- [ ] LiPo Battery + Charger
- [ ] USB Kablolar (2 adet)
- [ ] Jumper Kabloları (40 adet set)
- [ ] Vida/Somunlar (M2.5, M3)
- [ ] Braketler ve Montaj Malzemeleri
- [ ] Heatsink/Fan (Raspberry Pi için)

## 🔧 Montaj Öncesi Hazırlık

Tüm malzemeler elinizdeyse:

1. ✅ Her bileşenin çalıştığını test edin
2. ✅ Boyut ölçümleri yapın (yerleşim planı)
3. ✅ Güvenlik: LiPo batarya güvenliği araştırın
4. ✅ Araçlar: Tornavida seti, pense, multimetre hazırlayın

---

**Sonraki Adım:** [Yazılım Gereksinimleri →](software.md)

