# 📚 Modul 1: Pengenalan Embedded Systems dan ESP32 S3

<div align="center">
  
  ![ESP32 S3 Header](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/_images/esp32-s3-devkitc-1-v1.1-isometric.png)

  *ESP32-S3 Development Board*
</div>

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan modul ini, Anda diharapkan dapat:
- Menjelaskan konsep dasar sistem tertanam (embedded systems)
- Memahami arsitektur dan fitur ESP32 S3
- Menyiapkan lingkungan pengembangan yang diperlukan
- Menulis dan menjalankan program pertama pada ESP32 S3

---

## 📖 Minggu 1: Dasar-Dasar Microcontroller

### 🔍 Pengenalan Sistem Tertanam (Embedded Systems)

#### Apa itu Sistem Tertanam?

Sistem tertanam (embedded system) adalah sistem komputer khusus yang dirancang untuk melakukan fungsi tertentu dalam sistem yang lebih besar. Berbeda dengan komputer umum yang dirancang untuk fleksibilitas, sistem tertanam biasanya dioptimalkan untuk:
- Fungsi spesifik
- Ukuran kecil
- Efisiensi daya
- Keandalan tinggi
- Operasi real-time

> 💡 **Tahukah Anda?** Sistem tertanam ada di sekitar kita setiap hari. Dari peralatan rumah tangga seperti microwave dan mesin cuci, hingga perangkat canggih seperti kamera digital, mobil modern, dan perangkat medis.

#### Evolusi Microcontroller

<div align="center">
  
| Era | Contoh | Karakteristik |
|-----|--------|---------------|
| 1970-an | Intel 8048 | 8-bit, memori terbatas, clock rendah |
| 1980-an | Motorola 68HC11 | Peripheral terintegrasi, EEPROM |
| 1990-an | PIC16, 8051 | Flash memory, lebih banyak peripheral |
| 2000-an | AVR, MSP430 | Konsumsi daya rendah, ADC terintegrasi |
| 2010-an | ARM Cortex-M | 32-bit, performa tinggi, ekosistem besar |
| 2020-an | ESP32, RISC-V | Multi-core, wireless terintegrasi, AI acceleration |

</div>

#### Aplikasi Microcontroller di Industri Modern

- **Otomotif**: Engine control, ADAS (Advanced Driver-Assistance Systems), dashboard
- **IoT (Internet of Things)**: Smart home, pemantauan lingkungan, tracking aset
- **Medis**: Perangkat diagnostik, monitor pasien, implan medis
- **Industri**: Kontrol otomasi, robotik, pemantauan keamanan
- **Konsumen**: Wearables, smart appliance, pengontrol game

### 🧩 Arsitektur ESP32 S3

ESP32 S3 mewakili generasi terbaru dari keluarga microcontroller ESP. Mari kita jelajahi apa yang membuatnya istimewa.

#### Fitur Utama ESP32 S3

```
╔══════════════════════════════════════════════════════════╗
║                    ESP32 S3 HIGHLIGHTS                    ║
╠══════════════════════════════════════════════════════════╣
║ ✓ Dual-core Xtensa LX7 processor (hingga 240 MHz)        ║
║ ✓ 512KB SRAM + PSRAM eksternal opsional (hingga 16MB)    ║
║ ✓ 2.4 GHz WiFi (802.11 b/g/n) dan Bluetooth 5 (LE)       ║
║ ✓ Neural Network Accelerator (untuk aplikasi AI edge)    ║
║ ✓ Interface USB OTG                                       ║
║ ✓ 45 pin GPIO yang dapat diprogram                        ║
║ ✓ Peripheral lengkap (SPI, I2C, I2S, UART, dll.)         ║
║ ✓ Mode konsumsi daya ultra-rendah                         ║
╚══════════════════════════════════════════════════════════╝
```

#### Arsitektur Multi-Core

<div align="center">
  
  ![ESP32 S3 Architecture](https://www.researchgate.net/publication/364250790/figure/fig1/AS:1141018780594176@1665068120158/Block-diagram-of-the-ESP32-S3-chip.png)

  *Diagram Blok ESP32 S3*
</div>

ESP32 S3 memiliki arsitektur dual-core yang memungkinkan:
- **Pemrosesan paralel**: Menjalankan tugas-tugas secara bersamaan
- **Pembagian tugas**: Satu core untuk aplikasi, satu core untuk konektivitas
- **Real-time processing**: Ideal untuk aplikasi yang membutuhkan respons cepat

#### Perbandingan dengan Microcontroller Lain

<div align="center">

| Fitur | ESP32 S3 | Arduino Uno | STM32F4 | Raspberry Pi Pico |
|-------|----------|-------------|---------|-------------------|
| CPU | Dual-core 240MHz | Single-core 16MHz | Single-core 168MHz | Dual-core 133MHz |
| Arsitektur | Xtensa LX7 | AVR | ARM Cortex-M4 | ARM Cortex-M0+ |
| RAM | 512KB + ext PSRAM | 2KB | 192KB | 264KB |
| Flash | 16MB (maks) | 32KB | 1MB | 2MB |
| Wireless | WiFi + BLE | Tidak ada | Tidak ada | Tidak ada |
| GPIO | 45 pins | 14 pins | 80+ pins | 26 pins |
| ADC | 12-bit, 20 channel | 10-bit, 6 channel | 12-bit, 16 channel | 12-bit, 4 channel |
| Harga (approx.) | $5-10 | $3-5 | $8-15 | $4 |

</div>

> 🌟 **Kenapa ESP32 S3?** ESP32 S3 menawarkan keseimbangan sempurna antara performa, fitur, dan harga. Wireless terintegrasi (WiFi dan Bluetooth) memudahkan pengembangan aplikasi IoT tanpa komponen tambahan, sementara Neural Network Accelerator memungkinkan aplikasi machine learning pada edge device.

---

## 📖 Minggu 2: Persiapan Lingkungan Pengembangan

### 🛠️ Menyiapkan Lingkungan Pengembangan

#### Arduino IDE sebagai Platform Pengembangan

Untuk perkuliahan ini, kita akan menggunakan **Arduino IDE** sebagai platform pengembangan utama karena beberapa keunggulan berikut:

- **Ramah untuk pemula**: Interface yang familiar dan mudah digunakan
- **Library yang kaya**: Ribuan library tersedia untuk berbagai sensor dan modul
- **Komunitas besar**: Dukungan komunitas yang luas untuk pemecahan masalah
- **Cross-platform**: Tersedia untuk Windows, macOS, dan Linux
- **Fleksibilitas**: Mendukung berbagai board ESP32, termasuk ESP32 S3

> 💡 **Insight**: Meskipun ESP-IDF (framework resmi Espressif) menawarkan akses ke semua fitur hardware ESP32 S3, Arduino IDE memberikan keseimbangan yang baik antara kemudahan penggunaan dan fungsionalitas yang cukup untuk sebagian besar aplikasi pembelajaran dan proyek.

#### Menginstal Arduino IDE

Berikut adalah langkah-langkah detail untuk menginstal dan menyiapkan Arduino IDE untuk ESP32 S3:

1. **Unduh Arduino IDE**
   - Kunjungi [situs resmi Arduino](https://www.arduino.cc/en/software)
   - Unduh versi terbaru yang sesuai dengan sistem operasi Anda
   - Instal software dengan mengikuti petunjuk instalasi

2. **Konfigurasi untuk ESP32 S3**
   - Buka Arduino IDE
   - Buka menu **File > Preferences** (Windows/Linux) atau **Arduino > Preferences** (macOS)
   - Pada field "Additional Board Manager URLs", tambahkan URL berikut:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
     ![Preferences Window](https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2022/01/Install-ESP32-S3-Arduino-IDE-additional-boards-manager-URL.png)
   - Klik "OK" untuk menyimpan pengaturan

3. **Instal Board ESP32**
   - Buka menu **Tools > Board > Boards Manager**
   - Dalam kotak pencarian, ketik "esp32"
   - Cari "ESP32 by Espressif Systems" dan klik "Install"
   - Tunggu hingga proses instalasi selesai (dapat memakan waktu beberapa menit tergantung koneksi internet)
   
4. **Pilih Board ESP32 S3**
   - Buka menu **Tools > Board > ESP32 Arduino**
   - Pilih "ESP32S3 Dev Module" dari daftar board
   - Konfigurasikan opsi tambahan sesuai kebutuhan:
     - **Upload Speed**: 921600 (disarankan)
     - **Flash Mode**: QIO
     - **Flash Size**: 8MB
     - **Partition Scheme**: Default
     - **Core Debug Level**: None (untuk produksi) atau Verbose (untuk debugging)
     - **Port**: Pilih port COM/Serial yang muncul saat ESP32 S3 terhubung

> 📝 **Catatan**: Jika port tidak muncul saat board terhubung, Anda mungkin perlu menginstal driver CH340/CP210x sesuai dengan IC USB-to-Serial yang digunakan pada board Anda. Informasi tentang driver dapat ditemukan dalam dokumentasi board.

### 🔄 Program Pertama: LED Berkedip

Mari kita mulai dengan program klasik "Hello World" dari dunia embedded: membuat LED berkedip!

```cpp
// Program Blink untuk ESP32 S3 menggunakan Arduino Framework

const int ledPin = 2;  // Pin untuk LED, biasanya pin 2 memiliki LED built-in

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32 S3 Blink Example");
  
  // Konfigurasi pin LED sebagai output
  pinMode(ledPin, OUTPUT);
}

void loop() {
  Serial.println("LED ON");   // Tampilkan status di Serial Monitor
  digitalWrite(ledPin, HIGH); // Nyalakan LED
  delay(1000);                // Tunggu 1 detik (1000 ms)
  
  Serial.println("LED OFF");  // Tampilkan status di Serial Monitor
  digitalWrite(ledPin, LOW);  // Matikan LED
  delay(1000);                // Tunggu 1 detik (1000 ms)
}
```

### 🐞 Debugging pada ESP32 S3 dengan Arduino IDE

#### Metode Debugging

1. **Serial Monitor**
   - Metode paling sederhana dan efektif untuk debugging
   - Menampilkan nilai variabel dan status program
   - Digunakan dengan fungsi `Serial.print()` dan `Serial.println()`
   - Buka dengan klik ikon kaca pembesar di pojok kanan atas Arduino IDE atau melalui menu **Tools > Serial Monitor**
   - Pastikan baud rate di Serial Monitor cocok dengan yang dideklarasikan di kode (`Serial.begin(115200)`)

2. **Serial Plotter**
   - Visualisasi data dalam bentuk grafik real-time
   - Sangat berguna untuk data sensor atau nilai yang berubah seiring waktu
   - Buka melalui menu **Tools > Serial Plotter**
   - Data harus dikirim dalam format numerik yang tepat

3. **LED Debugging**
   - Menggunakan LED sebagai indikator visual
   - Berguna saat Serial Monitor tidak dapat diakses
   - Pola kedipan berbeda dapat mengindikasikan status yang berbeda

4. **Menggunakan Library Debugging Tambahan**
   - RemoteDebug: Memungkinkan debugging melalui telnet
   - ArduinoOTA: Memungkinkan update kode secara nirkabel
   - SerialDebug: Library debugging yang lebih canggih dengan fitur tambahan

#### Tips Debugging Umum

```
🔍 TIPS DEBUGGING YANG EFEKTIF 🔍

1. Mulai dengan diagram alur sederhana dari program Anda
2. Tambahkan log pada titik-titik penting dalam kode
3. Verifikasi nilai-nilai yang diharapkan pada setiap langkah
4. Gunakan LED sebagai indikator visual sederhana
5. Isolasi masalah dengan mengomentari bagian kode
6. Periksa kembali koneksi hardware dan pin yang digunakan
7. Baca dokumentasi resmi jika mengalami masalah spesifik
```

### 💻 Latihan Praktikum: Berinteraksi dengan LED dan Button

#### Tujuan:
- Mengintegrasikan input dan output dasar
- Memahami state machine sederhana
- Mengimplementasikan debouncing button

#### Latihan 1: Toggle LED dengan Button

```cpp
const int buttonPin = 21;  // Pin untuk button
const int ledPin = 2;      // Pin untuk LED

// Variabel untuk melacak status
int ledState = LOW;          // Status LED saat ini
int lastButtonState = HIGH;  // Status button terakhir (HIGH saat tidak ditekan)
unsigned long lastDebounceTime = 0;  // Waktu terakhir button berubah status
unsigned long debounceDelay = 50;    // Waktu debounce (milliseconds)

void setup() {
  Serial.begin(115200);
  
  pinMode(buttonPin, INPUT_PULLUP);  // Gunakan pull-up internal
  pinMode(ledPin, OUTPUT);
  
  // Set initial LED state
  digitalWrite(ledPin, ledState);
}

void loop() {
  // Baca status button saat ini
  int reading = digitalRead(buttonPin);
  
  // Cek apakah button baru saja berubah status
  // karena noise atau penekanan sesungguhnya
  if (reading != lastButtonState) {
    // Reset timer debounce
    lastDebounceTime = millis();
  }
  
  // Cek apakah sudah lewat waktu debounce
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika status button telah berubah
    if (reading != buttonState) {
      buttonState = reading;
      
      // Hanya toggle LED jika button ditekan (LOW karena INPUT_PULLUP)
      if (buttonState == LOW) {
        ledState = !ledState;  // Balik status LED
        Serial.println(ledState ? "LED ON" : "LED OFF");
      }
    }
  }
  
  // Set status LED
  digitalWrite(ledPin, ledState);
  
  // Simpan status button untuk perbandingan di iterasi berikutnya
  lastButtonState = reading;
}
```

#### Latihan 2: LED Berkedip dengan Waktu Berbeda

```cpp
const int buttonPin = 21;  // Pin untuk button
const int ledPin = 2;      // Pin untuk LED

// Array untuk menyimpan durasi LED dalam ms
const int blinkDurations[] = {500, 1000, 2000, 3000};
const int numDurations = sizeof(blinkDurations) / sizeof(blinkDurations[0]);

int currentDurationIndex = 0;  // Indeks durasi saat ini
unsigned long previousMillis = 0;  // Waktu LED terakhir berubah status
int ledState = LOW;           // Status LED saat ini

// Variabel untuk debouncing
int lastButtonState = HIGH;  // Status button terakhir
unsigned long lastDebounceTime = 0;
unsigned long debounceDelay = 50;

void setup() {
  Serial.begin(115200);
  Serial.println("ESP32 S3 Multiple Blink Rate Example");
  
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin, OUTPUT);
  
  Serial.print("Current blink duration: ");
  Serial.println(blinkDurations[currentDurationIndex]);
}

void loop() {
  // Update LED berdasarkan interval saat ini
  unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= blinkDurations[currentDurationIndex]) {
    previousMillis = currentMillis;
    ledState = !ledState;
    digitalWrite(ledPin, ledState);
  }
  
  // Baca status button dengan debouncing
  int reading = digitalRead(buttonPin);
  
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }
  
  if ((millis() - lastDebounceTime) > debounceDelay) {
    if (reading != buttonState) {
      buttonState = reading;
      
      // Jika button ditekan, ganti kecepatan berkedip
      if (buttonState == LOW) {
        currentDurationIndex = (currentDurationIndex + 1) % numDurations;
        Serial.print("Changed blink duration to: ");
        Serial.println(blinkDurations[currentDurationIndex]);
      }
    }
  }
  
  lastButtonState = reading;
}
```

---

## 📝 Tugas Minggu Ini

1. **Instalasi Lingkungan**: Pasang ESP-IDF atau Arduino IDE pada komputer Anda, pastikan bisa terkoneksi dengan board ESP32 S3.

2. **Eksperimen Blink**: Modifikasi program blink untuk membuat pola kedipan yang lebih kompleks (morse code, pola tertentu, dll.).

3. **Mini Project**: Buat "lampu lalu lintas" sederhana dengan 3 LED (merah, kuning, hijau) yang beroperasi dengan pola waktu yang tepat.

4. **Tantangan Ekstra**: Tambahkan button untuk mengubah mode operasi lampu lalu lintas (normal, berkedip kuning, mati).

## 📚 Referensi dan Bacaan Lebih Lanjut

- Espressif Systems. (2022). *ESP32-S3 Series Datasheet* [Online]. Tersedia: https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf

- Espressif Systems. (2022). *ESP32-S3 Technical Reference Manual* [Online]. Tersedia: https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf

- Espressif Systems. (2023). *Arduino core for the ESP32, ESP32-S2, ESP32-S3, and ESP32-C3* [Online]. Tersedia: https://github.com/espressif/arduino-esp32

- Huang, R. (2021). *ESP32 Arduino Tutorials: ESP32-S3 Board* [Online]. Tersedia: https://RandomNerdTutorials.com/esp32-s3-board-arduino-ide

- Kolban, N. (2022). *Kolban's Book on ESP32: Programming with Arduino IDE*. Leanpub.

- McRoberts, M. (2023). *Beginning Arduino* (5th ed.). Apress. doi:10.1007/978-1-4842-8535-5

- MEAM. (2023). *Introduction to Embedded Systems with ESP32*. University of Pennsylvania [Online]. Tersedia: https://alliance.seas.upenn.edu/~meam620/wiki/index.php

- Seneviratne, P. (2022). *ESP32 Internet of Things Projects: Leveraging the ESP32 WiFi and Bluetooth Functionality for Building IoT Projects*. Packt Publishing.

---

<div align="center">
  
  ![QR Code](https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=https://github.com/yourusername/esp32-course)
  
  Scan untuk mengakses repositori lengkap
  
  *© 2025 Mata Kuliah Microcontroller*
</div>
