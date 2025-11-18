# Pathoven Drive - Türkçe Dokümantasyon

> ROS2 Tabanlı Otonom RC Araç Projesi

## 🎯 Proje Hakkında

Pathoven Drive, ROS2 Jazzy kullanarak Raspberry Pi 5 üzerinde çalışan, otonom haritalama ve navigasyon yeteneğine sahip bir RC araç projesidir.

## ✨ Özellikler

- 🤖 **ROS2 Jazzy** - En güncel ROS2 sürümü
- 🍓 **Raspberry Pi 5** - Güçlü işlemci
- 📡 **RPLidar A1** - 360° lazer tarama
- 🗺️ **SLAM Toolbox** - Gerçek zamanlı haritalama
- 🧭 **Nav2** - Otonom navigasyon
- 🌐 **Web Arayüzü** - Modern kontrol paneli
- ⚡ **Socket.IO Bridge** - Düşük gecikmeli iletişim

## 🚀 Hızlı Başlangıç

### Gereksinimler

**Donanım:**
- Raspberry Pi 5 (4GB+ RAM)
- Arduino Uno/Nano
- RPLidar A1
- RC Car (Ackermann direksiyon, 15cm wheelbase)
- USB kabloları
- Güç kaynağı (5V 3A+)

**Yazılım:**
- Ubuntu 24.04 Noble
- ROS2 Jazzy Jalisco
- Python 3.10+
- Node.js 18+

### Kurulum

```bash
# ROS2 Workspace oluştur
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

# Paketleri klonla (örnek)
git clone https://github.com/your-repo/motor_control_bridge
git clone https://github.com/your-repo/robot_description

# Bağımlılıkları kur
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y

# Build
colcon build --symlink-install
source install/setup.bash
```

### İlk Çalıştırma

```bash
# Terminal 1: Backend
ros2 launch robot_description headless_mapping.launch.py

# Terminal 2: Socket.IO Bridge
ros2 run motor_control_bridge ros2_socketio_bridge

# Terminal 3: Web GUI
cd ~/bozaw_web_socketio && npm run dev
```

Browser'da açın: **http://raspberrypi.local:3000**

## 📖 Dokümantasyon Yapısı

Bu dokümantasyon, projenin tüm aşamalarını adım adım anlatmaktadır:

1. **Giriş** - Proje tanıtımı ve gereksinimler
2. **Arduino** - Motor kontrolü ve serial iletişim
3. **Raspberry Pi** - ROS2 kurulumu ve yapılandırma
4. **Motor Control** - ROS2 motor kontrol paketi
5. **Robot Modeli** - URDF ve TF sistemi
6. **LiDAR** - RPLidar entegrasyonu
7. **SLAM** - Haritalama ve lokalizasyon
8. **Navigasyon** - Otonom seyir
9. **Web Arayüzü** - Kullanıcı arayüzü geliştirme

## 🎓 Öğrenme Çıktıları

Bu projeyi tamamladığınızda:

- ✅ ROS2 temellerini öğrenmiş olacaksınız
- ✅ Robotik kinematik ve odometry anlayacaksınız
- ✅ SLAM algoritmalarını uygulayacaksınız
- ✅ Otonom navigasyon sistemleri geliştireceksiniz
- ✅ Web tabanlı robotik arayüzler oluşturacaksınız
- ✅ Gerçek zamanlı veri iletişimi kuracaksınız

## 🛠️ Teknoloji Stack

| Katman | Teknoloji |
|--------|-----------|
| **Hardware** | Raspberry Pi 5, Arduino, RPLidar A1 |
| **OS** | Ubuntu 24.04 Noble |
| **Framework** | ROS2 Jazzy Jalisco |
| **SLAM** | SLAM Toolbox |
| **Navigation** | Nav2 |
| **Communication** | Socket.IO, WebSocket |
| **Web** | React, Vite, JavaScript |
| **Language** | Python 3.10, C++, JavaScript |

## 📚 Kaynaklar

- [ROS2 Resmi Dokümantasyon](https://docs.ros.org/en/jazzy/)
- [Nav2 Dokümantasyon](https://navigation.ros.org/)
- [SLAM Toolbox](https://github.com/SteveMacenski/slam_toolbox)
- [RPLidar ROS](https://github.com/Slamtec/rplidar_ros)

## 🤝 Katkıda Bulunma

Bu proje açık kaynaklıdır ve katkılarınızı bekliyoruz! Detaylar için [Katkıda Bulunma Rehberi](extras/contributing.md) sayfasını ziyaret edin.

## 📞 İletişim

- **Website**: [pathoven.github.io](https://pathoven.github.io)
- **GitHub**: [github.com/pathoven](https://github.com/pathoven)

---

<div style="text-align: center; margin-top: 30px;">
  <p><strong>Hazır mısınız? Hadi başlayalım! 🚀</strong></p>
  <p><a href="tr/introduction/overview">📖 Proje Genel Bakış →</a></p>
</div>

