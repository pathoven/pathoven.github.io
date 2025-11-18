# Proje Genel Bakış

Pathoven Drive, ROS2 Jazzy ile çalışan küçük ölçekli bir Ackermann araç platformudur. Dokümantasyonun amacı ekip dışındaki kişilerin de aynı donanım ve yazılım yığınını zahmetsizce kurabilmesidir.

## Mimari Özeti

| Katman | Bileşenler | Notlar |
| --- | --- | --- |
| Algılama | RPLidar A1/A2, IMU, kamera | SLAM için 2B taramalar ve yakın engel tespiti |
| Hesaplama | Raspberry Pi 5 (8 GB) | Ubuntu 24.04, ROS2 düğümleri ve web panosu |
| Kontrol | Arduino Nano Every | Motor sürücüsü, direksiyon servosu ve failsafe PWM geçişi |
| Haberleşme | Ethernet + Wi-Fi | Gecikmeyi azaltmak için Ethernet, uzaktan erişim için Wi-Fi |

## Öne Çıkan Özellikler

- Joystick veya web arayüzü ile gerçek zamanlı teleoperasyon
- SLAM Toolbox ile harita oluşturma ve saklama
- Nav2 tabanlı otonom waypoint navigasyonu
- CPU, sıcaklık ve voltajı gösteren tanılama panosu

## Dokümantasyon Prensipleri

1. **Pratik** – Her sayfa uygulanabilir görevlerle biter.
2. **Donanım öncelikli** – Yeni yazılım eklemeden önce tüm sensörleri doğruluyoruz.
3. **İki dilli** – Türkçe ve İngilizce içerikler aynı adımları anlatır.

---

Parça listesine geçmek için [Donanım Gereksinimleri](#/tr/introduction/hardware) sayfasını ziyaret edin.
