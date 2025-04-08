# 📘 Modul 1: Pengenalan dan Persiapan ESP32-S3

<div align="center">
  <img src="https://github.com/user-attachments/assets/9ddf4b0b-0300-4beb-9ba3-29479c1c4e45" width="400">
  <p><em>ESP32-S3 Development Board - Microcontroller Modern dengan Kemampuan AI</em></p>
  
  <p>
    <img src="https://img.shields.io/badge/Tingkat-Pemula-brightgreen" alt="Tingkat: Pemula">
    <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
    <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
  </p>
</div>

## 📋 Daftar Isi

- [Tujuan Pembelajaran](#-tujuan-pembelajaran)
- [Pengenalan Microcontroller](#-pengenalan-microcontroller)
- [Mengenal ESP32-S3](#-mengenal-esp32-s3)
- [Mempersiapkan Lingkungan Arduino IDE](#-mempersiapkan-lingkungan-arduino-ide)
- [Persiapan Rangkaian](#-persiapan-rangkaian)
- [Program Pertama: Hello World ESP32-S3](#-program-pertama-hello-world-esp32-s3)
- [Latihan Mandiri](#-latihan-mandiri)
- [Sumber Referensi](#-sumber-referensi)

---

## 🎯 Tujuan Pembelajaran

Setelah mempelajari modul ini, Anda diharapkan dapat:

1. Memahami konsep dasar microcontroller dan perannya dalam sistem embedded
2. Menjelaskan arsitektur dan keunggulan ESP32-S3 dibandingkan microcontroller lain
3. Menginstal dan mengkonfigurasi Arduino IDE untuk pengembangan ESP32-S3
4. Menyiapkan rangkaian dasar untuk ESP32-S3
5. Menulis, mengunggah, dan men-debug program dasar pada ESP32-S3
6. Menggunakan Serial Monitor sebagai alat komunikasi dan debugging

---

## 🔍 Pengenalan Microcontroller

### Apa itu Microcontroller?

Microcontroller adalah komputer kecil terintegrasi dalam satu chip semikonduktor. Bayangkan sebuah komputer yang sangat kecil—tidak lebih besar dari ujung jari Anda—yang dirancang khusus untuk mengendalikan perangkat elektronik.

Microcontroller memiliki tiga komponen utama:

- **CPU (Central Processing Unit)**: Otak yang menjalankan instruksi program
- **Memori**: Penyimpanan untuk program (Flash/ROM) dan data (RAM)
- **Peripheral I/O**: Antarmuka dengan dunia luar (pin digital/analog, komunikasi, timer)

> 💡 **Tahukah Anda?** Microcontroller ada di hampir semua perangkat elektronik modern: dari mesin cuci, microwave, mobil, hingga perangkat medis.

### Perbedaan Microcontroller dan Microprocessor

| Aspek | Microcontroller | Microprocessor |
|-------|----------------|----------------|
| **Definisi** | Sistem komputer lengkap dalam satu chip | Hanya unit pemrosesan (CPU) |
| **Komponen** | CPU, memori, dan I/O terintegrasi | Membutuhkan chip eksternal untuk memori dan I/O |
| **Fokus Desain** | Kontrol dan interaksi dengan perangkat | Pemrosesan data dan komputasi |
| **Konsumsi Daya** | Sangat rendah (miliwatt) | Lebih tinggi (beberapa watt) |
| **Kecepatan** | Moderat (MHz) | Tinggi (GHz) |
| **Aplikasi** | Perangkat elektronik spesifik, IoT | Komputer, laptop, server |
| **Contoh** | ESP32, Arduino, STM32 | Intel Core i7, AMD Ryzen |

### Evolusi Microcontroller

Perkembangan microcontroller dari masa ke masa:

- **1970-an**: Intel 8048 - CPU 8-bit, memori sangat terbatas
- **1980-an**: Intel 8051, Motorola 68HC11 - Arsitektur 8-bit yang lebih baik
- **1990-an**: PIC dari Microchip, AVR dari Atmel - Arsitektur RISC, flash memory
- **2000-an**: ARM Cortex-M series, Arduino - Arsitektur 32-bit, efisiensi daya tinggi
- **2010-sekarang**: ESP32/ESP8266, STM32, Raspberry Pi RP2040 - Konektivitas wireless, AI/ML, multi-core

> 🔄 **Evolusi Berkelanjutan**: Microcontroller modern seperti ESP32-S3 merepresentasikan puncak evolusi ini, menggabungkan daya komputasi tinggi dengan konektivitas nirkabel dan kemampuan AI yang dahulu hanya dimiliki oleh komputer berukuran penuh.

---

## 🧠 Mengenal ESP32-S3

### Sejarah Singkat

ESP32-S3 merupakan bagian dari keluarga ESP32 yang dikembangkan oleh Espressif Systems:

- **2014**: Espressif meluncurkan ESP8266, microcontroller Wi-Fi berbiaya rendah
- **2016**: ESP32 generasi pertama diluncurkan dengan Bluetooth dan dual-core
- **2019**: ESP32-S2 diluncurkan, fokus pada keamanan dan USB OTG
- **2020**: ESP32-S3 diperkenalkan dengan peningkatan performa, AI/ML, dan keamanan
- **2021**: ESP32-S3 mulai tersedia secara luas di pasaran

### Arsitektur ESP32-S3

<div align="center">
   <img src="https://github.com/user-attachments/assets/833e0c24-0969-4076-9ff0-62fbcdc556db" width="600">
  <p><em>Diagram Blok ESP32-S3</em></p>
</div>

ESP32-S3 memiliki arsitektur yang komprehensif:

**1. Sistem Prosesor**:
- **CPU**: Dual-core Xtensa LX7 32-bit dengan clock hingga 240 MHz
- **Cache**: Memori cache untuk instruksi dan data
- **DMA**: Direct Memory Access untuk transfer data efisien
- **ROM**: Untuk bootloader dan fungsi dasar
- **SRAM**: 512 KB untuk data dan program

**2. Memori Eksternal**:
- **Flash**: Mendukung hingga 16 MB flash eksternal
- **PSRAM**: Mendukung hingga 8 MB PSRAM eksternal

**3. Sistem Wireless**:
- **Wi-Fi**: 802.11 b/g/n (2.4 GHz)
- **Bluetooth**: Bluetooth 5.0 (Classic & BLE)

**4. Peripheral dan Interface**:
- 45 GPIO pin
- 14 sensor sentuh kapasitif
- ADC 12-bit, DAC 8-bit
- UART, I2C, I2S, SPI, TWAI (CAN)
- LCD Interface, Camera Interface
- USB OTG

**5. Fitur Keamanan**:
- Secure Boot, Flash Encryption
- Hardware Accelerators untuk enkripsi

**6. Akselerator AI/ML**:
- Vector extensions untuk komputasi neural network

### Fitur Unggulan

ESP32-S3 memiliki beberapa keunggulan yang membuatnya menonjol:

1. **🔄 Pemrosesan Dual-Core**: Memungkinkan pembagian tugas yang efisien
2. **📡 Konektivitas Wireless Terintegrasi**: Wi-Fi dan Bluetooth dalam satu chip
3. **🤖 Kemampuan Machine Learning**: Vector DSP extensions mempercepat inferensi AI
4. **🔒 Keamanan Tingkat Lanjut**: Secure boot dan flash encryption
5. **🔌 Interface USB OTG**: Memudahkan koneksi dengan komputer dan perangkat lain
6. **🧰 Peripheral yang Kaya**: Banyaknya GPIO dan interface komunikasi
7. **🔋 Efisiensi Daya**: Mode sleep yang dapat mengurangi konsumsi daya hingga microampere

### Perbandingan dengan Microcontroller Lain

| Fitur | ESP32-S3 | Arduino Uno | Raspberry Pi Pico | STM32F4 | Teensy 4.1 |
|-------|----------|------------|-------------------|---------|------------|
| **CPU** | Dual-core 240 MHz | 16 MHz | Dual-core 133 MHz | 168 MHz | 600 MHz |
| **RAM** | 512 KB | 2 KB | 264 KB | 128-256 KB | 1 MB |
| **Flash** | Hingga 16 MB | 32 KB | 2 MB | 512 KB - 2 MB | 8 MB |
| **GPIO** | 45 pin | 14 pin | 26 pin | 16-140 pin | 55 pin |
| **Wi-Fi** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Bluetooth** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **USB** | OTG | Tidak | Micro USB | OTG (beberapa varian) | OTG |
| **AI Acceleration** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Harga Aprox.** | $5-10 | $3-5 | $4 | $5-15 | $25-30 |

> 🏆 **Keunggulan Kompetitif**: ESP32-S3 menawarkan keseimbangan unik antara performa tinggi, konektivitas wireless terintegrasi, dan harga yang terjangkau.

---

## ⚙️ Mempersiapkan Lingkungan Arduino IDE

### Instalasi Arduino IDE

Arduino IDE (Integrated Development Environment) adalah lingkungan pengembangan yang populer untuk microcontroller. Meskipun awalnya dibuat untuk board Arduino, saat ini Arduino IDE mendukung berbagai platform termasuk ESP32-S3.

<div align="center">
  <img src="https://github.com/user-attachments/assets/8586ba11-964d-439a-babf-f177ada76df4" width="600">
  <p><em>Tampilan Arduino IDE 2.0</em></p>
</div>

**Langkah Instalasi:**

1. **Download Arduino IDE**:
   - Kunjungi [arduino.cc/en/software](https://arduino.cc/en/software)
   - Pilih versi sesuai sistem operasi Anda (Windows, macOS, Linux)
   - Disarankan menggunakan Arduino IDE 2.0 atau yang lebih baru

2. **Instalasi pada Windows**:
   - Jalankan file .exe yang didownload
   - Ikuti petunjuk wizard instalasi
   - Pilih untuk menginstal driver USB jika ditawarkan

3. **Instalasi pada macOS**:
   - Buka file .dmg yang didownload
   - Seret ikon Arduino ke folder Applications
   - Pada saat pertama menjalankan, Anda mungkin perlu mengonfirmasi di System Preferences > Security & Privacy

4. **Instalasi pada Linux**:
   - Ekstrak file .tar.xz ke lokasi pilihan Anda
   - Jalankan script install.sh dari terminal
   - Atau gunakan package manager: `sudo apt-get install arduino` (untuk Ubuntu/Debian)

5. **Verifikasi Instalasi**:
   - Buka Arduino IDE
   - Pastikan tampilan utama muncul tanpa error

### Konfigurasi untuk ESP32-S3

Secara default, Arduino IDE tidak langsung mendukung ESP32-S3. Anda perlu menambahkan dukungan melalui Board Manager.

**Langkah Konfigurasi:**

1. **Tambahkan URL Board Manager**:
   - Buka Arduino IDE
   - Pilih **File > Preferences** (Windows/Linux) atau **Arduino > Preferences** (macOS)
   - Pada field "Additional Boards Manager URLs", tambahkan URL berikut:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Jika sudah ada URL sebelumnya, tambahkan dengan koma sebagai pemisah
   - Klik "OK" untuk menyimpan

2. **Instal ESP32 Board Package**:
   - Pilih **Tools > Board > Boards Manager**
   - Di kolom pencarian, ketik "esp32"
   - Cari paket bernama "ESP32 by Espressif Systems"
   - Pilih versi terbaru dan klik "Install"
   - Tunggu hingga proses selesai (bisa memakan waktu beberapa menit)

3. **Pilih Board ESP32-S3**:
   - Setelah instalasi selesai, pilih **Tools > Board > ESP32 Arduino**
   - Pilih "ESP32S3 Dev Module" dari daftar

4. **Konfigurasi Pengaturan Board Spesifik**:

<div align="center">
  <img src="https://github.com/user-attachments/assets/8389998c-335a-4b61-9e19-767cbaed0e16" width="600">
  <p><em>Konfigurasi Spesifik ESP32-S3 di Arduino IDE</em></p>
</div>

   Konfigurasi pilihan untuk ESP32-S3 di menu Tools:
   
   - **Board**: "ESP32S3 Dev Module"
   - **USB CDC On Boot**: "Enabled" (Aktifkan USB CDC saat boot)
   - **CPU Frequency**: "240MHz (WiFi)" (Frekuensi CPU optimal)
   - **Core Debug Level**: "None" (Tidak ada debug level untuk kinerja optimal)
   - **USB DFU On Boot**: "Enabled" (Aktifkan USB DFU saat boot)
   - **Upload Mode**: "UART0 / Hardware CDC" (Mode upload via UART)
   - **Flash Mode**: "QIO 80MHz" (Mode flash optimal)
   - **Flash Size**: "4MB" (Ukuran flash memori)
   - **Partition Scheme**: "Default 4MB with spiffs" (Skema partisi dengan SPIFFS)
   - **PSRAM**: "Disabled" (atau "OPI PSRAM" jika board mendukung)

   > ⚠️ **Penting**: Pengaturan di atas sangat krusial untuk memastikan ESP32-S3 beroperasi dengan benar. Pastikan Anda mengatur nilai yang tepat, terutama untuk "USB CDC On Boot", "CPU Frequency", dan "Flash Size".

5. **Instal Driver USB** (khusus Windows):
   - Untuk board berbasis CP210x: [Download Driver CP210x](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)
   - Untuk board berbasis CH340: [Download Driver CH340](http://www.wch.cn/download/CH341SER_EXE.html)
   - Jalankan installer driver dan ikuti petunjuknya
   - Restart komputer setelah menginstal driver

6. **Verifikasi Koneksi**:
   - Hubungkan board ESP32-S3 ke komputer dengan kabel USB
   - Periksa apakah board muncul di **Tools > Port**
   - Pilih port yang sesuai (misalnya COM30 di Windows)

> ⚠️ **Troubleshooting Koneksi**: Jika board tidak muncul di daftar port, coba kabel USB yang berbeda, coba port USB yang berbeda, tekan tombol BOOT saat mengunggah program, atau periksa driver terinstal dengan benar.

### Struktur Program Arduino

Program Arduino (disebut juga "sketch") memiliki struktur dasar dengan dua fungsi utama:

```cpp
// Deklarasi variabel global dan include library
#include <Arduino.h>

// Variabel global yang bisa diakses di semua fungsi
int ledPin = 2;
bool ledState = false;

// Fungsi setup() dijalankan sekali saat board dinyalakan atau di-reset
void setup() {
  // Inisialisasi pin, komunikasi serial, dan konfigurasi awal
  Serial.begin(115200);  // Memulai komunikasi serial dengan baud rate 115200
  pinMode(ledPin, OUTPUT);  // Mengatur pin LED sebagai output
  
  Serial.println("ESP32-S3 telah dimulai!");  // Mengirim pesan ke Serial Monitor
  delay(1000);  // Menunggu 1 detik agar Serial Monitor siap
}

// Fungsi loop() dijalankan berulang-ulang setelah setup() selesai
void loop() {
  // Kode utama program yang akan dijalankan berulang
  ledState = !ledState;  // Mengubah status LED (menyala/mati)
  
  digitalWrite(ledPin, ledState);  // Mengatur pin sesuai status baru
  
  if (ledState) {
    Serial.println("LED menyala");  // Mengirim pesan ketika LED menyala
  } else {
    Serial.println("LED mati");  // Mengirim pesan ketika LED mati
  }
  
  delay(1000);  // Menunggu selama 1 detik (1000 ms) sebelum mengulang
}
```

**Fungsi-fungsi Dasar Arduino:**

- `pinMode(pin, mode)`: Mengatur pin sebagai INPUT, OUTPUT, atau INPUT_PULLUP
- `digitalWrite(pin, value)`: Menulis nilai HIGH (1) atau LOW (0) ke pin digital
- `digitalRead(pin)`: Membaca nilai pin digital (HIGH atau LOW)
- `analogWrite(pin, value)`: Menghasilkan sinyal PWM (0-255) pada pin yang mendukung
- `analogRead(pin)`: Membaca nilai analog (0-4095 pada ESP32-S3) dari pin ADC
- `delay(ms)`: Menunda program selama beberapa milidetik
- `millis()`: Mendapatkan waktu (dalam milidetik) sejak program dimulai

### Mengenal Serial Monitor

Serial Monitor adalah alat yang sangat penting untuk debugging dan komunikasi antara ESP32-S3 dan komputer.

<div align="center">
  <img src="https://github.com/user-attachments/assets/87cb2e3a-e130-4381-911b-f0b2f7c56e36" width="500">
  <p><em>Serial Monitor Arduino IDE</em></p>
</div>

**Cara Menggunakan Serial Monitor:**

1. **Membuka Serial Monitor**:
   - Klik ikon kaca pembesar di pojok kanan atas Arduino IDE
   - Atau pilih **Tools > Serial Monitor**
   - Shortcut: Ctrl+Shift+M (Windows/Linux) atau Cmd+Shift+M (macOS)

2. **Mengatur Baud Rate**:
   - Pilih kecepatan yang sama dengan yang dikonfigurasi di kode `Serial.begin()`
   - Pilihan umum: 9600, 115200 (paling umum untuk ESP32-S3)

3. **Mengirim Data**:
   - Ketik teks di kolom input di bagian atas
   - Pilih opsi line ending (Newline, Carriage return, Both, No line ending)
   - Klik "Send" atau tekan Enter

4. **Menerima Data**:
   - Output dari fungsi `Serial.print()` dan `Serial.println()` akan muncul di jendela utama

> 💡 **Tip**: Tambahkan `Serial.println()` di berbagai titik program untuk melacak alur eksekusi dan nilai variabel. Ini sangat membantu untuk debugging.

---

## 🔌 Persiapan Rangkaian

Sebelum kita mulai memprogram ESP32-S3, kita perlu menyiapkan rangkaian dasar yang diperlukan. Rangkaian dasar ini akan digunakan untuk praktikum "Hello World" pertama kita.

### Komponen yang Dibutuhkan

Untuk rangkaian dasar ESP32-S3, Anda memerlukan:

1. **Board ESP32-S3 Development** (misalnya ESP32-S3-DevKitC-1)
2. **Kabel USB** (Type-C atau sesuai dengan port pada board Anda)
3. **LED** (disarankan menggunakan warna merah atau hijau)
4. **Resistor 220Ω atau 330Ω** (untuk membatasi arus ke LED)
5. **Breadboard** (untuk merangkai komponen)
6. **Kabel jumper** (untuk menghubungkan komponen)

### Diagram Pinout ESP32-S3

Sebelum merangkai, penting untuk memahami pinout ESP32-S3:

<div align="center">
  <img src="https://github.com/user-attachments/assets/666ad3c5-c36d-4dcc-abf3-a278675a21b7" width="700">
  <p><em>Pinout Diagram ESP32-S3-DevKitC-1</em></p>
</div>

Catatan penting tentang pinout:
- ESP32-S3 beroperasi pada level tegangan 3.3V (bukan 5V)
- GPIO2 biasanya terhubung ke LED bawaan pada sebagian besar development board
- Pin-pin diberi label dengan nomor GPIO dan fungsi alternatifnya
- Beberapa pin memiliki fungsi khusus, jadi periksa dokumentasi board Anda

### Rangkaian Dasar: LED External

Untuk rangkaian "Hello World" kita, kita akan menghubungkan LED eksternal ke ESP32-S3:

<div align="center">
  <img src="https://i.imgur.com/szgAFoM.png" width="600">
  <p><em>Rangkaian LED eksternal dengan ESP32-S3</em></p>
</div>

Langkah-langkah membuat rangkaian:

1. **Hubungkan ESP32-S3 ke breadboard**:
   - Pasang ESP32-S3 pada breadboard dengan hati-hati sesuai dengan posisi pin
   - Pastikan semua pin terhubung dengan baik ke breadboard

2. **Rangkai LED dan resistor**:
   - Pasang LED pada breadboard (perhatikan polaritas: kaki panjang adalah anoda/positif)
   - Hubungkan resistor 220Ω/330Ω antara kaki anoda LED dan pin GPIO yang akan digunakan
   - Dalam contoh ini, kita menggunakan GPIO13
   - Hubungkan kaki katoda LED (kaki pendek) ke pin GND pada ESP32-S3

3. **Koneksi Daya**:
   - ESP32-S3 akan mendapatkan daya melalui kabel USB yang terhubung ke komputer

### Rangkaian Alternatif: Menggunakan LED Bawaan

Jika board ESP32-S3 Anda memiliki LED bawaan (biasanya terhubung ke GPIO2), Anda dapat menggunakan LED tersebut tanpa komponen tambahan:

<div align="center">
  <img src="https://i.imgur.com/JyPEqWm.png" width="600">
  <p><em>Menggunakan LED bawaan pada ESP32-S3</em></p>
</div>

Untuk menggunakan LED bawaan:
1. Cukup hubungkan ESP32-S3 ke komputer menggunakan kabel USB
2. Dalam kode, gunakan pin GPIO2 untuk mengontrol LED bawaan
3. Tidak diperlukan komponen tambahan

> ⚠️ **Perhatian**: Beberapa board ESP32-S3 mungkin memiliki LED bawaan yang terhubung ke pin yang berbeda. Periksa dokumentasi board Anda untuk memastikan pin yang benar.

### Catatan Penting tentang Rangkaian

1. **Tegangan Operasi**:
   - ESP32-S3 beroperasi pada tegangan 3.3V
   - Jangan hubungkan langsung ke sumber 5V atau komponen 5V tanpa level shifter

2. **Arus Maksimum**:
   - Pin GPIO ESP32-S3 dapat memasok hingga 40mA per pin
   - Tetapi disarankan untuk membatasi arus di bawah 20mA untuk keamanan

3. **Resistor Pembatas Arus**:
   - Selalu gunakan resistor pembatas arus (220Ω-330Ω) saat menghubungkan LED
   - Tanpa resistor, LED dapat rusak atau menyebabkan kerusakan pada pin ESP32-S3

4. **Polaritas LED**:
   - LED memiliki polaritas: kaki anoda (lebih panjang) dihubungkan ke sisi positif/pin GPIO
   - Kaki katoda (lebih pendek) dihubungkan ke ground (GND)

5. **Koneksi USB**:
   - Gunakan kabel USB berkualitas baik yang mendukung transfer data
   - Beberapa kabel USB hanya untuk pengisian daya dan tidak mendukung komunikasi data

---

## 💡 Program Pertama: Hello World ESP32-S3

Mari kita buat program pertama untuk ESP32-S3, sebuah "Hello World" yang akan mengedipkan LED bawaan dan mengirim pesan melalui Serial Monitor.

### Persiapan Hardware

Sebelum mulai memprogram, pastikan Anda memiliki:
- Board ESP32-S3 yang terhubung ke komputer melalui kabel USB
- Kabel USB yang mendukung transfer data, bukan hanya untuk charging
- Port USB yang berfungsi dengan baik
- Rangkaian LED (bawaan atau eksternal) seperti yang dijelaskan pada bagian sebelumnya

### Kode Program

Berikut adalah kode lengkap untuk program "Hello World" kita:

```cpp
/*
 * Program Hello World ESP32-S3
 * Fungsi: Mengedipkan LED bawaan dan mengirim informasi ke Serial Monitor
 * Platform: ESP32-S3
 * Framework: Arduino IDE
 */

// Definisikan nomor pin untuk LED bawaan
// Catatan: Pin ini mungkin berbeda tergantung pada board ESP32-S3 yang digunakan
#define LED_PIN 2

// Variabel untuk menghitung jumlah kedipan
int counter = 0;

// Variabel untuk menyimpan waktu mulai program
unsigned long startTime;

void setup() {
  // Inisialisasi komunikasi serial dengan kecepatan 115200 bps
  Serial.begin(115200);
  
  // Berikan sedikit waktu agar Serial Monitor siap
  delay(1000);
  
  // Catat waktu mulai program
  startTime = millis();
  
  // Tampilkan pesan selamat datang
  Serial.println("\n\n===================================");
  Serial.println("    HELLO WORLD ESP32-S3!");
  Serial.println("===================================");
  Serial.println("Program ini akan mengedipkan LED dan");
  Serial.println("mengirim informasi ke Serial Monitor");
  
  // Konfigurasi pin LED sebagai output
  pinMode(LED_PIN, OUTPUT);
  
  // Matikan LED pada awalnya
  digitalWrite(LED_PIN, LOW);
  
  // Tampilkan informasi tentang board
  Serial.print("Chip model: ");
  Serial.println(ESP.getChipModel());
  Serial.print("Chip cores: ");
  Serial.println(ESP.getChipCores());
  Serial.print("CPU frequency: ");
  Serial.print(ESP.getCpuFreqMHz());
  Serial.println(" MHz");
  Serial.print("Flash size: ");
  Serial.print(ESP.getFlashChipSize() / 1024 / 1024);
  Serial.println(" MB");
  
  Serial.println("\nProgram dimulai!");
  Serial.println("-----------------------------------");
}

void loop() {
  // Hitung waktu berjalan
  unsigned long runningTime = millis() - startTime;
  unsigned long seconds = runningTime / 1000;
  
  // Nyalakan LED
  digitalWrite(LED_PIN, HIGH);
  
  // Tambahkan hitungan
  counter++;
  
  // Tampilkan status
  Serial.print("Kedipan ke-");
  Serial.print(counter);
  Serial.print(" (waktu berjalan: ");
  Serial.print(seconds);
  Serial.println("s)");
  Serial.println("LED menyala");
  
  // Tunda selama LED menyala
  delay(500);
  
  // Matikan LED
  digitalWrite(LED_PIN, LOW);
  Serial.println("LED mati");
  
  // Tunda lebih lama saat LED mati
  delay(1500);
  
  // Tampilkan rangkuman setiap 10 kedipan
  if (counter % 10 == 0) {
    Serial.println("\n=================================");
    Serial.print("RANGKUMAN: LED telah berkedip ");
    Serial.print(counter);
    Serial.print(" kali dalam ");
    Serial.print(seconds);
    Serial.println(" detik");
    Serial.println("=================================\n");
  }
}
```

### Penjelasan Kode

**1. Definisi dan Variabel Global:**
- `#define LED_PIN 2` mendefinisikan konstanta untuk pin LED (sesuaikan dengan board Anda)
- `counter` menyimpan jumlah kedipan LED
- `startTime` menyimpan waktu mulai program dalam milidetik

**2. Fungsi setup():**
- Inisialisasi komunikasi serial dengan kecepatan 115200 bps
- Menunggu 1 detik agar Serial Monitor siap
- Mencatat waktu mulai program
- Menampilkan pesan selamat datang dan informasi board
- Mengonfigurasi pin LED sebagai output
- Mematikan LED pada awalnya

**3. Informasi Board:**
- Menggunakan fungsi dari ESP32 API untuk mendapatkan informasi hardware
- Menampilkan model chip, jumlah core, frekuensi CPU, dan ukuran flash

**4. Fungsi loop():**
- Menghitung waktu berjalan program
- Menyalakan LED dengan `digitalWrite(LED_PIN, HIGH)`
- Menambah hitungan kedipan
- Menampilkan status ke Serial Monitor
- Menunggu 500ms (0.5 detik) dengan LED menyala
- Mematikan LED dengan `digitalWrite(LED_PIN, LOW)`
- Menunggu 1500ms (1.5 detik) dengan LED mati
- Setiap 10 kedipan, menampilkan rangkuman

### Langkah Mengunggah Program

1. **Sambungkan Board**:
   - Hubungkan board ESP32-S3 ke komputer menggunakan kabel USB
   - Pastikan board terdeteksi di Arduino IDE (akan muncul di **Tools > Port**)

2. **Verifikasi Konfigurasi**:
   - Periksa bahwa board ESP32-S3 dipilih di **Tools > Board**
   - Pastikan port yang benar dipilih di **Tools > Port**
   - Verifikasi pengaturan board sudah sesuai seperti yang disebutkan di bagian [Konfigurasi Pengaturan Board Spesifik](#konfigurasi-untuk-esp32-s3)

3. **Verifikasi Kode**:
   - Klik tombol "Verify" (centang) atau tekan Ctrl+R (Windows/Linux) / Cmd+R (macOS)
   - Tunggu hingga proses kompilasi selesai
   - Jika ada error, perbaiki kode sesuai pesan error yang muncul

4. **Unggah Program**:
   - Klik tombol "Upload" (panah ke kanan) atau tekan Ctrl+U (Windows/Linux) / Cmd+U (macOS)
   - Tunggu proses kompilasi dan unggah selesai
   - Pada beberapa board, Anda perlu menekan tombol BOOT selama proses unggah

5. **Memantau Proses**:
   - Arduino IDE akan menampilkan kemajuan unggah di panel bawah
   - Pesan "Done uploading" menunjukkan proses berhasil

### Debugging Program

Setelah program terunggah, mari verifikasi bahwa program berjalan dengan benar:

1. **Buka Serial Monitor**:
   - Klik ikon kaca pembesar di pojok kanan atas Arduino IDE
   - Atau pilih **Tools > Serial Monitor**
   - Pastikan baud rate diatur ke 115200

2. **Amati Output Serial**:
   - Anda akan melihat pesan selamat datang dan informasi board
   - Kemudian program akan menampilkan status LED yang berkedip
   - Setiap 10 kedipan, rangkuman akan ditampilkan

3. **Verifikasi Kedipan LED**:
   - LED pada board ESP32-S3 harus berkedip sesuai dengan pola yang diprogram:
     - Menyala selama 0.5 detik
     - Mati selama 1.5 detik

**Troubleshooting:**

- **Jika LED Tidak Berkedip**: Periksa nomor pin yang digunakan (`LED_PIN`). Beberapa board menggunakan pin berbeda untuk LED bawaan. Coba ubah nilai `LED_PIN` ke nilai lain (misalnya 13, 5, atau 9) sesuai dengan dokumentasi board Anda. Jika menggunakan LED eksternal, periksa rangkaian Anda.

- **Jika Serial Monitor Tidak Menampilkan Output**: Periksa bahwa baud rate di Serial Monitor sama dengan yang dikonfigurasi di kode (115200). Coba reset board dengan menekan tombol RESET. Pastikan kabel USB terhubung dengan baik dan mendukung transfer data.

- **Jika Program Tidak Terunggah**: Periksa apakah port yang benar dipilih. Pada beberapa board ESP32-S3, Anda perlu menekan dan tahan tombol BOOT saat mengklik "Upload", kemudian lepaskan setelah upload dimulai. Coba dengan kabel USB yang berbeda atau port USB yang berbeda.

> 💡 **Tip Pro**: Untuk memverifikasi bahwa board Anda berfungsi dengan benar tanpa masalah kode, coba unggah program contoh yang sudah teruji seperti **File > Examples > 01.Basics > Blink**. Jangan lupa untuk mengganti nomor pin LED jika diperlukan.

---

## 🏋️ Latihan Mandiri

Sekarang saatnya Anda mencoba sendiri untuk memastikan pemahaman. Cobalah tantangan-tantangan berikut:

### Latihan 1: Modifikasi Pola Kedipan

Ubah kode untuk membuat pola kedipan yang lebih kompleks:
- Buat LED berkedip cepat 3 kali (setiap kedipan 200ms menyala, 200ms mati)
- Kemudian buat jeda panjang (2 detik)
- Ulangi pola tersebut

<details>
<summary>Petunjuk</summary>

```cpp
void loop() {
  // Kedip cepat 3 kali
  for(int i=0; i<3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(200);
    digitalWrite(LED_PIN, LOW);
    delay(200);
  }
  
  // Jeda panjang
  delay(2000);
}
```
</details>

### Latihan 2: Menambahkan Informasi Running Time yang Lebih Rinci

Modifikasi program untuk menampilkan waktu berjalan dalam format jam:menit:detik:
- Gunakan `millis()` untuk menghitung waktu berjalan
- Konversi ke format jam:menit:detik
- Tampilkan di setiap iterasi loop

<details>
<summary>Petunjuk</summary>

```cpp
unsigned long runningTime = millis() - startTime;
unsigned long seconds = (runningTime / 1000) % 60;
unsigned long minutes = (runningTime / (1000 * 60)) % 60;
unsigned long hours = (runningTime / (1000 * 60 * 60)) % 24;

Serial.printf("Waktu berjalan: %02d:%02d:%02d\n", hours, minutes, seconds);
```
</details>

### Latihan 3: Menambahkan Tombol Reset Counter

Jika Anda memiliki push button atau dapat menambahkan satu, modifikasi program untuk:
- Membaca input dari tombol yang terhubung ke pin GPIO
- Mereset counter ke 0 ketika tombol ditekan
- Menampilkan pesan "Counter di-reset" di Serial Monitor

<details>
<summary>Petunjuk</summary>

```cpp
#define BUTTON_PIN 5  // Ganti dengan pin yang sesuai

void setup() {
  // ...kode yang sudah ada...
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Menggunakan resistor pull-up internal
}

void loop() {
  // Cek apakah tombol ditekan (akan memberikan nilai LOW karena pull-up)
  if (digitalRead(BUTTON_PIN) == LOW) {
    counter = 0;
    Serial.println("Counter di-reset!");
    delay(300);  // Debouncing sederhana
  }
  
  // ...kode yang sudah ada...
}
```
</details>

### Latihan 4: Implementasi Kontrol Melalui Serial

Ubah program untuk menerima perintah dari Serial Monitor:
- Saat "on" dikirim, LED menyala terus
- Saat "off" dikirim, LED mati terus
- Saat "blink" dikirim, LED kembali ke mode berkedip
- Saat "status" dikirim, tampilkan status sistem saat ini

<details>
<summary>Petunjuk</summary>

```cpp
// Tambahkan variabel mode
String mode = "blink";  // Mode default: berkedip

void loop() {
  // Periksa perintah serial
  if (Serial.available() > 0) {
    String command = Serial.readStringUntil('\n');
    command.trim();  // Menghapus whitespace
    
    if (command == "on") {
      mode = "on";
      Serial.println("Mode: ON - LED akan menyala terus");
    } 
    else if (command == "off") {
      mode = "off";
      Serial.println("Mode: OFF - LED akan mati terus");
    }
    else if (command == "blink") {
      mode = "blink";
      Serial.println("Mode: BLINK - LED akan berkedip");
    }
    else if (command == "status") {
      Serial.println("Status saat ini:");
      Serial.println("Mode: " + mode);
      Serial.println("Jumlah kedipan: " + String(counter));
      // Tambahkan informasi lain yang relevan
    }
  }
  
  // Logika berdasarkan mode
  if (mode == "on") {
    digitalWrite(LED_PIN, HIGH);
    delay(1000);  // Hanya untuk memberi jeda antar iterasi
  }
  else if (mode == "off") {
    digitalWrite(LED_PIN, LOW);
    delay(1000);  // Hanya untuk memberi jeda antar iterasi
  }
  else if (mode == "blink") {
    // Kode kedipan yang sudah ada
    digitalWrite(LED_PIN, HIGH);
    // ...
    delay(500);
    digitalWrite(LED_PIN, LOW);
    // ...
    delay(1500);
  }
}
```
</details>

---

## 📚 Sumber Referensi

1. Espressif Systems. (2023). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

2. Espressif Systems. (2023). *ESP32-S3 Datasheet*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)

3. Arduino. (2023). *Arduino Reference*. [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/)

4. Espressif Systems. (2023). *ESP-IDF Programming Guide*. [https://docs.espressif.com/projects/esp-idf/en/latest/](https://docs.espressif.com/projects/esp-idf/en/latest/)

5. RandomNerdTutorials. (2023). *ESP32 Tutorials*. [https://randomnerdtutorials.com/esp32/](https://randomnerdtutorials.com/esp32/)

---

**Modul berikutnya**: Dasar-Dasar Input/Output dengan ESP32-S3
