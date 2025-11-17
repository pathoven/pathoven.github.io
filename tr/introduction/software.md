# Yazılım Gereksinimleri

## İşletim Sistemi

### Ubuntu 24.04 Noble (Önerilen)
- ROS2 Jazzy için resmi destek
- Raspberry Pi 5 için optimize
- LTS (Long Term Support) sürüm

### Kurulum
```bash
# Ubuntu 24.04 image indir
# Raspberry Pi Imager ile SD karta yaz
```

## ROS2 Jazzy Jalisco

### Neden ROS2 Jazzy?
- En güncel LTS sürüm
- Ubuntu 24.04 ile tam uyumlu
- Modern C++17 ve Python 3.10+ desteği
- Nav2 ve SLAM Toolbox tam desteği

### Kurulum
```bash
# ROS2 repository ekle
sudo apt update && sudo apt install software-properties-common
sudo add-apt-repository universe

# ROS2 Jazzy kur
sudo apt install ros-jazzy-desktop
sudo apt install ros-jazzy-navigation2
sudo apt install ros-jazzy-slam-toolbox

# Geliştirme araçları
sudo apt install python3-colcon-common-extensions
sudo apt install python3-rosdep
```

## Python Paketleri

```bash
# Temel paketler
pip3 install --user pyserial
pip3 install --user python-socketio
pip3 install --user aiohttp
pip3 install --user psutil

# Test araçları
pip3 install --user pytest
```

## Node.js ve Web Araçları

```bash
# Node.js 18+ kur
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Web GUI bağımlılıkları
cd ~/bozaw_web_socketio
npm install
```

## Geliştirme Araçları (Opsiyonel)

```bash
# Git
sudo apt install git

# VS Code
snap install code --classic

# Terminator (çoklu terminal)
sudo apt install terminator

# Htop (sistem izleme)
sudo apt install htop
```

## Donanım Sürücüleri

### RPLidar
```bash
cd ~/ros2_ws/src
git clone -b ros2 https://github.com/Slamtec/rplidar_ros.git
```

### Arduino Serial
- Arduino IDE veya PlatformIO
- `dialout` grubuna kullanıcı ekle:
```bash
sudo usermod -a -G dialout $USER
# Logout/login gerekli
```

## Özet Kontrol Listesi

- [ ] Ubuntu 24.04 Noble kurulu
- [ ] ROS2 Jazzy kurulu
- [ ] Python 3.10+ ve gerekli paketler
- [ ] Node.js 18+ kurulu
- [ ] Git yapılandırıldı
- [ ] RPLidar sürücüsü klonlandı
- [ ] Serial port izinleri ayarlandı

**Sonraki:** [Proje Hedefleri →](#/tr/introduction/goals)

