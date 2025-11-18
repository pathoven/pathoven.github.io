# Raspberry Pi 5 Kurulumu

## 🎯 Bu Bölümde Yapacaklarımız

- Raspberry Pi 5'e Ubuntu 24.04 kurulumu
- İlk boot ve temel yapılandırma
- Network bağlantısı kurma
- Sistem güncellemeleri
- Gerekli araçların kurulumu

## 📋 Gereksinimler

**Donanım:**
- Raspberry Pi 5 (4GB veya 8GB)
- microSD kart (32GB+, Class 10 veya UHS-I)
- USB-C güç adaptörü (5V 5A önerilir)
- microSD kart okuyucu (PC için)
- HDMI kablo + monitör (ilk kurulum için)
- USB klavye ve mouse (ilk kurulum için)

**Yazılım:**
- Raspberry Pi Imager
- Ubuntu 24.04 Server veya Desktop image

## 🔧 Adım 1: Ubuntu 24.04 Image Hazırlama

### Raspberry Pi Imager ile (Önerilen)

1. **Raspberry Pi Imager'ı İndirin**
   - [rpi-imager download](https://www.raspberrypi.com/software/)
   - Windows, macOS, Linux için mevcut

2. **Imager'ı Başlatın**
   - microSD kartı bilgisayara takın
   - Raspberry Pi Imager'ı açın

3. **İşletim Sistemi Seçin**
   ```
   Choose OS → Other general-purpose OS → Ubuntu → 
   Ubuntu Server 24.04 LTS (64-bit)
   ```
   
   > **💡 İpucu:** Desktop sürümü daha fazla disk alanı kullanır ama GUI ile kurulum yapmak istiyorsanız tercih edebilirsiniz. Bu proje için **Server** önerilir (headless kullanacağız).

4. **Storage Seçin**
   - microSD kartınızı seçin
   - ⚠️ **Dikkat:** Karttaki tüm veriler silinecek!

5. **Advanced Options (⚙️ ikonu)**
   - `Set hostname`: `raspberrypi` (veya istediğiniz isim)
   - `Enable SSH`: ✅ (Use password authentication)
   - `Set username and password`:
     - Username: `ubuntu` (veya istediğiniz)
     - Password: Güçlü bir şifre
   - `Configure wireless LAN` (WiFi kullanacaksanız):
     - SSID: WiFi ağ adınız
     - Password: WiFi şifreniz
     - Wireless LAN country: TR
   - `Set locale settings`:
     - Time zone: Europe/Istanbul
     - Keyboard layout: tr (veya us)

6. **Write (Yaz)**
   - `Yes` diyerek onaylayın
   - İşlem 5-10 dakika sürer
   - Tamamlandığında kartı çıkarın

## 🚀 Adım 2: İlk Boot

### Donanım Bağlantısı

```
1. microSD kartı Raspberry Pi'ye takın
2. HDMI kablosunu bağlayın (monitöre)
3. USB klavye ve mouse'u bağlayın
4. Ethernet kablosunu bağlayın (WiFi yerine - daha stabil)
5. Güç kablosunu EN SON bağlayın
```

### İlk Açılış

1. **Güç Verildiğinde**
   - Yeşil LED yanıp sönmeli (disk aktivitesi)
   - İlk boot 1-2 dakika sürer
   - Cloud-init yapılandırması yapılır

2. **Login Ekranı**
   ```
   Ubuntu 24.04 LTS raspberrypi tty1
   
   raspberrypi login: ubuntu
   Password: [şifrenizi girin]
   ```

3. **İlk Login Sonrası**
   - Şifre değiştirme isteyebilir (cloud-init'e göre)
   - Sistem hazır!

## 🌐 Adım 3: Network Yapılandırması

### IP Adresini Öğrenme

```bash
# Tüm network interface'leri göster
ip addr show

# Veya sadece IP'yi göster
hostname -I
```

**Çıktı örneği:**
```
192.168.1.100  # Bu IP'yi not edin (SSH için)
```

### WiFi Bağlantısı (Ethernet kullanmıyorsanız)

Ubuntu 24.04'te network yönetimi **netplan** ile yapılır.

```bash
# Network yapılandırma dosyasını düzenle
sudo nano /etc/netplan/50-cloud-init.yaml
```

**Örnek yapılandırma:**
```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
  wifis:
    wlan0:
      dhcp4: true
      access-points:
        "WiFi_Aginiz":
          password: "WiFi_Sifreniz"
```

**Uygulamak için:**
```bash
sudo netplan apply

# Bağlantıyı kontrol et
ping -c 4 google.com
```

## 🔄 Adım 4: Sistem Güncellemeleri

Sistem güncellemelerini yapmak **çok önemli**:

```bash
# Paket listelerini güncelle
sudo apt update

# Yüklü paketleri yükselt
sudo apt upgrade -y

# Sistem araçlarını güncelle
sudo apt full-upgrade -y

# Kullanılmayan paketleri temizle
sudo apt autoremove -y

# Reboot (önerilir)
sudo reboot
```

Bu işlem 10-30 dakika sürebilir (bağlantıya göre).

## 🔐 Adım 5: SSH Yapılandırması

### SSH ile Bağlanma (PC'den)

**Linux / macOS:**
```bash
ssh ubuntu@192.168.1.100
# Şifrenizi girin
```

**Windows (PowerShell):**
```powershell
ssh ubuntu@192.168.1.100
```

**Veya PuTTY:**
- Host: `192.168.1.100`
- Port: `22`
- Connection type: SSH

### SSH Key ile Bağlanma (Daha Güvenli)

**PC'de (client):**
```bash
# SSH key oluştur (eğer yoksa)
ssh-keygen -t ed25519 -C "your_email@example.com"

# Public key'i Raspberry Pi'ye kopyala
ssh-copy-id ubuntu@192.168.1.100

# Artık şifresiz bağlanabilirsiniz
ssh ubuntu@192.168.1.100
```

### Hostname ile Bağlanma

IP yerine hostname kullanmak daha kolay:

```bash
# .local domain ile (mDNS)
ssh ubuntu@raspberrypi.local
```

> **💡 İpucu:** Windows'ta mDNS için [Bonjour Print Services](https://support.apple.com/kb/DL999) kurmanız gerekebilir.

## 🛠️ Adım 6: Temel Araçlar Kurulumu

```bash
# Geliştirme araçları
sudo apt install -y build-essential git cmake

# Python 3 araçları
sudo apt install -y python3-pip python3-dev

# Network araçları
sudo apt install -y net-tools curl wget

# Monitoring araçları
sudo apt install -y htop tmux

# Text editör (opsiyonel)
sudo apt install -y vim nano
```

## 📊 Adım 7: Sistem Bilgilerini Kontrol Etme

### CPU ve RAM

```bash
# CPU bilgisi
lscpu

# RAM kullanımı
free -h

# Disk kullanımı
df -h

# Sistem bilgisi
uname -a
```

### Sıcaklık Kontrolü

```bash
# CPU sıcaklığı
vcgencmd measure_temp

# Veya
cat /sys/class/thermal/thermal_zone0/temp
# Çıktı: 50000 (50.0°C anlamına gelir)
```

**Uygun sıcaklıklar:**
- Idle: 40-50°C
- Yük altında: 50-70°C
- ⚠️ >75°C: Thermal throttling başlar
- 🔥 >85°C: Tehlikeli, soğutma ekleyin!

### Voltaj ve Clock Kontrolü

```bash
# Voltaj
vcgencmd measure_volts

# CPU clock hızı
vcgencmd measure_clock arm

# Throttling durumu
vcgencmd get_throttled
# 0x0 = Normal
# Diğer değerler = Sorun var (düşük voltaj, sıcaklık vb.)
```

## 🔧 Adım 8: Performans Optimizasyonu

### Swap Boyutunu Artırma (8GB RAM'den azsa)

```bash
# Mevcut swap kontrolü
free -h

# Swap dosyasını büyüt (2GB örnek)
sudo dphys-swapfile swapoff
sudo nano /etc/dphys-swapfile
# CONF_SWAPSIZE=2048 olarak değiştirin

sudo dphys-swapfile setup
sudo dphys-swapfile swapon
```

### GPU Memory Azaltma (Headless için)

```bash
# Boot config düzenle
sudo nano /boot/firmware/config.txt

# Şunu ekleyin:
gpu_mem=16  # Minimum GPU memory (headless için yeterli)

# Kaydet ve reboot
sudo reboot
```

### Zabbix (Unnecessary Services) Kapatma

```bash
# Kullanılmayan servisleri listele
systemctl list-unit-files --state=enabled

# Örnek: Bluetooth kapatma (kullanmıyorsanız)
sudo systemctl disable bluetooth
sudo systemctl stop bluetooth
```

## 🔐 Adım 9: Güvenlik (Opsiyonel ama Önerilen)

### Firewall (UFW) Aktifleştirme

```bash
# UFW kur
sudo apt install -y ufw

# SSH'a izin ver (bağlantı kopmadan önce!)
sudo ufw allow 22/tcp

# Firewall'u aktifleştir
sudo ufw enable

# Durumu kontrol et
sudo ufw status
```

### Fail2Ban (Brute Force Koruması)

```bash
# Fail2ban kur
sudo apt install -y fail2ban

# Otomatik başlat
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

## ✅ Kurulum Kontrolü

Herşey hazır mı kontrol edin:

```bash
# SSH çalışıyor mu?
sudo systemctl status ssh

# Internet bağlantısı var mı?
ping -c 4 google.com

# Disk alanı yeterli mi? (En az 10GB boş olmalı)
df -h

# Sistem güncel mi?
sudo apt update && sudo apt list --upgradable

# Sıcaklık normal mi? (<70°C)
vcgencmd measure_temp
```

## 🎯 Sonraki Adımlar

Raspberry Pi 5 kurulumu tamamlandı! 🎉

Artık ROS2 kurulumuna geçebilirsiniz:

**Sonraki:** [ROS2 Jazzy Kurulumu →](ros2-install.md)

---

## 🐛 Sorun Giderme

### Problem: SSH bağlanamıyor

**Çözümler:**
```bash
# 1. SSH servisi çalışıyor mu? (RPi'de)
sudo systemctl status ssh
sudo systemctl start ssh

# 2. Firewall SSH'a izin veriyor mu?
sudo ufw allow 22/tcp

# 3. IP doğru mu?
hostname -I

# 4. mDNS çalışıyor mu?
sudo systemctl status avahi-daemon
```

### Problem: WiFi bağlanmıyor

```bash
# Network interface'leri kontrol et
ip link show

# WiFi aktif mi?
sudo rfkill list all
# Eğer "blocked" ise:
sudo rfkill unblock wifi

# Netplan yeniden uygula
sudo netplan apply
```

### Problem: Sistem yavaş

```bash
# CPU kullanımını kontrol et
htop

# Sıcaklık kontrolü
vcgencmd measure_temp

# Throttling var mı?
vcgencmd get_throttled

# RAM dolmuş mu?
free -h
```

---

**💡 İpucu:** Artık monitör, klavye ve mouse'a ihtiyacınız yok! SSH ile her şeyi yapabilirsiniz.

