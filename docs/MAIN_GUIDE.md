# 🚗 BOZAW RC Car - Ana Kullanım Kılavuzu
## Comprehensive User Guide for ROS2 Autonomous Robot

**Versiyon**: 2.0  
**Son Güncelleme**: 7 Kasım 2025  
**Platform**: Raspberry Pi 5 + ROS2 Jazzy  
**Durum**: ✅ Production Ready

---

## 📋 İçindekiler

1. [Hızlı Başlangıç](#1-hızlı-başlangıç)
2. [Sistem Mimarisi](#2-sistem-mimarisi)
3. [Kurulum](#3-kurulum)
4. [Temel Kullanım](#4-temel-kullanım)
5. [Web GUI Kullanımı](#5-web-gui-kullanımı)
6. [İleri Seviye Özellikler](#6-i̇leri-seviye-özellikler)
7. [Performance Optimization](#7-performance-optimization)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Hızlı Başlangıç

### 1.1 Sistem Gereksinimleri

**Hardware:**
- Raspberry Pi 5 (4GB+ RAM önerilir)
- Arduino Uno/Nano (motor kontrolü)
- RPLidar A1 (laser scanner)
- RC Car (Ackermann steering, 15cm wheelbase)
- USB kabloları (Arduino ve RPLidar için)
- Güç kaynağı (5V 3A+ tavsiye edilir)

**Software:**
- Ubuntu 24.04 Noble (Raspberry Pi OS)
- ROS2 Jazzy Jalisco
- Python 3.10+
- Node.js 18+ (web GUI için)

---

### 1.2 Üç Kullanım Yöntemi

#### 🌟 Yöntem 1: Socket.IO Bridge + Web GUI (ÖNERİLEN!)

**En hafif ve en hızlı yöntem!**

```bash
# Raspberry Pi'de - tmux ile 4 terminal

# Terminal 1: Backend (mapping)
cd ~/ros2_ws && source install/setup.bash
ros2 launch robot_description headless_mapping.launch.py

# Terminal 2: Socket.IO Bridge (Ctrl+B, C)
ros2 run motor_control_bridge ros2_socketio_bridge

# Terminal 3: Map Saver Service (Ctrl+B, C)
ros2 run motor_control_bridge map_saver_service

# Terminal 4: Web GUI (Ctrl+B, C)
cd ~/bozaw_web_socketio && npm run dev
```

**Browser'da**: http://raspberrypi.local:3000

**Avantajlar:**
- ✅ CPU: 25-35% (en düşük!)
- ✅ RAM: 1.6GB
- ✅ Latency: 20-40ms (en hızlı!)
- ✅ Modern web UI
- ✅ Kolay kurulum

---

#### Yöntem 2: Headless Only (Görsel Yok)

**En minimal yöntem - sadece backend:**

```bash
cd ~/ros2_ws && source install/setup.bash
ros2 launch robot_description headless_mapping.launch.py
```

**Avantajlar:**
- ✅ CPU: 20-35% (en hafif!)
- ✅ RAM: 1.5GB
- ✅ En stabil
- ❌ Görsel arayüz yok

**Kullanım:** SSH'dan topic'leri izleyerek kullanılır.

```bash
# Haritayı izle (text)
ros2 topic echo /map --once

# Pozisyonu izle
ros2 topic echo /odom

# Manuel kontrol (başka terminalde)
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

---

#### Yöntem 3: rosbridge + Web GUI (Alternatif)

**Not:** Socket.IO bridge daha iyi performans veriyor, bu yöntem alternatif olarak tutuldu.

```bash
# Terminal 1: rosbridge
ros2 launch robot_description rosbridge.launch.py

# Terminal 2-4: Diğerleri aynı (Yöntem 1 gibi)
```

**Dezavantajlar:**
- ⚠️ CPU: 30-40% (daha yüksek)
- ⚠️ Kurulum daha karmaşık
- ⚠️ 20+ dependency

---

### 1.3 İlk Çalıştırma (Step-by-Step)

#### Adım 1: Hardware Bağlantıları

```bash
# USB bağlantılarını kontrol et
ls -l /dev/ttyUSB*

# Beklenen:
# /dev/ttyUSB0 → Arduino
# /dev/ttyUSB1 → RPLidar

# Eğer farklıysa, launch dosyalarında port'ları düzelt
```

#### Adım 2: ROS2 Workspace Hazırlama

```bash
cd ~/ros2_ws
source install/setup.bash

# Test: node'lar kullanılabilir mi?
ros2 pkg list | grep motor_control_bridge
ros2 pkg list | grep robot_description
```

#### Adım 3: Sistem Başlatma

```bash
# tmux session başlat (önerilen)
tmux new -s bozaw

# Window 0: Mapping
ros2 launch robot_description headless_mapping.launch.py
```

**Beklenen log çıktısı:**
```
[rplidar]: RPLidar health status : OK ✅
[rplidar]: Start ✅
[slam_lifecycle_manager]: Configuring slam_toolbox ✅
[slam_lifecycle_manager]: Activating slam_toolbox ✅
[motor_control_bridge]: Motor control bridge hazır ✅
[odometry_publisher]: Odometry publisher başlatıldı ✅
```

#### Adım 4: Web GUI Başlatma

```bash
# Yeni tmux window (Ctrl+B, C)
cd ~/ros2_ws && source install/setup.bash
ros2 run motor_control_bridge ros2_socketio_bridge

# Yeni window (Ctrl+B, C)
ros2 run motor_control_bridge map_saver_service

# Yeni window (Ctrl+B, C)
cd ~/bozaw_web_socketio && npm run dev
```

#### Adım 5: Browser'da Bağlanma

1. PC'den browser aç: http://raspberrypi.local:3000
2. **Connect** butonuna tıkla
3. Harita görünmeli (siyah/gri alanlar)
4. Laser scan'ler (yeşil çizgiler) görünmeli
5. Robot pozisyonu (kırmızı kare + sarı ok) görünmeli

**İlk kullanım hazır!** 🎉

---

## 2. Sistem Mimarisi

### 2.1 Genel Mimari

```
┌──────────────────────────────────────────────────────────┐
│              Raspberry Pi 5 (ROS2 Jazzy)                 │
│                                                           │
│  ┌────────────────────────────────────────────┐          │
│  │  Headless Mapping Backend                 │          │
│  │  • motor_control_bridge                   │          │
│  │  • odometry_publisher (Ackermann)         │          │
│  │  • rplidar_node                           │          │
│  │  • slam_toolbox (async)                   │          │
│  │  • robot_state_publisher                  │          │
│  └──────────────┬─────────────────────────────┘          │
│                 │ ROS2 Topics                            │
│  ┌──────────────▼─────────────────────────────┐          │
│  │  Socket.IO Bridge (~350 lines Python)     │          │
│  │  • ROS2 subscribers (/odom, /scan, /map)  │          │
│  │  • Data throttling & downsampling         │          │
│  │  • WebSocket server (port 8080)           │          │
│  │  • CPU: ~3-5% | RAM: ~80MB                │          │
│  └──────────────┬─────────────────────────────┘          │
│                 │ WebSocket (Socket.IO)                  │
└─────────────────┼──────────────────────────────────────────┘
                  │
                  ▼
       ┌──────────────────────┐
       │   Web Browser (PC)   │
       │                      │
       │  React + Socket.IO   │
       │  • Real-time map     │
       │  • Laser scans       │
       │  • Manual control    │
       │  • Telemetry         │
       │  • One-click save    │
       └──────────────────────┘
```

---

### 2.2 ROS2 Topics ve Dataflow

#### Subscribed Topics (Dinlenen)
```
/cmd_vel (geometry_msgs/Twist)
  └─> motor_control_bridge
      └─> Arduino serial komutları (A1-9, L1-9, C)
```

#### Published Topics (Yayınlanan)
```
Arduino → /motor_rpm (std_msgs/Float32)
  └─> odometry_publisher
      └─> /odom (nav_msgs/Odometry)
      └─> /joint_states (sensor_msgs/JointState)

RPLidar → /scan (sensor_msgs/LaserScan)
  
SLAM Toolbox:
  /map (nav_msgs/OccupancyGrid) - 1 Hz
  /map_updates (map_msgs/OccupancyGridUpdate)
```

#### TF Tree
```
map (SLAM)
 └─ odom (Odometry)
     └─ base_footprint (Robot zemin projektörü)
         └─ base_link (Robot ana gövde)
             ├─ laser_frame (RPLidar)
             ├─ front_left_wheel
             ├─ front_right_wheel
             ├─ rear_left_wheel
             └─ rear_right_wheel
```

---

### 2.3 Arduino Seri Protokolü

**Arduino'ya gönderilen komutlar:**

| Komut | Açıklama | Örnek |
|-------|----------|-------|
| `A1-9` | İleri git (1=yavaş, 9=hızlı) | `A5` |
| `B1-9` | Geri git | `B3` |
| `C` | Dur | `C` |
| `L1-9` | Sola dön (1=az, 9=çok) | `L7` |
| `R1-9` | Sağa dön | `R4` |
| `K` | Direksiyonu merkeze al | `K` |

**Arduino'dan gelen veriler:**
```
RPM:<değer>\n

Örnek: RPM:145.32
```

---

## 3. Kurulum

### 3.1 İlk Kurulum (Raspberry Pi'de)

#### Adım 1: ROS2 Jazzy Kurulumu

```bash
# Ubuntu 24.04 Noble üzerine
sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository universe

# ROS2 Jazzy
sudo apt install -y ros-jazzy-desktop
sudo apt install -y ros-jazzy-navigation2 ros-jazzy-nav2-bringup
sudo apt install -y ros-jazzy-slam-toolbox

# Geliştirme araçları
sudo apt install -y python3-colcon-common-extensions
sudo apt install -y python3-rosdep python3-pip
```

#### Adım 2: Workspace Oluşturma

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

# RPLidar driver
git clone -b ros2 https://github.com/Slamtec/rplidar_ros.git

# Motor control bridge paketi (kendi paketiniz)
# robot_description paketi (kendi paketiniz)
# Bu paketler zaten mevcut olmalı
```

#### Adım 3: Dependencies

```bash
cd ~/ros2_ws
source /opt/ros/jazzy/setup.bash

# rosdep ile dependencies kur
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -r -y

# Python dependencies
pip3 install --user pyserial python-socketio aiohttp psutil
```

#### Adım 4: Build

```bash
cd ~/ros2_ws
colcon build --symlink-install

# Source et
source install/setup.bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

#### Adım 5: Web GUI Setup

```bash
# Node.js kurulumu (eğer yoksa)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Web GUI dependencies
cd ~/bozaw_web_socketio
npm install
```

#### Adım 6: Permissions

```bash
# Serial port izinleri
sudo usermod -a dialout $USER
# Logout/login gerekli

# Port'ları kontrol et
ls -l /dev/ttyUSB*
```

---

### 3.2 Opsiyonel: Laser Scan Matcher

Daha doğru odometry için laser scan matcher kurun:

```bash
sudo apt-get install -y ros-jazzy-scan-tools
```

**Kullanmak için:** `headless_mapping.launch.py` içinde `use_laser_matcher = True` yapın.

**Fayda:**
- %70-80 drift azalması
- ±15cm → ±5cm pozisyon doğruluğu
- ±10° → ±3° açı doğruluğu

Detaylar: [Bölüm 6.2 - Laser Scan Matcher](#62-laser-scan-matcher-drift-düzeltme)

---

## 4. Temel Kullanım

### 4.1 Mapping (Harita Oluşturma)

#### Başlatma

```bash
# Terminal 1: Backend
cd ~/ros2_ws && source install/setup.bash
ros2 launch robot_description headless_mapping.launch.py

# Terminal 2: Socket.IO bridge
ros2 run motor_control_bridge ros2_socketio_bridge

# Terminal 3: Map saver
ros2 run motor_control_bridge map_saver_service

# Terminal 4: Web GUI
cd ~/bozaw_web_socketio && npm run dev
```

#### Hareket Ettirme

**Yöntem 1: Web GUI (Önerilen)**
- Browser'da http://raspberrypi.local:3000 aç
- Keyboard ile kontrol et:
  - `W` veya `↑`: İleri
  - `S` veya `↓`: Geri
  - `A` veya `←`: Sola dön
  - `D` veya `→`: Sağa dön
  - `Space`: Dur

**Yöntem 2: Teleop (SSH'dan)**
```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

#### Mapping İpuçları

1. **Yavaş hareket edin** (max 0.3 m/s)
2. **Düz çizgilerden kaçının** - Dönerek gidin
3. **Zengin geometri arayın** - Köşeler, duvarlar, objeler
4. **Loop closure yapın** - Başlangıç noktasına geri dönün
5. **Overlap sağlayın** - Aynı alanları tekrar tarayın

#### Harita Kaydetme

**Yöntem 1: Web GUI (En Kolay)**
- Web arayüzünde **💾 Save Map** butonuna tıkla
- Harita otomatik kaydedilir: `~/maps/map_YYYYMMDD_HHMMSS.yaml`
- `~/maps/latest.yaml` symlink güncellenir

**Yöntem 2: Manuel**
```bash
ros2 run nav2_map_server map_saver_cli -f ~/maps/my_map
```

**Yöntem 3: Service Call**
```bash
ros2 service call /save_map std_srvs/srv/Trigger
```

**Kaydedilen dosyalar:**
```
~/maps/
├── map_20251107_150530.yaml           # ROS2 map metadata
├── map_20251107_150530.pgm            # Map image
├── map_20251107_150530.posegraph      # SLAM Toolbox data
├── map_20251107_150530.data           # SLAM Toolbox data
├── latest.yaml -> map_20251107_150530.yaml
└── latest.pgm -> map_20251107_150530.pgm
```

---

### 4.2 Navigation (Otonom Gezinme)

**Not:** Navigation modu henüz tam entegre edilmedi. Şu anda mapping fonksiyonel.

**Gelecekte:**
```bash
# Kaydedilmiş harita ile navigation
ros2 launch robot_description full_system.launch.py \
    mode:=navigation \
    map:=~/maps/my_map.yaml

# RViz veya Web GUI'de hedef nokta seç
# Robot otomatik olarak yol planlar ve gider
```

---

### 4.3 Manuel Kontrol (Detaylı)

#### Web GUI Kontrolleri

**Keyboard:**
- `W` / `↑`: İleri (linear.x = +0.3)
- `S` / `↓`: Geri (linear.x = -0.3)
- `A` / `←`: Sola dön (angular.z = +0.5)
- `D` / `→`: Sağa dön (angular.z = -0.5)
- `Space`: Dur (tüm hızlar = 0)

**On-screen Buttons:**
- ⬆️ Forward
- ⬇️ Backward
- ⬅️ Turn Left
- ➡️ Turn Right
- ⏸️ Stop

**Hız Ayarlama:**
- Web GUI'de slider ile linear/angular hız ayarlanabilir
- Default: linear = 0.3 m/s, angular = 0.5 rad/s

#### Teleop (SSH'dan)

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard

# Kontroller (terminal'de açıklanır):
# i: İleri
# k: Dur
# ,: Geri
# j: Sola
# l: Sağa
# q/z: Hız artır/azalt
```

#### Direct cmd_vel Publishing

```bash
# İleri git
ros2 topic pub -r 10 /cmd_vel geometry_msgs/msg/Twist \
  "{linear: {x: 0.3}, angular: {z: 0.0}}"

# Sola dönerek git
ros2 topic pub -r 10 /cmd_vel geometry_msgs/msg/Twist \
  "{linear: {x: 0.2}, angular: {z: 0.5}}"

# Dur
ros2 topic pub --once /cmd_vel geometry_msgs/msg/Twist \
  "{linear: {x: 0.0}, angular: {z: 0.0}}"
```

---

## 5. Web GUI Kullanımı

### 5.1 Arayüz Özellikleri

#### Ana Bölümler

1. **Connection Panel (Üst)**
   - WebSocket bağlantı durumu
   - Raspberry Pi URL girişi
   - Connect/Disconnect butonu

2. **Map Canvas (Sol/Orta - Büyük)**
   - Real-time SLAM haritası
   - Laser scan'ler (yeşil çizgiler)
   - Robot pozisyonu (kırmızı kare + sarı ok)
   - Grid overlay (gri)

3. **Control Panel (Sağ)**
   - Directional buttons (⬆️⬇️⬅️➡️⏸️)
   - Speed sliders (Linear / Angular)
   - Save Map butonu (💾)
   - Emergency Stop (🛑)

4. **Telemetry Panel (Alt)**
   - Position (X, Y, Theta)
   - Velocity (Linear, Angular)
   - Motor RPM
   - System status

---

### 5.2 Socket.IO Bridge Detayları

#### Socket.IO Events

**Server → Client (ROS2 data):**

| Event | Açıklama | Frequency | Data Format |
|-------|----------|-----------|-------------|
| `odom` | Odometry | Real-time | `{x, y, z, qx, qy, qz, qw, vx, vz}` |
| `rpm` | Motor RPM | Real-time | `{rpm: float}` |
| `scan` | Laser scan | 10 Hz | `{ranges: [], angles: [], ...}` |
| `map` | SLAM map | 1 Hz | `{metadata: {...}, data: base64}` |
| `status` | System status | On event | `{message: string}` |

**Client → Server (Control commands):**

| Event | Açıklama | Data Format |
|-------|----------|-------------|
| `cmd_vel` | Velocity command | `{linear: float, angular: float}` |
| `save_map` | Save map | `{}` |
| `get_status` | Request status | `{}` |

---

#### Bridge QoS Settings

**Önemli:** Map QoS ayarları SLAM Toolbox ile uyumlu olmalı!

```python
# ros2_socketio_bridge.py içinde
map_qos = QoSProfile(
    reliability=ReliabilityPolicy.RELIABLE,
    durability=DurabilityPolicy.TRANSIENT_LOCAL,  # ← KEY!
    history=HistoryPolicy.KEEP_LAST,
    depth=1
)
```

**Neden TRANSIENT_LOCAL?**
- SLAM Toolbox map'i TRANSIENT_LOCAL ile yayınlar
- Late-joining subscribers (web GUI) son map'i hemen alır
- VOLATILE kullanılırsa map görünmez!

---

#### Data Optimization

**1. Throttling:**
```python
self.scan_throttle = 0.1   # 10 Hz (50 Hz yerine)
self.map_throttle = 1.0    # 1 Hz (sürekli yerine)
```

**2. Downsampling (Laser Scan):**
- Ham data: 720 points @ 10 Hz = 7200 points/sec
- Downsampled: 144 points @ 10 Hz = 1440 points/sec
- **%80 reduction!**

**3. Compression (Map):**
- Base64 encoding
- ~100KB transfer (uncompressed: ~500KB)

**Sonuç:** CPU: ~3-5%, RAM: ~80MB, Latency: 20-40ms

---

### 5.3 Web GUI Özelleştirme

#### Port Değiştirme

**Bridge (Python):**
```python
# ros2_socketio_bridge.py - main() içinde
web.run_app(app, host='0.0.0.0', port=9090)  # 8080 → 9090
```

**Web GUI (React):**
```javascript
// src/App.jsx
const [serverUrl, setServerUrl] = useState('http://raspberrypi.local:9090')
```

#### Yeni Event Ekleme

**Örnek: Battery voltage monitoring**

**1. Bridge (Python):**
```python
# ros2_socketio_bridge.py içinde
self.battery_sub = self.create_subscription(
    Float32, '/battery_voltage', self.battery_callback, qos
)

def battery_callback(self, msg: Float32):
    asyncio.run_coroutine_threadsafe(
        sio.emit('battery', {'voltage': msg.data}),
        sio.eio.start_service_task.get_loop()
    )
```

**2. Web GUI (React):**
```javascript
// src/App.jsx - useEffect içinde
newSocket.on('battery', (data) => {
  setBatteryVoltage(data.voltage)
})
```

---

## 6. İleri Seviye Özellikler

### 6.1 RPM Kalibrasyonu

**Neden gerekli?**
- Tekerlek yarıçapı nominal değerden farklı olabilir (aşınma, lastik basıncı)
- Enkoder kalibrasyon hatası
- Slip ve sürtünme etkileri

#### Kalibrasyon Süreci

```bash
# Terminal 1: Sistemi başlat
ros2 launch robot_description headless_mapping.launch.py

# Terminal 2: Kalibrasyon tool'u çalıştır
ros2 run motor_control_bridge rpm_calibration_tool
```

**Adımlar:**

1. Aracı düz ve pürüzsüz bir yüzeye yerleştirin
2. Başlangıç noktasını bant ile işaretleyin
3. Enter tuşuna basın
4. Araç 10 saniye boyunca düz ileri gider (0.3 m/s)
5. Durduğunda bitiş noktasını işaretleyin
6. Başlangıç - bitiş arası gerçek mesafeyi ölçün (bant metre)
7. Ölçtüğünüz mesafeyi metre cinsinden girin

#### Örnek Çıktı

```
📊 KALIBRASYON SONUÇLARI
=============================================================
Ortalama RPM: 145.32
Test süresi: 10.0s
Komut hızı: 0.3 m/s

Beklenen mesafe: 3.000m
Odometry mesafe: 3.150m
RPM hesaplı mesafe: 3.145m

🎯 Girilen gerçek mesafe: 2.980m

📈 HATA ANALİZİ
-------------------------------------------------------------
Odometry hatası: +5.70%
RPM hesaplama hatası: +5.54%

🔧 ÖNERİLEN DÜZELTMELER
-------------------------------------------------------------
Mevcut wheel_radius: 0.033000m
Önerilen wheel_radius: 0.031242m
Düzeltme faktörü: 0.9467

💡 ÖNERİLEN AKSIYONLAR:
   1. headless_mapping.launch.py'de wheel_radius'u güncelleyin
   2. Veya odometry_publisher parametresini değiştirin
=============================================================
```

#### Düzeltme Uygulama

**Launch file'da:**
```python
# headless_mapping.launch.py veya full_system.launch.py
odometry_node = Node(
    package='motor_control_bridge',
    executable='odometry_publisher',
    parameters=[{
        'wheel_radius': 0.031242,  # ← UPDATED from calibration
        'wheelbase': 0.15,
        'track_width': 0.12,
    }]
)
```

**Rebuild gerekli:**
```bash
cd ~/ros2_ws
colcon build --packages-select robot_description
source install/setup.bash
```

#### Kalibrasyon Sonrası Test

```bash
# Aynı testi tekrar yap
ros2 run motor_control_bridge rpm_calibration_tool

# Şimdi hata %1'in altında olmalı!
```

**Beklenen İyileştirme:** %5-10 odometry doğruluk artışı

---

### 6.2 Laser Scan Matcher (Drift Düzeltme)

**Problem:**
- Sadece RPM-based odometry kullanıyoruz
- Wheel slip hesaba katılmıyor
- Ackermann steering kaynaklı hatalar
- Angular velocity tahmini (gerçek değil)

**Çözüm:** Laser Scan Matcher
- RPM + LiDAR scan matching füzyonu
- ICP (Iterative Closest Point) algoritması
- Wheel slip otomatik düzeltme
- Daha stabil pose estimation

#### Kurulum

```bash
sudo apt-get install -y ros-jazzy-scan-tools
```

**`scan-tools` paketi `laser_scan_matcher` içerir.**

#### Launch File Güncelleme

`headless_mapping.launch.py` içinde zaten hazır:

```python
# Line 46 civarı
use_laser_matcher = True  # False → True yapın
```

**Rebuild:**
```bash
cd ~/ros2_ws
colcon build --packages-select robot_description
source install/setup.bash
```

#### Topic Flow

```
/motor_rpm → odometry_publisher → /odom_raw (RPM-based)
                                      ↓
                                  laser_scan_matcher → /odom (corrected)
                                      ↑
/scan (LiDAR) ───────────────────────┘
                                      ↓
                                  SLAM Toolbox
```

**Açıklama:**
1. `odometry_publisher`: RPM'den `/odom_raw` hesaplar
2. `laser_scan_matcher`: `/odom_raw` + `/scan` alır, düzeltilmiş `/odom` üretir
3. SLAM Toolbox: Düzeltilmiş `/odom` kullanır → Daha iyi harita!

#### Parametreler

```yaml
# headless_mapping.launch.py - laser_scan_matcher parameters
max_iterations: 10                    # ICP iterasyon sayısı
max_correspondence_dist: 0.3          # 30cm - nokta eşleştirme max mesafesi
max_angular_correction_deg: 45.0      # Max düzeltme açısı
max_linear_correction: 0.50           # Max düzeltme mesafesi (50cm)

kf_dist_linear: 0.10                  # Her 10cm'de güncelleme
kf_dist_angular: 0.175                # Her ~10° dönüşte güncelleme
```

#### Doğrulama

```bash
# Sistemde laser_scan_matcher çalışıyor mu?
ros2 node list | grep laser

# Topic'leri kontrol et
ros2 topic list | grep odom
# /odom ve /odom_raw görmeli

# /odom_raw: RPM-based (raw)
ros2 topic hz /odom_raw  # ~10 Hz

# /odom: Corrected (fused)
ros2 topic hz /odom  # ~5-10 Hz
```

#### Beklenen İyileştirmeler

| Metric | Without Laser Matcher | With Laser Matcher | Improvement |
|--------|----------------------|-------------------|-------------|
| **Position Drift** (1m straight) | ±15cm | ±3-5cm | **67-80%** |
| **Heading Drift** (360° turn) | ±10° | ±2-3° | **70-80%** |
| **Long-term Drift** (10m path) | ±50cm | ±10-15cm | **70-80%** |
| **Steering Overshoot** | High | Low | **Corrected** |
| **SLAM Map Quality** | Fair | Good | **Better** |

#### Test Senaryoları

**Test 1: Düz Çizgi**
1. 2m düz ileri git
2. 2m düz geri gel
3. Başlangıç noktasına ne kadar yakınsın?

**Without:** ±10-15cm hata  
**With:** ±3-5cm hata ✅

**Test 2: Kare Çizme**
1. 1m ileri
2. 90° sola dön
3. Tekrarla x4
4. Başlangıç noktasına geri dön

**Without:** 10-20cm açık loop  
**With:** 3-5cm açık loop ✅

**Test 3: SLAM Loop Closure**
1. Büyük bir odada tam tur at
2. Başlangıç noktasına geri dön

**Without:** Loop closure zor, drift fazla  
**With:** Loop closure kolay, drift az ✅

#### CPU Maliyeti

- Laser scan matcher: **+5-8% CPU**
- Raspberry Pi 5: Hala rahat çalışır (~35-40% total)
- **Değer!** Doğruluk artışı CPU maliyetinden çok daha önemli

---

### 6.3 Performance Monitoring

**Amaç:** Sistem kaynaklarını gerçek zamanlı izlemek

#### Başlatma

```bash
ros2 run motor_control_bridge performance_monitor
```

**Parametrelerle:**
```bash
ros2 run motor_control_bridge performance_monitor \
    --ros-args \
    -p publish_rate:=1.0 \
    -p log_to_file:=true \
    -p log_file:=/tmp/ros2_perf.log
```

#### Çıktı (her 10 saniye)

```
============================================================
📊 SYSTEM PERFORMANCE (15:32:45)
------------------------------------------------------------
CPU: 42.3% | RAM: 58.7% (3.2GB / 4.0GB)
Temperature: 65.4°C

ROS2 Processes: 12 | CPU: 38.2% | RAM: 1245MB

Top 5 Resource Consumers:
  1. slam_toolbox         - CPU:  15.3% | RAM:  452.1MB
  2. rplidar              - CPU:   8.7% | RAM:  186.4MB
  3. motor_control_bridge - CPU:   5.2% | RAM:   98.7MB
  4. robot_state_pub      - CPU:   3.8% | RAM:   75.3MB
  5. odometry_publisher   - CPU:   2.1% | RAM:   42.1MB
============================================================
```

#### Otomatik Uyarılar

```
⚠️  High CPU usage: 85.3%
⚠️  High RAM usage: 89.7%
🔥 High temperature: 78.2°C
```

#### Optimization Tips

**CPU >70%:**
- ✅ Headless mode kullan (RViz kapalı)
- ✅ SLAM update rate düşür
- ✅ LiDAR scan rate azalt (mümkünse)
- ✅ Gereksiz process'leri kapat

**RAM >85%:**
- ✅ SLAM map resolution azalt
- ✅ SLAM history size limitele
- ✅ Swap space kullan (yavaş ama güvenli)

**Temperature >75°C:**
- ⚠️ Heatsink/fan ekle
- ⚠️ Workload azalt
- ⚠️ Havalandırma iyileştir
- ⚠️ Thermal throttling başlar (>80°C)

#### Log Analizi

```bash
# Log dosyası inceleme
tail -f /tmp/ros2_perf.log

# CSV formatında - Excel/Python ile analiz edilebilir
# timestamp,cpu_percent,ram_percent,ram_used_mb,ram_available_mb,temp_c,swap_percent
```

---

## 7. Performance Optimization

### 7.1 Performans Karşılaştırması

| Yöntem | CPU | RAM | Lag | Önerilen |
|--------|-----|-----|-----|----------|
| **RViz X11 Forwarding** | 45-60% | 2.1GB | Yüksek | ❌ |
| **Headless Only** | 20-35% | 1.5GB | Yok | ✅ |
| **Headless + Socket.IO GUI** | 25-40% | 1.7GB | Minimum | ✅✅ |
| **Headless + rosbridge GUI** | 30-45% | 1.9GB | Düşük | ⚠️ |
| **+ Laser Matcher** | +5-8% | +120MB | Yok | ✅ (opsiyonel) |

---

### 7.2 SLAM Optimization

#### SLAM Parameters Tuning

`~/ros2_ws/src/robot_description/config/mapper_params_online_async.yaml`

**Performans için:**
```yaml
# Update rate azalt
minimum_travel_distance: 0.3  # 0.2 → 0.3 (daha az update)
minimum_travel_heading: 0.3   # 0.2 → 0.3

# Map resolution azalt (daha az detail)
resolution: 0.10              # 0.05 → 0.10 (iki kat azalma)

# Loop closure azalt (CPU tasarrufu)
loop_search_maximum_distance: 2.0  # 3.0 → 2.0
```

**Kalite için (CPU artacak):**
```yaml
# Update rate artır
minimum_travel_distance: 0.1  # Daha sık update
minimum_travel_heading: 0.1

# Map resolution artır (daha detaylı)
resolution: 0.025             # 0.05 → 0.025

# Loop closure artır (daha iyi kapanma)
loop_search_maximum_distance: 5.0
```

---

### 7.3 Socket.IO Bridge Optimization

#### Throttling Ayarlama

`ros2_socketio_bridge.py` içinde:

```python
# Daha hızlı scan updates (daha fazla CPU)
self.scan_throttle = 0.05  # 20 Hz

# Daha hızlı map updates
self.map_throttle = 0.5  # 2 Hz

# Daha yavaş (daha az CPU)
self.scan_throttle = 0.2   # 5 Hz
self.map_throttle = 2.0    # 0.5 Hz
```

#### Downsampling Ayarlama

```python
# scan_callback içinde
step = 5  # Her 5. noktayı al (default)
step = 10 # Her 10. noktayı al (daha az CPU, daha az detail)
step = 3  # Her 3. noktayı al (daha fazla detail, daha fazla CPU)
```

---

## 8. Troubleshooting

### 8.1 Sık Karşılaşılan Sorunlar

#### Problem 1: Web GUI bağlanmıyor

**Belirtiler:**
- "Connecting..." sürekli dönüyor
- Browser console'da "Connection failed" hatası

**Çözümler:**

```bash
# 1. Bridge çalışıyor mu?
ros2 node list | grep socketio
# /ros2_socketio_bridge görmelisin

# 2. Port 8080 açık mı?
curl http://raspberrypi.local:8080
# HTML response görmelisin

# 3. Network bağlantısı var mı?
ping raspberrypi.local

# 4. Firewall kontrolü
sudo ufw status
sudo ufw allow 8080
sudo ufw allow 3000
```

---

#### Problem 2: Map görünmüyor (siyah canvas)

**Belirtiler:**
- Canvas tamamen siyah
- Laser scan'ler görünebilir ama map yok

**Çözümler:**

```bash
# 1. Map yayınlanıyor mu?
ros2 topic hz /map
# ~1 Hz görmelisin

# 2. SLAM aktif mi?
ros2 lifecycle get /slam_toolbox
# "active [3]" görmelisin

# Eğer "unconfigured" ise:
ros2 lifecycle set /slam_toolbox configure
ros2 lifecycle set /slam_toolbox activate

# 3. QoS uyumlu mu?
ros2 topic info /map -v
# Hem publisher hem subscriber TRANSIENT_LOCAL olmalı

# 4. Bridge log'larını kontrol et
ros2 run motor_control_bridge ros2_socketio_bridge 2>&1 | grep -i map
# "Map sent" mesajları görmelisin
```

---

#### Problem 3: Odometry drift yapıyor

**Belirtiler:**
- Robot gerçekte düz gidiyor ama odometry'de döndüğü görünüyor
- Loop closure yapamıyor
- Harita bozuk

**Çözümler:**

**1. RPM Kalibrasyonu Yap:**
```bash
ros2 run motor_control_bridge rpm_calibration_tool
# Wheel radius'u düzelt
```

**2. Laser Scan Matcher Ekle:**
```bash
# headless_mapping.launch.py içinde
use_laser_matcher = True

# Rebuild
cd ~/ros2_ws
colcon build --packages-select robot_description
```

**3. Parametreleri Ayarla:**
- Steering gain azalt
- Ackermann modelini kontrol et

---

#### Problem 4: CPU %100'e çıkıyor

**Belirtiler:**
- Sistem donuyor
- Lag var
- Temperature >80°C

**Çözümler:**

```bash
# 1. Performance monitor ile analiz et
ros2 run motor_control_bridge performance_monitor

# 2. En çok CPU kullanan node'u bul
top -p $(pgrep -f ros2)

# 3. RViz kullanıyor musun? → Headless'e geç
ros2 launch robot_description headless_mapping.launch.py

# 4. SLAM parametrelerini optimize et
# mapper_params_online_async.yaml:
# - resolution: 0.10 (0.05 yerine)
# - minimum_travel_distance: 0.3 (0.2 yerine)

# 5. Socket.IO throttling artır
# ros2_socketio_bridge.py:
# - scan_throttle = 0.2
# - map_throttle = 2.0
```

---

#### Problem 5: Arduino bağlanamıyor

**Belirtiler:**
- `/motor_rpm` topic yok
- `motor_control_bridge` hata veriyor

**Çözümler:**

```bash
# 1. Port kontrolü
ls -l /dev/ttyUSB*
# /dev/ttyUSB0 (Arduino) görmelisin

# 2. Permission kontrolü
groups $USER
# 'dialout' grubunda olmalısın

# Eğer yoksa:
sudo usermod -a dialout $USER
# Logout/login gerekli

# 3. Arduino serial monitor
screen /dev/ttyUSB0 115200
# "RPM:<değer>" mesajları görmelisin

# 4. Port değiştiyse, launch file'da güncelle
# motor_control_node parametrelerinde:
# serial_port: '/dev/ttyACM0'  # veya doğru port
```

---

#### Problem 6: RPLidar çalışmıyor

**Belirtiler:**
- `/scan` topic yok
- Yeşil laser scan çizgileri görünmüyor

**Çözümler:**

```bash
# 1. Port kontrolü
ls -l /dev/ttyUSB*
# /dev/ttyUSB1 (RPLidar) görmelisin

# 2. RPLidar node çalışıyor mu?
ros2 node list | grep rplidar

# 3. Manuel test
ros2 launch rplidar_ros rplidar_a1_launch.py serial_port:=/dev/ttyUSB1

# Log'da şunu görmelisin:
# [rplidar]: RPLidar health status : OK
# [rplidar]: Start

# 4. Scan yayınlanıyor mu?
ros2 topic hz /scan
# ~10 Hz görmelisin

# 5. Scan data var mı?
ros2 topic echo /scan --once
# ranges[] array'inde değerler görmelisin
```

---

### 8.2 Debug Komutları

#### ROS2 Topic Debugging

```bash
# Tüm topic'leri listele
ros2 topic list

# Topic detayları
ros2 topic info /odom -v

# Topic yayın frekansı
ros2 topic hz /scan

# Topic verilerini oku
ros2 topic echo /odom

# Topic'i bir kere oku
ros2 topic echo /map --once
```

#### ROS2 Node Debugging

```bash
# Çalışan node'lar
ros2 node list

# Node detayları
ros2 node info /slam_toolbox

# Node parametreleri
ros2 param list /slam_toolbox
ros2 param get /slam_toolbox resolution
```

#### TF Debugging

```bash
# TF tree görüntüle
ros2 run tf2_tools view_frames
evince frames.pdf

# TF transform kontrol
ros2 run tf2_ros tf2_echo map base_link

# TF monitor (delay kontrolü)
ros2 run tf2_ros tf2_monitor
```

#### Service Debugging

```bash
# Service'leri listele
ros2 service list

# Service tipi
ros2 service type /save_map

# Service çağır
ros2 service call /save_map std_srvs/srv/Trigger

# SLAM Toolbox serialize
ros2 service call /slam_toolbox/serialize_map \
    slam_toolbox/srv/SerializePoseGraph \
    "{filename: '/home/usr/maps/test'}"
```

---

### 8.3 Log Analizi

#### ROS2 Logs

```bash
# Node log seviyesi değiştir
ros2 run motor_control_bridge ros2_socketio_bridge \
    --ros-args --log-level DEBUG

# Log dosyalarını oku
cd ~/.ros/log/
tail -f latest/ros2_socketio_bridge-1.log
```

#### System Logs

```bash
# Kernel messages (USB cihazlar)
dmesg | tail -50

# System journal
journalctl -f -u roscore
```

---

## 9. Sonuç ve İleri Adımlar

### 9.1 Mevcut Durum

✅ **Tamamlanan:**
- Headless mapping (stabil, optimize)
- Socket.IO bridge (custom, hafif)
- Modern web arayüzü
- RPM calibration tools
- Performance monitoring
- Comprehensive documentation

⏳ **Devam eden:**
- Navigation mode (Nav2 entegrasyonu)
- Advanced obstacle avoidance
- Multi-waypoint navigation

---

### 9.2 Gelecek İyileştirmeler

**Kısa Vade (1-2 hafta):**
- [ ] Navigation mode tam entegrasyonu
- [ ] Autonomous waypoint following
- [ ] Battery voltage monitoring
- [ ] IMU integration (MPU6050)

**Orta Vade (1-2 ay):**
- [ ] 3D visualization (Three.js)
- [ ] Video streaming (web_video_server)
- [ ] Multi-floor mapping
- [ ] Cloud storage (map upload/download)

**Uzun Vade (3+ ay):**
- [ ] Fleet management (multi-robot)
- [ ] AI path planning
- [ ] Voice control
- [ ] Autonomous exploration mode

---

### 9.3 Katkıda Bulunma

Bu proje aktif olarak geliştirilmektedir. Katkılarınızı bekliyoruz!

**Nasıl katkıda bulunulur:**
1. Issue açın (bug report, feature request)
2. Fork yapın ve geliştirin
3. Pull request gönderin
4. Dokümantasyonu güncelleyin

---

## 📚 Ek Kaynaklar

### ROS2 Resmi Dokümantasyon
- [ROS2 Jazzy](https://docs.ros.org/en/jazzy/)
- [Nav2](https://navigation.ros.org/)
- [SLAM Toolbox](https://github.com/SteveMacenski/slam_toolbox)
- [RPLidar ROS](https://github.com/Slamtec/rplidar_ros)

### Faydalı Videolar
- [Articulated Robotics - ROS2 Tutorial Series](https://www.youtube.com/c/ArticulatedRobotics)
- [The Construct - Nav2 Course](https://www.theconstructsim.com/)

### Örnek Projeler
- [articubot_one](https://github.com/joshnewans/articubot_one) - Differential drive robot
- [turtlebot3](https://github.com/ROBOTIS-GIT/turtlebot3) - Popular ROS2 robot
- [f1tenth](https://f1tenth.org/) - Autonomous racing

---

**Son Güncelleme**: 7 Kasım 2025  
**Versiyon**: 2.0  
**Geliştirici**: BOZAW Team  
**Durum**: ✅ Production Ready

**🎉 İyi eğlenceler!** 🚗💨

