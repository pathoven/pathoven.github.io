# Yazılım Kurulumu

## İşletim Sistemi

### Ubuntu 24.04 Noble (Önerilen)
- ROS2 Jazzy için resmi destek sunar.
- Raspberry Pi 5 üzerinde en güncel kernel optimizasyonlarına sahiptir.
- 5 yıl süren LTS (Long Term Support) desteği sağlar.

#### Kurulum Adımları
```bash
# Ubuntu 24.04 imajını indir
# Raspberry Pi Imager ile SD karta yaz
```

## ROS2 Jazzy Jalisco

### Neden Jazzy?
- En yeni LTS ROS2 sürümü.
- Modern C++17 ve Python 3.10 desteği.
- Nav2 ve SLAM Toolbox paketleriyle tam uyum.

### Kurulum
```bash
sudo apt update && sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt install ros-jazzy-desktop \
                 ros-jazzy-navigation2 \
                 ros-jazzy-slam-toolbox \
                 python3-colcon-common-extensions \
                 python3-rosdep
```

## Python Paketleri

```bash
pip3 install --user pyserial python-socketio aiohttp psutil pytest
```

## Node.js ve Web Araçları

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

cd ~/bozaw_web_socketio
npm install
```

## Geliştirme Araçları (Opsiyonel)

```bash
sudo apt install git
snap install code --classic
sudo apt install terminator htop
```

## Donanım Sürücüleri

```bash
cd ~/ros2_ws/src
git clone -b ros2 https://github.com/Slamtec/rplidar_ros.git

sudo usermod -a -G dialout $USER
# Çıkış yapıp tekrar giriş yapın
```

## Özet Kontrol Listesi

- [ ] Ubuntu 24.04 kuruldu.
- [ ] ROS2 Jazzy paketleri yüklendi.
- [ ] Python ve Node.js bağımlılıkları tamamlandı.
- [ ] RPLidar sürücüsü workspace'e klonlandı.
- [ ] Kullanıcı `dialout` grubuna eklendi.

---

Şimdi [Proje Hedefleri](/tr/introduction/goals) sayfasına geçebilirsiniz.
