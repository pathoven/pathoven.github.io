# Arduino Temelleri

Arduino Nano Every, BTS7960 motor sürücüsünü ve direksiyon servosunu kontrol eder. Başlangıç için temel bir firmware yeterlidir.

## Firmware Yükleme

```bash
git clone https://github.com/pathoven/pathoven_firmware.git
cd pathoven_firmware
arduino-cli compile --fqbn arduino:megaavr:nona4809 .
arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:megaavr:nona4809 .
```

## Seri Protokol

| Alan | Açıklama | Örnek |
| --- | --- | --- |
| `CMD` | Mod (`MANUAL`, `AUTO`, `STOP`) | `CMD:MANUAL` |
| `STR` | Direksiyon PWM değeri (1000-2000 µs) | `STR:1500` |
| `THR` | Gaz PWM değeri | `THR:1520` |
| `HB` | Heartbeat sayacı | `HB:1234` |

ROS2 tarafında `/dev/ttyUSB0` portuna her satırı `\n` ile biten bu formatta paket yazmanız yeterli.

## Güvenlik Özellikleri

- Servo sınırları 1100–1900 µs aralığında tutulur.
- 200 ms boyunca komut gelmezse watchdog gazı sıfırlar.
- RC alıcısındaki manuel geçiş anahtarı Arduino çıkışını bypass edebilir.

## Sonraki Adım

Firmware çalıştıktan sonra Raspberry Pi üzerindeki [kurulum rehberine](#/tr/raspberrypi/setup) geri dönün.
