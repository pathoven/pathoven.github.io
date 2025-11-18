# Arduino ile Basit Kodlama

## 🎯 Bu Bölümde Öğrenecekleriniz

- Arduino IDE kurulumu ve yapılandırması
- Temel Arduino programlama kavramları
- Serial iletişim temelleri
- LED ve buton kontrolü (Hello World!)

## 📋 Gereksinimler

- Arduino Uno veya Nano
- USB kablosu (A to B veya A to Mini-USB)
- Breadboard (deney tahtası)
- LED (herhangi bir renk)
- 220Ω direnç
- Jumper kablolar

## 🔧 Arduino IDE Kurulumu

### Windows

1. [Arduino.cc](https://www.arduino.cc/en/software) adresinden Arduino IDE'yi indirin
2. İndirilen `.exe` dosyasını çalıştırın
3. Kurulum talimatlarını takip edin
4. Arduino IDE'yi başlatın

### Linux (Ubuntu/Debian)

```bash
# Snap ile kurulum (önerilen)
sudo snap install arduino

# Veya apt ile
sudo apt update
sudo apt install arduino

# USB izinlerini ayarla
sudo usermod -a -G dialout $USER
# Logout/login yapın
```

### macOS

```bash
# Homebrew ile
brew install arduino-cli

# Veya .dmg dosyasını indirin
# https://www.arduino.cc/en/software
```

## 🚀 İlk Program: LED Yakma (Blink)

### Donanım Bağlantısı

```
Arduino Pin 13 ──► LED (+) uzun bacak
LED (-) kısa bacak ──► 220Ω direnç ──► GND
```

> **💡 İpucu:** Arduino Uno'da dahili bir LED vardır (Pin 13'e bağlı). Harici LED olmadan da test edebilirsiniz!

### Kod

```cpp
/*
 * Blink - LED yakıp söndürme
 * En temel Arduino programı
 */

// Pin tanımlaması
const int LED_PIN = 13;

// Setup fonksiyonu - Bir kere çalışır (başlangıçta)
void setup() {
  // Pin 13'ü çıkış olarak ayarla
  pinMode(LED_PIN, OUTPUT);
}

// Loop fonksiyonu - Sürekli tekrar eder
void loop() {
  digitalWrite(LED_PIN, HIGH);   // LED'i yak
  delay(1000);                   // 1 saniye bekle
  digitalWrite(LED_PIN, LOW);    // LED'i söndür
  delay(1000);                   // 1 saniye bekle
}
```

### Yükleme Adımları

1. Arduino'yu USB ile bilgisayara bağlayın
2. **Tools → Board** → Arduino Uno (veya kullandığınız model)
3. **Tools → Port** → COM3 (Windows) veya /dev/ttyUSB0 (Linux)
4. **Upload** butonuna basın (➡️ ok simgesi)
5. LED'in yanıp söndüğünü görmelisiniz!

## 📡 Serial İletişim

Serial port, Arduino ile bilgisayar arasında veri alışverişi yapmayı sağlar.

### Basit Serial Çıktı

```cpp
void setup() {
  // Serial iletişimi başlat (115200 baud)
  Serial.begin(115200);
  Serial.println("Arduino başlatıldı!");
}

void loop() {
  Serial.print("Çalışma süresi: ");
  Serial.print(millis());  // Başlangıçtan beri geçen ms
  Serial.println(" ms");
  
  delay(1000);  // Her saniye yazdır
}
```

**Serial Monitor'u Açma:**
- **Tools → Serial Monitor** (veya Ctrl+Shift+M)
- Baud rate'i **115200** olarak ayarlayın
- Arduino'dan gelen mesajları göreceksiniz

### Serial Input (Veri Okuma)

```cpp
void setup() {
  Serial.begin(115200);
  pinMode(13, OUTPUT);
  Serial.println("Komutlar: ON, OFF");
}

void loop() {
  // Seri porttan veri geldi mi?
  if (Serial.available() > 0) {
    String command = Serial.readStringUntil('\n');
    command.trim();  // Boşlukları temizle
    
    if (command == "ON") {
      digitalWrite(13, HIGH);
      Serial.println("LED açıldı");
    } 
    else if (command == "OFF") {
      digitalWrite(13, LOW);
      Serial.println("LED kapatıldı");
    }
    else {
      Serial.println("Geçersiz komut!");
    }
  }
}
```

**Test Etme:**
1. Serial Monitor'u açın
2. Üstteki text kutusuna `ON` yazın ve Enter'a basın
3. LED yanmalı
4. `OFF` yazın, LED sönmeli

## 🔘 Buton Okuma

### Donanım Bağlantısı

```
Arduino 5V ──► Buton bir bacak
Buton diğer bacak ──► Pin 2
Pin 2 ──► 10kΩ direnç ──► GND (pull-down)
```

### Kod

```cpp
const int BUTTON_PIN = 2;
const int LED_PIN = 13;

void setup() {
  pinMode(BUTTON_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  // Buton durumunu oku
  int buttonState = digitalRead(BUTTON_PIN);
  
  if (buttonState == HIGH) {
    digitalWrite(LED_PIN, HIGH);
    Serial.println("Buton basılı - LED açık");
  } 
  else {
    digitalWrite(LED_PIN, LOW);
    Serial.println("Buton serbest - LED kapalı");
  }
  
  delay(100);  // Debounce için kısa bekleme
}
```

## 📝 Arduino Kod Yapısı

### Temel Yapı

```cpp
// 1. Kütüphane dahil etme (varsa)
#include <Servo.h>

// 2. Global değişkenler ve sabitler
const int LED_PIN = 13;
int counter = 0;

// 3. Setup fonksiyonu (bir kez çalışır)
void setup() {
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(115200);
}

// 4. Loop fonksiyonu (sürekli döngü)
void loop() {
  // Ana kodunuz buraya
  counter++;
  Serial.println(counter);
  delay(1000);
}

// 5. Yardımcı fonksiyonlar (opsiyonel)
void myFunction() {
  // Custom işlemler
}
```

### Önemli Fonksiyonlar

| Fonksiyon | Açıklama | Örnek |
|-----------|----------|-------|
| `pinMode(pin, mode)` | Pin modunu ayarla | `pinMode(13, OUTPUT)` |
| `digitalWrite(pin, value)` | Dijital pin yaz | `digitalWrite(13, HIGH)` |
| `digitalRead(pin)` | Dijital pin oku | `int val = digitalRead(2)` |
| `analogRead(pin)` | Analog pin oku (0-1023) | `int val = analogRead(A0)` |
| `analogWrite(pin, value)` | PWM yaz (0-255) | `analogWrite(9, 128)` |
| `delay(ms)` | Milisaniye bekle | `delay(1000)` |
| `millis()` | Başlangıçtan beri ms | `unsigned long t = millis()` |
| `Serial.begin(baud)` | Serial başlat | `Serial.begin(115200)` |
| `Serial.print(data)` | Serial yazdır | `Serial.print("Hello")` |
| `Serial.println(data)` | Serial yazdır + yeni satır | `Serial.println(123)` |

## 🎯 Pratik Egzersizler

### Egzersiz 1: Farklı Hızlarda Yanıp Sönen LED

LED'i 3 farklı hızda yanıp söndürün:
- Hızlı: 200ms açık, 200ms kapalı
- Normal: 500ms açık, 500ms kapalı
- Yavaş: 1000ms açık, 1000ms kapalı

```cpp
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
  // Hızlı
  for(int i = 0; i < 5; i++) {
    digitalWrite(13, HIGH);
    delay(200);
    digitalWrite(13, LOW);
    delay(200);
  }
  
  // Normal
  // ... kodunuzu yazın
  
  // Yavaş
  // ... kodunuzu yazın
}
```

### Egzersiz 2: Serial Kontrollü Parlaklık

Serial'dan 0-255 arası değer alın ve LED parlaklığını ayarlayın (PWM).

```cpp
void setup() {
  Serial.begin(115200);
  pinMode(9, OUTPUT);  // PWM pin
  Serial.println("0-255 arası değer girin:");
}

void loop() {
  if (Serial.available() > 0) {
    int brightness = Serial.parseInt();
    
    // Değer kontrolü
    if (brightness >= 0 && brightness <= 255) {
      analogWrite(9, brightness);
      Serial.print("Parlaklık: ");
      Serial.println(brightness);
    } else {
      Serial.println("Hata: 0-255 arası olmalı!");
    }
  }
}
```

### Egzersiz 3: Buton ile Sayaç

Her buton basışında sayacı artırın ve Serial'a yazdırın.

**İpucu:** Debounce mantığını kullanın (son basıştan 50ms geçti mi?)

## 🐛 Sık Karşılaşılan Hatalar

### 1. "Port not found" / "Port bulunamadı"

**Çözüm:**
```bash
# Linux'ta USB izinlerini kontrol et
ls -l /dev/ttyUSB0
sudo chmod 666 /dev/ttyUSB0  # Geçici çözüm
sudo usermod -a -G dialout $USER  # Kalıcı çözüm (logout gerekir)

# Windows'ta Device Manager'da Arduino'yu kontrol et
```

### 2. "avrdude: stk500_recv(): programmer is not responding"

**Çözümler:**
- USB kabloyu çıkarıp takın
- Farklı USB portu deneyin
- Arduino IDE'yi yeniden başlatın
- Doğru board seçilmiş mi kontrol edin

### 3. LED yanmıyor

**Kontrol edin:**
- LED'in yönü doğru mu? (uzun bacak +, kısa bacak -)
- Direnç bağlandı mı?
- Pin numarası kodla aynı mı?
- `pinMode(pin, OUTPUT)` yapıldı mı?

## 📚 Önerilen Kaynaklar

- [Arduino Official Tutorials](https://www.arduino.cc/en/Tutorial/HomePage)
- [Arduino Reference](https://www.arduino.cc/reference/en/)
- [Arduino Forum](https://forum.arduino.cc/)

## ✅ Özet

Bu bölümde öğrendikleriniz:
- ✅ Arduino IDE kurulumu
- ✅ LED kontrol (dijital çıkış)
- ✅ Serial iletişim (input/output)
- ✅ Buton okuma (dijital giriş)
- ✅ Temel Arduino kod yapısı

---

**Sonraki Adım:** [Servo Kontrolü →](servo.md)

