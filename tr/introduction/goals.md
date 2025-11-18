# Proje Hedefleri

Bu proje, ROS2 ekosistemini uçtan uca öğrenebilmeniz için planlandı. Her aşama, araca yeni bir yetenek kazandırırken size ölçülebilir bir teknik kazanım sağlar.

## Öğrenme Hedefleri

### 1. Temeller
- ROS2 düğüm grafikleri, topic, service ve parameter yapısını kavrayın.
- Colcon ile tekrar üretilebilir workspace'ler oluşturun.
- Dokümantasyon ve firmware için Git tabanlı akışları uygulayın.

### 2. Gömülü Kontrol
- Arduino üzerinde motor sürme ve encoder okuma kodlarını yazın.
- IMU ve encoder verisini seri hat üzerinden ROS2 topiclerine aktarın.
- İletişim koptuğunda aracı güvenli şekilde durduran watchdog'lar tasarlayın.

### 3. Algılama ve Navigasyon
- SLAM Toolbox'ı iç ve dış mekânda kararlı çalışacak şekilde yapılandırın.
- Nav2 planlayıcılarıyla (Smac, Theta*) waypoint görevleri oluşturun.
- LiDAR engel verisini kullanarak lokal costmap ayarlarını optimize edin.

### 4. Operasyon
- Diagnostics tabanlı sağlık panoları oluşturun.
- Log alma ve rosbag kayıt süreçlerini otomatikleştirin.
- Stack'i SSH ve tmux ile başsız olarak dağıtın.

## Kilometre Taşları

| Faz | Hedef | Ölçülebilir Sonuç |
| --- | --- | --- |
| MVP | Manuel sürüş + canlı LiDAR | Web arayüzü aracı hareket ettirir ve taramaları gösterir |
| Haritalama | Tutarlı SLAM haritaları | Kaydedilen harita yeniden yüklendiğinde <5 cm sapma |
| Otonomi | Waypoint navigasyonu | Nav2 iç mekânda üç noktalı görevi tamamlar |
| Güvenilirlik | Saha kullanımı | Sağlık paneli 30 dk testte yeşil kalır |

## Kontrol Listesi

- [ ] Araç dashboard üzerinden arm/disarm ediliyor.
- [ ] RPLidar en az 3 Hz hızla ve <%2 kayıpla yayın yapıyor.
- [ ] EKF, 50 Hz hızında birleştirilmiş odometri üretiyor.
- [ ] Nav2 kare rotayı iki kez müdahalesiz tamamlıyor.
- [ ] Tüm düğümler systemd servisleriyle kendini yeniden başlatıyor.

## Önerilen Çalışma Sırası

1. Aracı fiziksel olarak toplayın ve motor yönlerini doğrulayın.
2. Yazılım kurulumunu yapıp ROS2 workspace'ini ayağa kaldırın.
3. Küçük bir alanda SLAM çalıştırıp harita dışa aktarın.
4. Direksiyon servosu için PID değerlerini ayarlayın.
5. Çoklu waypoint görevleri gibi üst seviye davranışlar ekleyin.

---

Sırada [Arduino Temelleri](#/tr/arduino/basics) sayfası var; kontrol yazılımını buradan yükleyin.
