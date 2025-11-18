# Donanım Gereksinimleri

| Kategori | Bileşen | Notlar |
| --- | --- | --- |
| Hesaplama | Raspberry Pi 5 (8 GB) + 32 GB microSD | Ağır SLAM yükleri için pasif soğutucu veya fan şart |
| Kontrol | Arduino Nano Every + BTS7960 motor sürücüsü | 5 V PWM kabul eden benzer sürücüler kullanılabilir |
| Güç | 3S LiPo (5000 mAh), 5 V step-down regülatör | Logic devreler için ayrı bir besleme hattı ayırın |
| Sensörler | RPLidar A1/A2, BNO055/BNO086 IMU, geniş açılı kamera | RPLidar'ı yaklaşık 18 cm yüksekliğe yerleştirdik |
| Şasi | 1/10 RC şasi (örn. Redcat Lightning EPX) | Ackermann direksiyonlu ve 540 brushed motorlu olmalı |

## Kablolama Notları

- RPLidar USB kablosunu 1 metreden kısa tutun.
- Direksiyon PWM hattı için bükümlü servo kablosu kullanın.
- Raspberry Pi, Arduino ve motor sürücüsünün topraklarını mutlaka ortaklayın.
- IMU'yu aracın ağırlık merkezine yakın monte edin.

## Opsiyonel Yükseltmeler

- Intel RealSense D435 ile derinlik algılama
- Sahada kullanım için LTE yönlendirici
- Pil izleme için ayrı bir mikrodenetleyici

---

Ubuntu, ROS2 ve yardımcı araçları kurmak için [Yazılım Kurulumu](/tr/introduction/software) sayfasına geçin.
