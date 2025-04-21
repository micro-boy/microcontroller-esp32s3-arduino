# 📘 Modul 2: Dasar-Dasar Input/Output dengan ESP32-S3

<div align="center">

  <img src="https://github.com/user-attachments/assets/fa8faa43-a763-4df4-8d60-5a4fa869de7d" width="400">
  
  <p><em>ESP32-S3 Development Board - Kendalikan Dunia Digital dan Analog</em></p>
  
  <p>
    <img src="https://img.shields.io/badge/Tingkat-Pemula_ke_Menengah-yellow" alt="Tingkat: Pemula ke Menengah">
    <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
    <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
  </p>
</div>

## 📋 Daftar Isi

- [Tujuan Pembelajaran](#-tujuan-pembelajaran)
- [Kendali GPIO Digital](#-kendali-gpio-digital)
  - [Prinsip Dasar GPIO](#prinsip-dasar-gpio)
  - [Mode Input dan Output](#mode-input-dan-output)
  - [Praktikum 1: Tombol dan LED](#praktikum-1-tombol-dan-led)
  - [Pull-up dan Pull-down Resistor](#pull-up-dan-pull-down-resistor)
  - [Debouncing](#debouncing)
  - [Praktikum 2: Lampu Lalu Lintas Sederhana](#praktikum-2-lampu-lalu-lintas-sederhana)
- [Pemrosesan Sinyal Analog](#-pemrosesan-sinyal-analog)
  - [ADC (Analog-to-Digital Converter)](#adc-analog-to-digital-converter)
  - [DAC (Digital-to-Analog Converter)](#dac-digital-to-analog-converter)
  - [PWM (Pulse Width Modulation)](#pwm-pulse-width-modulation)
  - [Praktikum 3: Pengukur Cahaya dengan LDR](#praktikum-3-pengukur-cahaya-dengan-ldr)
  - [Praktikum 4: Kontrol Kecerahan LED](#praktikum-4-kontrol-kecerahan-led)
- [Proyek Mini: Lampu Otomatis](#-proyek-mini-lampu-otomatis)
- [Latihan Mandiri](#-latihan-mandiri)
- [Troubleshooting Umum](#-troubleshooting-umum)
- [Sumber Referensi](#-sumber-referensi)

---

## 🎯 Tujuan Pembelajaran

Setelah mempelajari modul ini, Anda diharapkan dapat:

1. Memahami konsep dasar GPIO (General Purpose Input/Output) pada microcontroller
2. Menggunakan pin digital ESP32-S3 untuk operasi input dan output
3. Menerapkan teknik debouncing untuk tombol dan switch
4. Menggunakan ADC untuk membaca sinyal analog
5. Menghasilkan sinyal analog dengan DAC dan PWM
6. Mengimplementasikan kontrol LED dengan berbagai metode
7. Menghubungkan sensor analog sederhana dan membaca nilainya
8. Membangun proyek sederhana yang menggabungkan input dan output digital dan analog

---

## 🖥️ Kendali GPIO Digital

### Prinsip Dasar GPIO

GPIO (General Purpose Input/Output) adalah pin serbaguna pada microcontroller yang dapat dikonfigurasi sebagai input atau output digital. GPIO adalah antarmuka dasar yang memungkinkan microcontroller berinteraksi dengan dunia luar.

<div align="center">
  <img src="https://github.com/user-attachments/assets/55ff7c62-fd76-4be3-842a-e0c6976584ca" width="500">
  <p><em>Konsep GPIO sebagai Jembatan antara Microcontroller dan Dunia Luar</em></p>
</div>

**Karakteristik GPIO pada ESP32-S3:**

- ESP32-S3 memiliki 45 pin GPIO (GPIO0 - GPIO48, beberapa nomor tidak digunakan)
- Bekerja pada level tegangan 3.3V
- Sebagian besar pin dapat digunakan sebagai input atau output digital
- Beberapa pin memiliki fungsi khusus (contoh: pin boot, ADC, DAC, dll.)
- Beberapa pin memiliki pull-up/pull-down internal
- Arus maksimum yang disarankan: 40mA per pin

> ⚠️ **Penting**: Tidak semua pin GPIO tersedia pada modul ESP32-S3 DevKit. Beberapa pin digunakan untuk komponen pada board itu sendiri seperti flash memory dan USB. Selalu periksa pinout diagram board Anda.

### Mode Input dan Output

GPIO dapat dikonfigurasi dalam beberapa mode berbeda sesuai kebutuhan aplikasi.

**Mode Output:**

Pada mode output, pin dikontrol oleh microcontroller untuk mengirim sinyal keluar:

- **OUTPUT**: Mengatur pin sebagai output push-pull standar
  - Dapat menghasilkan level tegangan HIGH (3.3V) atau LOW (0V)
  - Digunakan untuk mengontrol LED, relay, transistor, dll.

```cpp
pinMode(13, OUTPUT);    // Mengatur pin 13 sebagai output
digitalWrite(13, HIGH); // Mengatur pin 13 ke level tegangan HIGH (3.3V)
digitalWrite(13, LOW);  // Mengatur pin 13 ke level tegangan LOW (0V)
```

**Mode Input:**

Pada mode input, pin membaca sinyal dari luar:

- **INPUT**: Mode input dasar
  - Pin berada dalam keadaan high-impedance (mengambang)
  - Dapat membaca tegangan HIGH atau LOW dari sumber eksternal
  - Tidak memiliki resistor pull-up atau pull-down internal

- **INPUT_PULLUP**: Mode input dengan resistor pull-up internal
  - Mengaktifkan resistor pull-up internal (~10kΩ)
  - Tanpa koneksi eksternal, pin secara default berada pada level HIGH
  - Berguna untuk membaca tombol/switch tanpa resistor eksternal

- **INPUT_PULLDOWN**: Mode input dengan resistor pull-down internal
  - Mengaktifkan resistor pull-down internal (~10kΩ)
  - Tanpa koneksi eksternal, pin secara default berada pada level LOW
  - Tersedia pada ESP32-S3 (tidak semua microcontroller memiliki fitur ini)

```cpp
pinMode(4, INPUT);         // Mode input dasar
pinMode(5, INPUT_PULLUP);  // Mode input dengan pull-up internal
pinMode(6, INPUT_PULLDOWN); // Mode input dengan pull-down internal

// Membaca nilai dari pin input
int val1 = digitalRead(4); // Membaca nilai dari pin 4 (HIGH atau LOW)
int val2 = digitalRead(5); // Membaca nilai dari pin 5 (HIGH atau LOW)
```

### Praktikum 1: Tombol dan LED

Mari kita buat rangkaian sederhana yang menghubungkan tombol (push button) dan LED dengan ESP32-S3. LED akan menyala ketika tombol ditekan.

**Komponen yang Dibutuhkan:**
1. ESP32-S3 DevKit
2. LED (warna sesuai pilihan)
3. Resistor 220Ω (untuk LED)
4. Push button
5. Breadboard
6. Kabel jumper

**Rangkaian:**

<div align="center">
  <img src="https://github.com/user-attachments/assets/3096e922-2d65-4fcb-b5c2-c80bb57e1b9d" width="500">
  <p><em>Rangkaian Tombol dan LED dengan ESP32-S3</em></p>
</div>

1. Hubungkan LED:
   - Kaki anoda (panjang) ke resistor 220Ω
   - Resistor ke pin GPIO10 ESP32-S3
   - Kaki katoda (pendek) ke GND

2. Hubungkan tombol:
   - Satu kaki tombol ke pin GPIO4 ESP32-S3
   - Kaki lainnya ke GND

**Simulasi Online:**

📱 **Simulasi Online**: [Coba rangkaian ini di Wokwi](https://wokwi.com/projects/428248754333770753)

⚠️ **Catatan**: Simulasi online memberikan gambaran dasar tentang cara kerja rangkaian, tetapi pengalaman dengan hardware asli mungkin sedikit berbeda. Jika tidak dapat mengakses simulasi, praktikum ini dapat dilakukan langsung dengan hardware di laboratorium.

**Kode Program:**

```cpp
/*
 * Praktikum 1: Tombol dan LED
 * Fungsi: Menyalakan LED ketika tombol ditekan
 * Platform: ESP32-S3
 */

// Definisi pin
const int BUTTON_PIN = 4;  // Pin untuk tombol
const int LED_PIN = 10;    // Pin untuk LED

// Variabel untuk menyimpan status tombol
int buttonState = 0;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  delay(1000);  // Jeda agar Serial Monitor siap
  
  // Konfigurasikan pin
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Input dengan pull-up internal
  pinMode(LED_PIN, OUTPUT);           // Output untuk LED
  
  Serial.println("Program Tombol dan LED dimulai!");
  Serial.println("Tekan tombol untuk menyalakan LED");
}

void loop() {
  // Baca status tombol
  buttonState = digitalRead(BUTTON_PIN);
  
  // Logika: INPUT_PULLUP membuat pin HIGH ketika tombol tidak ditekan
  // dan LOW ketika tombol ditekan
  if (buttonState == LOW) {
    // Tombol ditekan
    digitalWrite(LED_PIN, HIGH);  // Nyalakan LED
    Serial.println("Tombol ditekan - LED menyala");
  } else {
    // Tombol tidak ditekan
    digitalWrite(LED_PIN, LOW);   // Matikan LED
    Serial.println("Tombol dilepas - LED mati");
  }
  
  // Tambahkan sedikit jeda untuk stabilitas
  delay(100);
}
```

**Penjelasan Kode:**
1. Kita mendefinisikan pin 4 sebagai input tombol dan pin 10 sebagai output LED
2. Pin tombol dikonfigurasi sebagai `INPUT_PULLUP` sehingga tidak memerlukan resistor pull-up eksternal
3. Dalam loop, kita membaca status tombol dengan `digitalRead()`
4. Ketika tombol ditekan, pin akan terbaca LOW karena terhubung ke GND
5. Berdasarkan status tombol, kita mengontrol LED dengan `digitalWrite()`

> 💡 **Catatan**: Perhatikan penggunaan INPUT_PULLUP yang membuat logika terbalik: LOW saat ditekan dan HIGH saat dilepas. Ini adalah pola umum ketika bekerja dengan tombol pada microcontroller.

### Pull-up dan Pull-down Resistor

Saat bekerja dengan input digital, pemahaman tentang resistor pull-up dan pull-down sangat penting.

<div align="center">
  <img src="https://github.com/user-attachments/assets/2f130dbf-b4e0-4800-97a3-3d221236e966" width="650">
  <p><em>Konfigurasi Rangkaian Pull-up dan Pull-down</em></p>
</div>

**Pull-up Resistor:**
- Resistor yang menghubungkan pin ke VCC (3.3V)
- Membuat pin berada pada level HIGH ketika tidak ada sinyal eksternal
- Ketika tombol/switch ditekan (terhubung ke GND), pin akan menjadi LOW
- ESP32-S3 memiliki pull-up internal (~10kΩ) yang dapat diaktifkan dengan `INPUT_PULLUP`

**Pull-down Resistor:**
- Resistor yang menghubungkan pin ke GND (0V)
- Membuat pin berada pada level LOW ketika tidak ada sinyal eksternal
- Ketika tombol/switch ditekan (terhubung ke VCC), pin akan menjadi HIGH
- ESP32-S3 memiliki pull-down internal yang dapat diaktifkan dengan `INPUT_PULLDOWN`

**Mengapa kita membutuhkan pull-up/pull-down?**
- Tanpa pull-up/pull-down, pin input berada dalam keadaan "mengambang" (floating)
- Pin mengambang sangat rentan terhadap noise elektrik dan dapat menghasilkan nilai acak
- Pull-up/pull-down memberikan nilai default yang stabil ketika tidak ada sinyal eksternal

**Rangkaian Alternatif:**

Jika Anda tidak ingin menggunakan resistor internal, Anda dapat membuat rangkaian dengan resistor eksternal:

```
// Konfigurasi dengan pull-up eksternal (resistor 10kΩ ke 3.3V)
pinMode(4, INPUT);

// Konfigurasi dengan pull-down eksternal (resistor 10kΩ ke GND)
pinMode(4, INPUT);
```

### Debouncing

Ketika bekerja dengan tombol/switch mekanis, Anda akan menghadapi fenomena yang disebut "bouncing" - fenomena di mana kontak mekanis menghasilkan beberapa pulsa pada saat transisi, alih-alih satu pulsa bersih.

<div align="center">
  <img src="https://github.com/user-attachments/assets/0d9715cf-a877-41bc-af60-0e2d0d94651a" width="500">
  <p><em>Fenomena Bouncing pada Tombol Mekanis</em></p>
</div>

**Efek Bouncing:**
- Saat tombol ditekan atau dilepas, kontak mekanis bergetar
- Getaran menyebabkan beberapa transisi HIGH-LOW dalam waktu singkat (milidetik)
- Microcontroller membaca ini sebagai beberapa penekanan tombol
- Hal ini menyebabkan perilaku tidak diinginkan seperti penghitungan ganda atau perubahan status berulang

**Teknik Debouncing:**

1. **Debouncing Software (Delay):**
   Metode paling sederhana adalah menambahkan delay setelah perubahan terdeteksi.

```cpp
// Contoh debouncing sederhana dengan delay
if (digitalRead(BUTTON_PIN) == LOW) {  // Tombol ditekan
  digitalWrite(LED_PIN, HIGH);         // Nyalakan LED
  delay(50);                          // Tunggu 50ms untuk bouncing selesai
}
```

2. **Debouncing dengan Timer:**
   Metode yang lebih canggih adalah memeriksa apakah sudah berlalu waktu tertentu sejak perubahan terakhir.

```cpp
// Debouncing dengan timestamp
unsigned long lastDebounceTime = 0;  // Waktu terakhir pin berubah
unsigned long debounceDelay = 50;    // Waktu debounce dalam ms
int lastButtonState = HIGH;          // Status tombol sebelumnya
int buttonState = HIGH;              // Status tombol saat ini
int LED_PIN = 17;
int BUTTON_PIN = 1;

void setup(){
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {
  // Baca status tombol saat ini
  int reading = digitalRead(BUTTON_PIN);

  // Periksa apakah ada perubahan status
  if (reading != lastButtonState) {
    // Reset timer debounce
    lastDebounceTime = millis();
  }

  // Periksa apakah sudah lewat waktu debounce yang cukup
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika status pin stabil selama debounceDelay
    // dan berubah dari status sebelumnya, update buttonState
    if (reading != buttonState) {
      buttonState = reading;

      // Hanya ubah status LED jika buttonState LOW (tombol ditekan)
      if (buttonState == LOW) {
        // Toggle status LED
        digitalWrite(LED_PIN, !digitalRead(LED_PIN));
      }
    }
  }

  // Simpan pembacaan saat ini untuk perbandingan di iterasi berikutnya
  lastButtonState = reading;
}
```

📱 **Simulasi Online**: [Coba rangkaian ini di Wokwi](https://wokwi.com/projects/428249660805757953)

> 💡 **Tip**: Pilih metode debouncing sesuai dengan kebutuhan aplikasi Anda. Metode delay sederhana umumnya cukup untuk aplikasi sederhana, tetapi metode dengan timestamp atau library lebih baik untuk aplikasi yang memerlukan responsivitas tinggi.

3. **Debouncing dengan Library:**
   Untuk pendekatan yang lebih robust, Anda bisa menggunakan library seperti Bounce2.

```cpp
#include <Bounce2.h>

// Inisialisasi objek Bounce untuk tombol
Bounce debouncer = Bounce();

int LED_PIN = 17;
int BUTTON_PIN = 1;

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  pinMode(LED_PIN, OUTPUT);
  
  // Atur pin dan waktu debounce
  debouncer.attach(BUTTON_PIN);
  debouncer.interval(50); // Interval debounce dalam ms
}

void loop() {
  // Update status debouncer
  debouncer.update();
  
  // Periksa apakah tombol baru saja ditekan (transisi HIGH ke LOW)
  if (debouncer.fell()) {
    // Toggle status LED
    digitalWrite(LED_PIN, !digitalRead(LED_PIN));
  }
}
```

**Simulasi Online:**

📱 **Simulasi Online**: [Coba contoh debouncing di Wokwi](https://wokwi.com/projects/428250143524059137)

> 💡 **Tip**: Pilih metode debouncing sesuai dengan kebutuhan aplikasi Anda. Metode delay sederhana umumnya cukup untuk aplikasi sederhana, tetapi metode dengan timestamp atau library lebih baik untuk aplikasi yang memerlukan responsivitas tinggi.

### Praktikum 2: Lampu Lalu Lintas Sederhana

Mari kita buat sistem lampu lalu lintas sederhana dengan tiga LED (merah, kuning, hijau) dan tombol untuk mengganti siklus.

**Komponen yang Dibutuhkan:**
1. ESP32-S3 DevKit
2. 3 LED (merah, kuning, hijau)
3. 3 Resistor 220Ω
4. 1 Push button
5. Breadboard
6. Kabel jumper

**Rangkaian:**

<div align="center">
  <img src="https://github.com/user-attachments/assets/3711e8ed-77c3-43ec-bc9e-875b0f71db1e" width="500">
  <p><em>Rangkaian Lampu Lalu Lintas dengan ESP32-S3</em></p>
</div>

1. Hubungkan LED:
   - LED merah: kaki anoda → resistor → GPIO8, kaki katoda → GND
   - LED kuning: kaki anoda → resistor → GPIO18, kaki katoda → GND
   - LED hijau: kaki anoda → resistor → GPIO17, kaki katoda → GND

2. Hubungkan tombol:
   - Satu kaki tombol ke GPIO1
   - Kaki lainnya ke GND

**Simulasi Online:**

📱 **Simulasi Online**: [Coba rangkaian lampu lalu lintas di Wokwi](https://wokwi.com/projects/428338952566609921)

⚠️ **Catatan**: Simulasi online sangat membantu untuk memahami logika program, namun pengalaman dengan hardware asli memberikan pemahaman yang lebih baik tentang komponen elektronik dan koneksi fisik.

**Kode Program:**

```cpp
/*
 * Praktikum 2: Lampu Lalu Lintas Sederhana
 * Fungsi: Mensimulasikan siklus lampu lalu lintas dengan 3 LED dan 
 *         tombol untuk mengganti mode siklus
 * Platform: ESP32-S3
 */

// Definisi pin untuk LED
const int RED_PIN = 8;
const int YELLOW_PIN = 18;
const int GREEN_PIN = 17;
const int BUTTON_PIN = 1;

// Definisi mode lampu lalu lintas
const int MODE_AUTO = 0;  // Mode otomatis (siklus normal)
const int MODE_MANUAL = 1;  // Mode manual (ganti dengan tombol)
const int MODE_BLINK = 2;  // Mode berkedip (kuning berkedip)

// Variabel global
int currentMode = MODE_AUTO;  // Mode saat ini
int currentLight = RED_PIN;   // Lampu yang aktif saat ini
unsigned long lastChangeTime = 0;  // Waktu perubahan terakhir
unsigned long lastDebounceTime = 0;  // Waktu debounce terakhir
int lastButtonState = HIGH;  // Status tombol sebelumnya
int buttonState = HIGH;      // Status tombol saat ini

// Durasi setiap lampu (dalam milidetik)
const unsigned long RED_DURATION = 5000;     // Merah: 5 detik
const unsigned long YELLOW_DURATION = 2000;  // Kuning: 2 detik
const unsigned long GREEN_DURATION = 5000;   // Hijau: 5 detik
const unsigned long BLINK_DURATION = 500;    // Interval kedip: 0.5 detik

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  delay(1000);
  
  // Konfigurasi pin
  pinMode(RED_PIN, OUTPUT);
  pinMode(YELLOW_PIN, OUTPUT);
  pinMode(GREEN_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Matikan semua lampu pada awalnya
  digitalWrite(RED_PIN, LOW);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN, LOW);
  
  // Nyalakan lampu merah pertama
  digitalWrite(RED_PIN, HIGH);
  
  Serial.println("Sistem Lampu Lalu Lintas Dimulai");
  Serial.println("Mode saat ini: Otomatis");
  Serial.println("Tekan tombol untuk mengganti mode");
}

void loop() {
  // Baca status tombol dengan debouncing
  int reading = digitalRead(BUTTON_PIN);
  
  // Periksa apakah status tombol berubah
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }
  
  // Periksa apakah perubahan status stabil
  if ((millis() - lastDebounceTime) > 50) {  // 50ms debounce time
    // Jika status telah berubah
    if (reading != buttonState) {
      buttonState = reading;
      
      // Jika tombol ditekan (LOW karena INPUT_PULLUP)
      if (buttonState == LOW) {
        // Ubah mode
        currentMode = (currentMode + 1) % 3;  // Rotasi antara 3 mode
        
        Serial.print("Mode Diubah ke: ");
        switch (currentMode) {
          case MODE_AUTO:
            Serial.println("Otomatis");
            break;
          case MODE_MANUAL:
            Serial.println("Manual");
            break;
          case MODE_BLINK:
            Serial.println("Berkedip");
            break;
        }
        
        // Reset semua lampu
        digitalWrite(RED_PIN, LOW);
        digitalWrite(YELLOW_PIN, LOW);
        digitalWrite(GREEN_PIN, LOW);
        
        // Set lampu awal berdasarkan mode
        if (currentMode == MODE_AUTO || currentMode == MODE_MANUAL) {
          digitalWrite(RED_PIN, HIGH);
          currentLight = RED_PIN;
        }
        
        // Reset timer
        lastChangeTime = millis();
      }
    }
  }
  
  // Simpan status tombol untuk perbandingan berikutnya
  lastButtonState = reading;
  
  // Logika berdasarkan mode saat ini
  switch (currentMode) {
    case MODE_AUTO:
      handleAutoMode();
      break;
    case MODE_MANUAL:
      handleManualMode();
      break;
    case MODE_BLINK:
      handleBlinkMode();
      break;
  }
}

void handleAutoMode() {
  unsigned long currentTime = millis();
  unsigned long elapsedTime = currentTime - lastChangeTime;
  
  // Ubah lampu berdasarkan waktu
  if (currentLight == RED_PIN && elapsedTime >= RED_DURATION) {
    // Dari Merah ke Hijau
    digitalWrite(RED_PIN, LOW);
    digitalWrite(GREEN_PIN, HIGH);
    currentLight = GREEN_PIN;
    lastChangeTime = currentTime;
    Serial.println("Otomatis: Hijau");
  }
  else if (currentLight == GREEN_PIN && elapsedTime >= GREEN_DURATION) {
    // Dari Hijau ke Kuning
    digitalWrite(GREEN_PIN, LOW);
    digitalWrite(YELLOW_PIN, HIGH);
    currentLight = YELLOW_PIN;
    lastChangeTime = currentTime;
    Serial.println("Otomatis: Kuning");
  }
  else if (currentLight == YELLOW_PIN && elapsedTime >= YELLOW_DURATION) {
    // Dari Kuning ke Merah
    digitalWrite(YELLOW_PIN, LOW);
    digitalWrite(RED_PIN, HIGH);
    currentLight = RED_PIN;
    lastChangeTime = currentTime;
    Serial.println("Otomatis: Merah");
  }
}

void handleManualMode() {
  // Dalam mode manual, tombol telah mengubah lampu
  // Kita hanya menunggu penekanan tombol berikutnya
}

void handleBlinkMode() {
  unsigned long currentTime = millis();
  unsigned long elapsedTime = currentTime - lastChangeTime;
  
  // Berkedip kuning
  if (elapsedTime >= BLINK_DURATION) {
    // Toggle status lampu kuning
    if (digitalRead(YELLOW_PIN) == HIGH) {
      digitalWrite(YELLOW_PIN, LOW);
    } else {
      digitalWrite(YELLOW_PIN, HIGH);
    }
    lastChangeTime = currentTime;
  }
}
```

**Penjelasan Kode:**
1. Program mengimplementasikan tiga mode kerja:
   - **Mode Otomatis**: Siklus normal lampu lalu lintas (merah → hijau → kuning → merah)
   - **Mode Manual**: Pengguna mengontrol perubahan lampu dengan tombol
   - **Mode Berkedip**: Lampu kuning berkedip (biasanya untuk situasi khusus)

2. Kita menggunakan teknik debouncing untuk mendeteksi penekanan tombol dengan benar

3. Tombol digunakan untuk mengganti antara tiga mode operasi

4. Setiap mode memiliki fungsi penanganan sendiri untuk mengisolasi logikanya

5. Kita menggunakan `millis()` daripada `delay()` agar program tetap responsif

> 💡 **Insight**: Penggunaan `millis()` untuk penentuan waktu non-blocking adalah praktik umum dalam pemrograman microcontroller. Ini memungkinkan program untuk bereaksi terhadap input (seperti tombol) sambil tetap mempertahankan timing yang tepat untuk tugas lain.

---

## 📊 Pemrosesan Sinyal Analog

Selain sinyal digital (HIGH/LOW), ESP32-S3 juga dapat bekerja dengan sinyal analog yang memiliki berbagai level tegangan. Kemampuan ini memungkinkan interaksi dengan sensor dan aktuator analog.

<div align="center">
  <img src="https://github.com/user-attachments/assets/0e63c9a3-fb98-4f07-8759-973e6c86b1fa" width="400">
  <p><em>Perbandingan Sinyal Digital dan Analog</em></p>
</div>

### ADC (Analog-to-Digital Converter)

ADC mengubah tegangan analog (0V hingga 3.3V) menjadi nilai digital (0 hingga 4095 pada ESP32-S3) yang dapat diproses oleh microcontroller.

**Spesifikasi ADC ESP32-S3:**
- Dua modul ADC: ADC1 dan ADC2
- Resolusi 12-bit (0-4095)
- Range input default: 0V hingga 3.3V (dapat dikonfigurasi dengan atenuasi)
- ADC1 memiliki 10 channel, ADC2 memiliki 10 channel
- ADC2 tidak dapat digunakan ketika WiFi aktif

**Penggunaan ADC:**

```cpp
// Baca nilai analog dari pin 1 (GPIO1, ADC1_CH0)
int adcValue = analogRead(1);

// Konversi nilai ADC menjadi tegangan (volt)
float voltage = adcValue * (3.3 / 4095.0);

// Mengonfigurasi resolusi ADC (default 12-bit)
analogReadResolution(12);  // Nilai 0-4095
// atau
analogReadResolution(10);  // Nilai 0-1023
```

**Atenuasi ADC:**

ESP32-S3 memungkinkan konfigurasi atenuasi yang mengubah range input ADC.

```cpp
// Atur atenuasi ADC untuk pin tertentu
analogSetPinAttenuation(pin, attenuation);

// Atenuasi yang tersedia:
// ADC_0db: range 0-1V
// ADC_2_5db: range 0-1.5V 
// ADC_6db: range 0-2V
// ADC_11db: range 0-3.3V (default)
```

> ⚠️ **Penting**: ADC pada ESP32-S3 tidak sepenuhnya linear di seluruh range. Untuk pengukuran yang sangat akurat, pertimbangkan untuk menggunakan kalibrasi atau referensi eksternal.

### DAC (Digital-to-Analog Converter)

DAC melakukan konversi sebaliknya - mengubah nilai digital menjadi tegangan analog. ESP32-S3 memiliki dua channel DAC 8-bit.

**Spesifikasi DAC ESP32-S3:**
- Dua channel DAC (DAC1 dan DAC2)
- Resolusi 8-bit (0-255)
- Output: 0V hingga 3.3V
- DAC1 terhubung ke GPIO17
- DAC2 terhubung ke GPIO18

**Penggunaan DAC:**

```cpp
// Mengatur output DAC pada pin DAC1 (GPIO17)
dacWrite(17, 128);  // Menetapkan output ke ~1.65V (setengah dari 3.3V)

// Mengatur output DAC pada pin DAC2 (GPIO18)
dacWrite(18, 255);  // Menetapkan output ke ~3.3V (maksimum)
```

### PWM (Pulse Width Modulation)

PWM adalah teknik untuk membuat sinyal "pseudo-analog" dengan mengubah duty cycle dari sinyal digital. ESP32-S3 memiliki dukungan PWM yang sangat fleksibel.

<div align="center">
  <img src="https://i.imgur.com/2xm4D7w.png" width="600">
  <p><em>Variasi Duty Cycle PWM dan Efeknya pada Nilai Rata-rata</em></p>
</div>

**Karakteristik PWM ESP32-S3:**
- MCPWM (Motor Control PWM) dan LEDC (LED Control) controllers
- Resolusi hingga 16-bit
- Frekuensi hingga beberapa MHz
- Hampir semua pin GPIO dapat digunakan untuk PWM

**Penggunaan LEDC PWM:**

```cpp
// Parameter LEDC
const int freq = 5000;        // Frekuensi PWM (Hz)
const int ledChannel = 0;     // Channel LEDC (0-15)
const int resolution = 8;     // Resolusi (bit), 8-bit memberikan nilai 0-255
const int ledPin = 13;        // Pin untuk output PWM

void setup() {
  // Konfigurasi LEDC
  ledcSetup(ledChannel, freq, resolution);
  
  // Hubungkan channel ke pin GPIO
  ledcAttachPin(ledPin, ledChannel);
}

void loop() {
  // Atur duty cycle ke 50% (127 dari 255 untuk resolusi 8-bit)
  ledcWrite(ledChannel, 127);
  delay(1000);
  
  // Atur duty cycle ke 25% (64 dari 255)
  ledcWrite(ledChannel, 64);
  delay(1000);
  
  // Atur duty cycle ke 75% (191 dari 255)
  ledcWrite(ledChannel, 191);
  delay(1000);
}
```

**Simulasi Online PWM:**

📱 **Simulasi Online**: [Coba PWM dasar di Wokwi](https://wokwi.com/projects/371564018721381377)

**Kasus Penggunaan Umum PWM:**
1. Mengontrol kecerahan LED
2. Menghasilkan sinyal audio
3. Mengontrol kecepatan motor
4. Servo kontrol
5. Dimming lampu

### Praktikum 3: Pengukur Cahaya dengan LDR

Sekarang mari kita buat rangkaian untuk mengukur tingkat cahaya menggunakan LDR (Light Dependent Resistor) dan ESP32-S3.

**Komponen yang Dibutuhkan:**
1. ESP32-S3 DevKit
2. LDR (Light Dependent Resistor)
3. Resistor 10kΩ (untuk pembagi tegangan)
4. LED
5. Resistor 220Ω (untuk LED)
6. Breadboard
7. Kabel jumper

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/Jw9rkBv.png" width="600">
  <p><em>Rangkaian Pengukur Cahaya dengan LDR dan ESP32-S3</em></p>
</div>

1. Hubungkan LDR dalam konfigurasi pembagi tegangan:
   - Satu ujung LDR ke 3.3V
   - Ujung lain LDR ke pin GPIO1 (ADC1_CH0) dan juga ke resistor 10kΩ
   - Ujung lain resistor 10kΩ ke GND

2. Hubungkan LED:
   - Kaki anoda (panjang) ke resistor 220Ω
   - Resistor ke pin GPIO13
   - Kaki katoda (pendek) ke GND

**Simulasi Online:**

📱 **Simulasi Online**: [Coba rangkaian pengukur cahaya di Wokwi](https://wokwi.com/projects/371565220184115201)

⚠️ **Catatan**: Dalam simulasi online, LDR bisa dikendalikan dengan mengklik pada komponennya untuk mensimulasikan perubahan cahaya. Pada hardware asli, Anda akan melihat respons nyata terhadap cahaya di lingkungan.

**Kode Program:**

```cpp
/*
 * Praktikum 3: Pengukur Cahaya dengan LDR
 * Fungsi: Membaca nilai LDR dan menampilkan level cahaya di Serial Monitor
 *         serta mengontrol LED berdasarkan level cahaya
 * Platform: ESP32-S3
 */

// Definisi pin
const int LDR_PIN = 1;    // GPIO1, ADC1_CH0 untuk LDR
const int LED_PIN = 13;   // GPIO13 untuk LED

// Parameter untuk pemetaan nilai
const int ANALOG_RESOLUTION = 4095;  // 12-bit ADC
const int SAMPLES = 10;             // Jumlah sampel untuk rata-rata
const int THRESHOLD = 2000;         // Ambang batas gelap/terang (sesuaikan sesuai kondisi)

// Variabel global
int lightLevel = 0;          // Nilai level cahaya
bool isLightOn = false;      // Status LED
unsigned long lastPrintTime = 0;  // Waktu terakhir cetak ke Serial

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  delay(1000);
  
  // Konfigurasi pin
  pinMode(LED_PIN, OUTPUT);
  
  // Konfigurasi ADC
  analogReadResolution(12);  // Set resolusi ADC ke 12-bit (0-4095)
  analogSetAttenuation(ADC_11db);  // Set atenuasi untuk range penuh 0-3.3V
  
  Serial.println("Pengukur Cahaya dengan LDR Dimulai");
  Serial.println("=====================================");
  Serial.println("Nilai LDR akan terbaca pada skala 0-4095");
  Serial.println("Nilai rendah = cahaya terang");
  Serial.println("Nilai tinggi = cahaya redup/gelap");
  Serial.println("LED akan menyala otomatis saat gelap");
}

void loop() {
  // Baca nilai LDR (rata-rata dari beberapa sampel)
  lightLevel = readLightLevel();
  
  // Kelola LED berdasarkan level cahaya
  if (lightLevel > THRESHOLD && !isLightOn) {
    // Gelap, nyalakan LED
    digitalWrite(LED_PIN, HIGH);
    isLightOn = true;
    Serial.println("Cahaya redup terdeteksi - LED dinyalakan");
  } 
  else if (lightLevel <= THRESHOLD && isLightOn) {
    // Terang, matikan LED
    digitalWrite(LED_PIN, LOW);
    isLightOn = false;
    Serial.println("Cahaya terang terdeteksi - LED dimatikan");
  }
  
  // Cetak nilai ke Serial Monitor setiap 2 detik
  if (millis() - lastPrintTime > 2000) {
    Serial.print("Level Cahaya: ");
    Serial.print(lightLevel);
    
    // Konversi ke persentase (terbalik, karena nilai LDR tinggi = rendah cahaya)
    int lightPercent = map(lightLevel, 0, ANALOG_RESOLUTION, 100, 0);
    Serial.print(" (");
    Serial.print(lightPercent);
    Serial.print("%)");
    
    // Tampilkan status LED
    Serial.print(" - LED: ");
    Serial.println(isLightOn ? "ON" : "OFF");
    
    // Visualisasi berbasis teks
    Serial.print("[");
    for (int i = 0; i < 20; i++) {
      if (i < lightPercent / 5) {
        Serial.print("*");
      } else {
        Serial.print(" ");
      }
    }
    Serial.println("]");
    
    lastPrintTime = millis();
  }
  
  delay(100);  // Jeda untuk stabilitas pembacaan
}

// Fungsi untuk membaca level cahaya dengan rata-rata beberapa sampel
int readLightLevel() {
  long sum = 0;
  
  // Ambil beberapa sampel dan hitung rata-rata
  for (int i = 0; i < SAMPLES; i++) {
    sum += analogRead(LDR_PIN);
    delay(2);  // Jeda kecil antara pembacaan
  }
  
  return sum / SAMPLES;
}
```

**Penjelasan Kode:**
1. Kita menggunakan ADC untuk membaca nilai dari LDR yang dirangkai dalam konfigurasi pembagi tegangan
2. Ketika cahaya terang, resistansi LDR rendah, sehingga nilai ADC juga rendah
3. Ketika cahaya redup/gelap, resistansi LDR tinggi, sehingga nilai ADC juga tinggi
4. Kita mengambil rata-rata dari beberapa sampel untuk mengurangi noise
5. LED dikendalikan berdasarkan ambang batas (threshold) nilai cahaya
6. Kita juga membuat visualisasi sederhana berbasis teks di Serial Monitor

> 💡 **Catatan**: Nilai ambang batas (THRESHOLD) mungkin perlu disesuaikan berdasarkan kondisi pencahayaan lingkungan dan karakteristik LDR yang digunakan.

### Praktikum 4: Kontrol Kecerahan LED

Mari kita buat rangkaian untuk mengontrol kecerahan LED menggunakan potentiometer dan PWM.

**Komponen yang Dibutuhkan:**
1. ESP32-S3 DevKit
2. Potentiometer (10kΩ atau 50kΩ)
3. LED
4. Resistor 220Ω (untuk LED)
5. Breadboard
6. Kabel jumper

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/3Ai68yp.png" width="600">
  <p><em>Rangkaian Kontrol Kecerahan LED dengan Potentiometer dan ESP32-S3</em></p>
</div>

1. Hubungkan potentiometer:
   - Terminal luar pertama ke 3.3V
   - Terminal luar kedua ke GND
   - Terminal tengah (wiper) ke pin GPIO1 (ADC1_CH0)

2. Hubungkan LED:
   - Kaki anoda (panjang) ke resistor 220Ω
   - Resistor ke pin GPIO13
   - Kaki katoda (pendek) ke GND

**Simulasi Online:**

📱 **Simulasi Online**: [Coba kontrol kecerahan LED di Wokwi](https://wokwi.com/projects/371566427258081281)

⚠️ **Catatan**: Dalam simulasi, putar potentiometer dengan mengklik dan menariknya untuk mengubah kecerahan LED.

**Kode Program:**

```cpp
/*
 * Praktikum 4: Kontrol Kecerahan LED
 * Fungsi: Mengontrol kecerahan LED menggunakan potentiometer dengan PWM
 * Platform: ESP32-S3
 */

// Definisi pin
const int POT_PIN = 1;    // GPIO1, ADC1_CH0 untuk potentiometer
const int LED_PIN = 13;   // GPIO13 untuk LED

// Parameter PWM
const int PWM_FREQ = 5000;     // Frekuensi PWM (Hz)
const int PWM_CHANNEL = 0;     // Channel PWM (0-15)
const int PWM_RESOLUTION = 10; // Resolusi PWM (bit)
const int PWM_MAX = (1 << PWM_RESOLUTION) - 1; // Nilai maksimum berdasarkan resolusi (1023 untuk 10-bit)

// Parameter lainnya
const int SAMPLES = 10;          // Jumlah sampel untuk rata-rata
const int ANALOG_RESOLUTION = 4095;  // Resolusi ADC (12-bit)

// Variabel global
int brightness = 0;             // Nilai kecerahan saat ini
int lastBrightness = -1;        // Kecerahan terakhir yang dicetak
unsigned long lastPrintTime = 0;  // Waktu terakhir cetak ke Serial

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  delay(1000);
  
  // Konfigurasi ADC
  analogReadResolution(12);  // Set resolusi ADC ke 12-bit (0-4095)
  analogSetAttenuation(ADC_11db);  // Set atenuasi untuk range penuh 0-3.3V
  
  // Konfigurasi PWM
  ledcSetup(PWM_CHANNEL, PWM_FREQ, PWM_RESOLUTION);
  ledcAttachPin(LED_PIN, PWM_CHANNEL);
  
  Serial.println("Kontrol Kecerahan LED Dimulai");
  Serial.println("====================================");
  Serial.println("Putar potentiometer untuk mengubah kecerahan LED");
  Serial.println("Nilai potentiometer: 0-4095");
  Serial.println("Nilai PWM: 0-1023 (10-bit)");
}

void loop() {
  // Baca nilai potentiometer (rata-rata dari beberapa sampel)
  int potValue = readPotValue();
  
  // Petakan nilai potentiometer ke range PWM
  brightness = map(potValue, 0, ANALOG_RESOLUTION, 0, PWM_MAX);
  
  // Atur kecerahan LED via PWM
  ledcWrite(PWM_CHANNEL, brightness);
  
  // Cetak nilai ke Serial Monitor setiap 0.5 detik jika berubah signifikan
  if ((millis() - lastPrintTime > 500) && (abs(brightness - lastBrightness) > PWM_MAX/100)) {
    Serial.print("Nilai Potentiometer: ");
    Serial.print(potValue);
    Serial.print(" | Kecerahan LED: ");
    Serial.print(brightness);
    Serial.print(" (");
    Serial.print(map(brightness, 0, PWM_MAX, 0, 100));
    Serial.println("%)");
    
    // Visualisasi berbasis teks
    Serial.print("[");
    for (int i = 0; i < 20; i++) {
      if (i < map(brightness, 0, PWM_MAX, 0, 20)) {
        Serial.print("*");
      } else {
        Serial.print(" ");
      }
    }
    Serial.println("]");
    
    lastBrightness = brightness;
    lastPrintTime = millis();
  }
  
  delay(20);  // Jeda kecil untuk stabilitas
}

// Fungsi untuk membaca nilai potentiometer dengan rata-rata beberapa sampel
int readPotValue() {
  long sum = 0;
  
  // Ambil beberapa sampel dan hitung rata-rata
  for (int i = 0; i < SAMPLES; i++) {
    sum += analogRead(POT_PIN);
    delay(2);  // Jeda kecil antara pembacaan
  }
  
  return sum / SAMPLES;
}
```

**Penjelasan Kode:**
1. Kita menggunakan ADC untuk membaca nilai dari potentiometer
2. Nilai potentiometer (0-4095) dipetakan ke range PWM (0-1023 untuk resolusi 10-bit)
3. PWM digunakan untuk mengontrol kecerahan LED melalui fungsi `ledcWrite()`
4. Kita mengimplementasikan visualisasi berbasis teks di Serial Monitor
5. Kode mengurangi output Serial dengan hanya menampilkan ketika nilai berubah signifikan

> 💡 **Tip**: Resolusi PWM 10-bit memberikan 1024 tingkat kecerahan yang berbeda, yang lebih dari cukup untuk transisi halus yang dapat dirasakan oleh mata manusia. Bahkan resolusi 8-bit (256 tingkat) sudah cukup untuk sebagian besar aplikasi visual.

---

## 🔄 Proyek Mini: Lampu Otomatis

Mari kita gabungkan pengetahuan yang telah dipelajari dengan membuat sebuah proyek mini - sistem lampu otomatis yang merespons tingkat cahaya dengan fungsionalitas tambahan.

**Komponen yang Dibutuhkan:**
1. ESP32-S3 DevKit
2. LDR (Light Dependent Resistor)
3. Resistor 10kΩ (untuk pembagi tegangan LDR)
4. LED (sebaiknya warna putih terang)
5. Resistor 220Ω (untuk LED)
6. Potentiometer 10kΩ
7. Push button
8. Breadboard
9. Kabel jumper

**Fungsionalitas:**
- Lampu menyala otomatis ketika gelap (berdasarkan LDR)
- Kecerahan lampu dapat diatur dengan potentiometer
- Tombol untuk mengganti mode (otomatis/manual/off)
- Indikator visual mode operasi di Serial Monitor

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/V4jD9o9.png" width="650">
  <p><em>Rangkaian Lampu Otomatis dengan ESP32-S3</em></p>
</div>

**Simulasi Online:**

📱 **Simulasi Online**: [Coba proyek lampu otomatis di Wokwi](https://wokwi.com/projects/371567768009932801)

⚠️ **Catatan**: Proyek ini menggabungkan semua konsep yang telah dipelajari. Pengujian dengan hardware asli memberikan pengalaman yang lebih komprehensif dalam membangun sistem otomatis.

**Kode Program:**

```cpp
/*
 * Proyek Mini: Lampu Otomatis 
 * Fungsi: Sistem lampu otomatis yang merespons tingkat cahaya
 *         dengan kontrol kecerahan manual dan pemilihan mode
 * Platform: ESP32-S3
 */

// Definisi pin
const int LDR_PIN = 1;       // GPIO1, ADC1_CH0 untuk LDR
const int POT_PIN = 2;       // GPIO2, ADC1_CH1 untuk potentiometer
const int LED_PIN = 13;      // GPIO13 untuk LED utama
const int BUTTON_PIN = 4;    // GPIO4 untuk tombol mode

// Parameter PWM
const int PWM_FREQ = 5000;     // Frekuensi PWM (Hz)
const int PWM_CHANNEL = 0;     // Channel PWM (0-15)
const int PWM_RESOLUTION = 10; // Resolusi PWM (bit)
const int PWM_MAX = 1023;      // Nilai maksimum berdasarkan resolusi (1023 untuk 10-bit)

// Parameter lainnya
const int SAMPLES = 10;          // Jumlah sampel untuk rata-rata
const int LIGHT_THRESHOLD = 2000;  // Ambang batas gelap/terang (sesuaikan)
const int ANALOG_RESOLUTION = 4095; // Resolusi ADC (12-bit)

// Definisi mode
const int MODE_AUTO = 0;    // Mode otomatis (nyala berdasarkan LDR)
const int MODE_MANUAL = 1;  // Mode manual (nyala berdasarkan potentiometer)
const int MODE_OFF = 2;     // Mode mati (LED selalu mati)

// Variabel global
int currentMode = MODE_AUTO;     // Mode saat ini
int lightLevel = 0;              // Nilai level cahaya
int brightness = 0;              // Nilai kecerahan saat ini
unsigned long lastPrintTime = 0; // Waktu terakhir cetak ke Serial

// Variabel untuk debouncing tombol
int lastButtonState = HIGH;
unsigned long lastDebounceTime = 0;
const unsigned long debounceDelay = 50;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  delay(1000);
  
  // Konfigurasi pin
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Konfigurasi ADC
  analogReadResolution(12);  // Set resolusi ADC ke 12-bit (0-4095)
  analogSetAttenuation(ADC_11db);  // Set atenuasi untuk range penuh 0-3.3V
  
  // Konfigurasi PWM
  ledcSetup(PWM_CHANNEL, PWM_FREQ, PWM_RESOLUTION);
  ledcAttachPin(LED_PIN, PWM_CHANNEL);
  
  Serial.println("\n===== Sistem Lampu Otomatis =====");
  Serial.println("Mode awal: Otomatis");
  Serial.println("Tekan tombol untuk mengubah mode");
}

void loop() {
  // Baca nilai LDR dan potentiometer
  lightLevel = readAverageAnalog(LDR_PIN);
  int potValue = readAverageAnalog(POT_PIN);
  
  // Petakan nilai potentiometer ke range PWM
  int manualBrightness = map(potValue, 0, ANALOG_RESOLUTION, 0, PWM_MAX);
  
  // Check tombol mode dengan debouncing
  checkModeButton();
  
  // Kontrol kecerahan LED berdasarkan mode
  if (currentMode == MODE_AUTO) {
    // Mode otomatis: nyala berdasarkan LDR
    if (lightLevel > LIGHT_THRESHOLD) {
      // Gelap terdeteksi, nyalakan LED
      // Gunakan potentiometer untuk membatasi kecerahan maksimum
      brightness = map(lightLevel, LIGHT_THRESHOLD, ANALOG_RESOLUTION, 
                      0, manualBrightness);
      brightness = constrain(brightness, 0, PWM_MAX);
    } else {
      // Terang terdeteksi, matikan LED
      brightness = 0;
    }
  } 
  else if (currentMode == MODE_MANUAL) {
    // Mode manual: kecerahan dikontrol langsung oleh potentiometer
    brightness = manualBrightness;
  } 
  else {
    // Mode OFF: LED selalu mati
    brightness = 0;
  }
  
  // Atur kecerahan LED via PWM
  ledcWrite(PWM_CHANNEL, brightness);
  
  // Cetak status ke Serial Monitor setiap 1 detik
  if (millis() - lastPrintTime > 1000) {
    printStatus(potValue);
    lastPrintTime = millis();
  }
  
  delay(50);  // Jeda kecil untuk stabilitas
}

// Fungsi untuk membaca nilai analog dengan rata-rata beberapa sampel
int readAverageAnalog(int pin) {
  long sum = 0;
  
  for (int i = 0; i < SAMPLES; i++) {
    sum += analogRead(pin);
    delay(2);  // Jeda kecil antara pembacaan
  }
  
  return sum / SAMPLES;
}

// Fungsi untuk memeriksa tombol mode dengan debouncing
void checkModeButton() {
  // Baca status tombol saat ini
  int reading = digitalRead(BUTTON_PIN);

  // Periksa apakah status tombol berubah
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }

  // Periksa apakah perubahan status stabil
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika status telah berubah dari HIGH ke LOW (tombol ditekan)
    if (reading == LOW && lastButtonState == HIGH) {
      // Ubah mode
      currentMode = (currentMode + 1) % 3;  // Rotasi antara 3 mode
      
      Serial.print("\n> Mode diubah ke: ");
      switch (currentMode) {
        case MODE_AUTO:
          Serial.println("OTOMATIS");
          break;
        case MODE_MANUAL:
          Serial.println("MANUAL");
          break;
        case MODE_OFF:
          Serial.println("OFF");
          break;
      }
    }
  }

  // Simpan status tombol untuk perbandingan berikutnya
  lastButtonState = reading;
}

// Fungsi untuk mencetak status sistem ke Serial Monitor
void printStatus(int potValue) {
  // Header status
  Serial.println("\n----- STATUS SISTEM -----");
  
  // Mode operasi
  Serial.print("Mode: ");
  switch (currentMode) {
    case MODE_AUTO:
      Serial.println("OTOMATIS");
      break;
    case MODE_MANUAL:
      Serial.println("MANUAL");
      break;
    case MODE_OFF:
      Serial.println("OFF");
      break;
  }
  
  // Level cahaya
  int lightPercent = map(lightLevel, 0, ANALOG_RESOLUTION, 100, 0);
  Serial.print("Level Cahaya: ");
  Serial.print(lightLevel);
  Serial.print(" (");
  Serial.print(lightPercent);
  Serial.println("%)");
  
  // Visualisasi cahaya
  Serial.print("Cahaya: [");
  for (int i = 0; i < 20; i++) {
    if (i < lightPercent / 5) {
      Serial.print("*");
    } else {
      Serial.print(" ");
    }
  }
  Serial.println("]");
  
  // Kecerahan LED
  int brightPercent = map(brightness, 0, PWM_MAX, 0, 100);
  Serial.print("Kecerahan LED: ");
  Serial.print(brightness);
  Serial.print(" (");
  Serial.print(brightPercent);
  Serial.println("%)");
  
  // Visualisasi kecerahan
  Serial.print("Output: [");
  for (int i = 0; i < 20; i++) {
    if (i < brightPercent / 5) {
      Serial.print("*");
    } else {
      Serial.print(" ");
    }
  }
  Serial.println("]");
  
  // Nilai potentiometer
  int potPercent = map(potValue, 0, ANALOG_RESOLUTION, 0, 100);
  Serial.print("Potentiometer: ");
  Serial.print(potValue);
  Serial.print(" (");
  Serial.print(potPercent);
  Serial.println("%)");
}
```

**Penjelasan Proyek:**
1. **Komponen Hardware:**
   - LDR untuk mendeteksi tingkat cahaya
   - Potentiometer untuk menyesuaikan kecerahan manual
   - LED sebagai lampu utama
   - Tombol untuk mengganti mode operasi

2. **Mode Operasi:**
   - **Mode Otomatis**: LED menyala berdasarkan tingkat cahaya (LDR), kecerahan maksimum dibatasi oleh potentiometer
   - **Mode Manual**: Kecerahan LED dikendalikan langsung oleh potentiometer
   - **Mode Off**: LED selalu mati, terlepas dari kondisi LDR

3. **Fitur Software:**
   - Rata-rata pembacaan untuk stabilitas
   - Debouncing tombol untuk pengalihan mode yang handal
   - Visualisasi berbasis teks di Serial Monitor
   - Pemetaan nilai (mapping) untuk konversi antar range nilai
   - Fungsi-fungsi modular untuk kode yang lebih terorganisir

> 💡 **Aplikasi Praktis**: Sistem lampu otomatis ini dapat digunakan sebagai lampu malam di rumah, lampu jalan skala kecil, atau sebagai demonstrasi konsep automasi sederhana. Ia menunjukkan bagaimana sensor, aktuator, dan logika kontrol bekerja bersama.

---

## 🏋️ Latihan Mandiri

Uji pemahaman Anda dengan mencoba latihan-latihan berikut:

### Latihan 1: Simulasi Lampu Darurat

Buat sistem yang mendeteksi ketika listrik padam (simulasikan dengan tombol) dan mengendalikan LED yang berkedip dengan pola SOS (••• ▬▬▬ •••).

📱 **Simulasi Online**: [Coba latihan SOS di Wokwi](https://wokwi.com/projects/371568846765051905)

<details>
<summary>Petunjuk</summary>

```cpp
void blinkSOS() {
  // Pola SOS: 3 kedipan pendek, 3 kedipan panjang, 3 kedipan pendek
  
  // 3 kedipan pendek (S)
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(200);
    digitalWrite(LED_PIN, LOW);
    delay(200);
  }
  
  delay(400);  // Jeda antar karakter
  
  // 3 kedipan panjang (O)
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(600);
    digitalWrite(LED_PIN, LOW);
    delay(200);
  }
  
  delay(400);  // Jeda antar karakter
  
  // 3 kedipan pendek (S)
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(200);
    digitalWrite(LED_PIN, LOW);
    delay(200);
  }
  
  delay(1000);  // Jeda panjang sebelum pengulangan
}
```
</details>

### Latihan 2: Pengukur Level Cairan

Buat simulasi sistem yang mendeteksi level cairan dalam tangki menggunakan potentiometer sebagai sensor level. Tampilkan status tangki (kosong, rendah, sedang, tinggi, penuh) di Serial Monitor dan dengan LED berbeda.

⚠️ **Catatan**: Praktikum ini memerlukan pengujian dengan hardware fisik di laboratorium.

<details>
<summary>Petunjuk</summary>

```cpp
// Membaca nilai potentiometer dan menerjemahkannya ke level cairan
int tankLevel = map(analogRead(POT_PIN), 0, 4095, 0, 100);

// Menentukan status berdasarkan level
String status;
if (tankLevel < 10) {
  status = "KOSONG";
  // Nyalakan LED merah
} else if (tankLevel < 30) {
  status = "RENDAH";
  // Nyalakan LED kuning
} else if (tankLevel < 70) {
  status = "SEDANG";
  // Nyalakan LED hijau
} else if (tankLevel < 90) {
  status = "TINGGI";
  // Nyalakan LED biru
} else {
  status = "PENUH";
  // Nyalakan LED putih
}
```
</details>

### Latihan 3: Kontrol Motor DC dengan PWM

Simulasikan kontrol motor DC dengan LED. Gunakan dua tombol untuk meningkatkan dan menurunkan kecepatan, dan potentiometer untuk membatasi kecepatan maksimum. Tampilkan "RPM" simulasi di Serial Monitor.

📱 **Simulasi Online**: [Coba simulator motor DC di Wokwi](https://wokwi.com/projects/371569777483578369)

<details>
<summary>Petunjuk</summary>

```cpp
// Deklarasi pin dan variabel
const int BUTTON_UP_PIN = 4;    // Tombol untuk menambah kecepatan
const int BUTTON_DOWN_PIN = 5;  // Tombol untuk mengurangi kecepatan
const int POT_PIN = 1;          // Potentiometer untuk batas kecepatan
const int MOTOR_PIN = 13;       // Pin PWM untuk simulasi motor (LED)

// Variabel kecepatan
int currentSpeed = 0;           // Kecepatan saat ini (0-255)
int speedLimit = 255;           // Batas kecepatan maksimum

// Dalam loop
// Baca batas kecepatan dari potentiometer
speedLimit = map(analogRead(POT_PIN), 0, 4095, 0, 255);

// Periksa tombol-tombol (dengan debouncing)
if (/* tombol UP ditekan */) {
  currentSpeed += 5;
  if (currentSpeed > speedLimit) currentSpeed = speedLimit;
}

if (/* tombol DOWN ditekan */) {
  currentSpeed -= 5;
  if (currentSpeed < 0) currentSpeed = 0;
}

// Atur PWM untuk simulasi motor
ledcWrite(PWM_CHANNEL, currentSpeed);

// Hitung "RPM" simulasi (hanya untuk visualisasi)
int simulatedRPM = map(currentSpeed, 0, 255, 0, 1000);
```
</details>

### Latihan 4: LED RGB dengan Perubahan Gradual

Kendalikan LED RGB untuk berubah warna secara gradual melalui spektrum warna. Gunakan ADC untuk menyesuaikan kecepatan transisi.

📱 **Simulasi Online**: [Coba pengendali LED RGB di Wokwi](https://wokwi.com/projects/371570661908520961)

<details>
<summary>Petunjuk</summary>

```cpp
// Definisi pin untuk LED RGB
const int RED_PIN = 13;    // PWM untuk merah
const int GREEN_PIN = 12;  // PWM untuk hijau
const int BLUE_PIN = 11;   // PWM untuk biru
const int SPEED_PIN = 1;   // Potentiometer untuk kecepatan

// Konfigurasi PWM untuk setiap channel
ledcSetup(0, 5000, 8);  // Merah
ledcSetup(1, 5000, 8);  // Hijau
ledcSetup(2, 5000, 8);  // Biru

ledcAttachPin(RED_PIN, 0);
ledcAttachPin(GREEN_PIN, 1);
ledcAttachPin(BLUE_PIN, 2);

// Dalam loop, buat algoritma perubahan warna
// Misalnya, gunakan rumus matematika berbasis sinus untuk transisi halus:
float phase = 0.0;
// ...

void loop() {
  // Baca kecepatan perubahan dari potentiometer
  int changeSpeed = map(analogRead(SPEED_PIN), 0, 4095, 1, 20);
  float phaseIncrement = 0.01 * changeSpeed;
  
  // Hitung nilai RGB berdasarkan fase
  int red = 128 + 127 * sin(phase);
  int green = 128 + 127 * sin(phase + 2.0943);  // 2π/3 phase shift
  int blue = 128 + 127 * sin(phase + 4.1887);   // 4π/3 phase shift
  
  // Atur warna LED RGB
  ledcWrite(0, red);
  ledcWrite(1, green);
  ledcWrite(2, blue);
  
  // Tingkatkan fase untuk pergerakan
  phase += phaseIncrement;
  if (phase >= 6.28318) {  // 2π
    phase = 0;
  }
  
  delay(20);
}
```
</details>

---

## 🔧 Troubleshooting Umum

Berikut beberapa masalah umum yang mungkin Anda temui saat bekerja dengan GPIO ESP32-S3 dan solusinya:

### 1. LED Tidak Menyala

- **Periksa Orientasi LED**: Pastikan LED terpasang dengan polaritas yang benar (anoda/kaki panjang ke resistor, katoda/kaki pendek ke GND)
- **Periksa Resistor**: Pastikan resistor yang digunakan memiliki nilai sesuai (sekitar 220Ω hingga 330Ω)
- **Periksa Pin**: Pastikan pin yang dikonfigurasi di kode sesuai dengan yang digunakan di rangkaian
- **Coba Pin Berbeda**: Beberapa pin mungkin memiliki fungsi khusus yang membatasi penggunaannya, coba pin lain
- **Ukur Tegangan**: Ukur tegangan di pin GPIO dan di LED untuk memastikan ada aliran listrik

### 2. Tombol Tidak Bekerja

- **Periksa Konfigurasi Pin**: Pastikan pin dikonfigurasi dengan benar (`INPUT`, `INPUT_PULLUP`, atau `INPUT_PULLDOWN`)
- **Periksa Koneksi**: Pastikan tombol terhubung dengan benar ke pin GPIO dan GND
- **Tambahkan Debouncing**: Implementasikan teknik debouncing untuk mengatasi bouncing kontak
- **Coba Pin Berbeda**: Beberapa pin mungkin memiliki perilaku khusus saat boot atau reset
- **Cek Nilai INPUT_PULLUP**: Ingat bahwa dengan `INPUT_PULLUP`, pin akan bernilai HIGH saat tombol tidak ditekan dan LOW saat ditekan

### 3. Pembacaan ADC Tidak Stabil

- **Implementasikan Rata-rata**: Baca beberapa sampel dan hitung rata-ratanya
- **Tambahkan Kapasitor**: Pasang kapasitor 0.1μF ke 1μF antara pin ADC dan GND untuk memfilter noise
- **Periksa Sumber Tegangan**: Pastikan tegangan referensi stabil
- **Periksa Konfigurasi ADC**: Pastikan resolusi dan atenuasi dikonfigurasi dengan benar
- **Gunakan ADC1**: ADC2 tidak dapat digunakan saat WiFi aktif

### 4. PWM Tidak Bekerja

- **Periksa Konfigurasi PWM**: Pastikan `ledcSetup()` dan `ledcAttachPin()` digunakan dengan parameter yang benar
- **Periksa Channel**: Pastikan channel yang sama digunakan di `ledcSetup()` dan `ledcWrite()`
- **Pastikan Pin Mendukung PWM**: Sebagian besar pin mendukung PWM, tetapi beberapa mungkin memiliki batasan
- **Periksa Resolusi dan Frekuensi**: Beberapa kombinasi frekuensi dan resolusi tidak valid, coba nilai yang berbeda

### 5. Program Upload Gagal

- **Periksa Mode Boot**: Beberapa board memerlukan tombol BOOT ditekan saat memulai upload
- **Periksa Port Serial**: Pastikan port yang benar dipilih di Arduino IDE
- **Periksa Driver USB**: Pastikan driver USB CP210x atau CH340 terinstal dengan benar
- **Reset Board**: Coba tekan tombol EN (reset) pada board sebelum upload
- **Coba Kabel USB Berbeda**: Beberapa kabel USB hanya untuk daya dan tidak mendukung data

---

## 📚 Sumber Referensi

1. Espressif Systems. (2023). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

2. Espressif Systems. (2023). *ESP-IDF Programming Guide - GPIO & RTC GPIO*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/gpio.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/gpio.html)

3. Espressif Systems. (2023). *ESP-IDF Programming Guide - Analog to Digital Converter*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/adc.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/adc.html)

4. Espressif Systems. (2023). *ESP-IDF Programming Guide - LED Control (LEDC)*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/ledc.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/ledc.html)

5. Arduino. (2023). *Arduino Reference - DigitalRead*. [https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/)

6. Arduino. (2023). *Arduino Reference - AnalogRead*. [https://www.arduino.cc/reference/en/language/functions/analog-io/analogread/](https://www.arduino.cc/reference/en/language/functions/analog-io/analogread/)

7. Kolban, N. (2022). *Kolban's Book on ESP32*. Leanpub. [https://leanpub.com/kolban-ESP32](https://leanpub.com/kolban-ESP32)

8. RandomNerdTutorials. (2023). *ESP32 PWM with Arduino IDE*. [https://randomnerdtutorials.com/esp32-pwm-arduino-ide/](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/)

9. RandomNerdTutorials. (2023). *ESP32 ADC*. [https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/)

10. Grokhotkov, I. (2022). *Debouncing Buttons with ESP32*. Github ESP32 Arduino Core. [https://github.com/espressif/arduino-esp32/blob/master/libraries/ESP32/examples/Button/Button.ino](https://github.com/espressif/arduino-esp32/blob/master/libraries/ESP32/examples/Button/Button.ino)

---

**Modul berikutnya**: Komunikasi dan Tampilan dengan ESP32-S3
