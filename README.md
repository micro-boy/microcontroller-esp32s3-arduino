# 🔌 Pembelajaran Microcontroller dengan ESP32 S3

<div align="center">
  <img src="https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/_images/esp32-s3-devkitc-1-v1.1-isometric.png" width="400" alt="ESP32-S3 DevKit">

  ![Arduino Compatible](https://img.shields.io/badge/Arduino-Compatible-brightgreen)
  ![License](https://img.shields.io/badge/License-MIT-blue)
  ![Status](https://img.shields.io/badge/Status-Active-success)
  
  *Materi perkuliahan microcontroller modern dengan ESP32 S3*
</div>

---

## 📚 Tentang Mata Kuliah

Repositori ini berisi seluruh materi untuk mata kuliah **Microcontroller** yang menggunakan platform ESP32 S3. Dirancang untuk mahasiswa teknik elektronika, informatika, dan bidang terkait lainnya, mata kuliah ini memberikan pemahaman komprehensif tentang sistem tertanam (embedded systems) modern.

Dengan fokus pada pendekatan hands-on dan project-based learning, mata kuliah ini menggabungkan teori dengan praktik langsung menggunakan ESP32 S3, sebuah microcontroller canggih dengan fitur-fitur seperti dual-core processor, WiFi, Bluetooth, dan Neural Network Accelerator.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan mata kuliah ini, mahasiswa diharapkan dapat:

- Memahami arsitektur dan prinsip kerja sistem microcontroller modern
- Mendesain dan mengimplementasikan solusi embedded systems untuk masalah dunia nyata
- Mengembangkan aplikasi IoT menggunakan konektivitas WiFi dan Bluetooth
- Mengimplementasikan teknik power management untuk aplikasi low-power
- Mengintegrasikan berbagai sensor dan aktuator dengan microcontroller
- Menerapkan konsep edge computing dan machine learning pada perangkat embedded

## 🧩 Prasyarat

Untuk mengikuti mata kuliah ini dengan baik, mahasiswa diharapkan memiliki:

- Pengetahuan dasar tentang pemrograman (ideally C/C++)
- Pemahaman dasar tentang rangkaian elektronika
- Kemampuan analisis dan pemecahan masalah
- Laptop dengan sistem operasi Windows, macOS, atau Linux
- Board ESP32 S3 (akan diinformasikan spesifikasi yang dibutuhkan)

## 🛠️ Persiapan Lingkungan Pengembangan

Mata kuliah ini menggunakan **Arduino IDE** sebagai platform pengembangan utama. Lihat [Modul 1](./modul1/) untuk petunjuk lengkap tentang:

- Instalasi Arduino IDE
- Konfigurasi untuk ESP32 S3
- Pengaturan board dan driver
- Testing komunikasi dengan board

## 📖 Struktur Materi

Mata kuliah ini terdiri dari 8 modul utama:

### [Modul 1: Pengenalan Embedded Systems dan ESP32 S3](./modul1/)
- Dasar-dasar sistem tertanam (embedded systems)
- Arsitektur dan fitur ESP32 S3
- Persiapan lingkungan pengembangan
- Program pertama: LED berkedip
- Teknik debugging dasar

### [Modul 2: Pemrograman dan Peripheral Dasar](./modul2/)
- Fundamental pemrograman embedded
- GPIO dan debouncing
- Timer dan PWM
- Analog-to-Digital Conversion
- Interfacing dengan sensor dasar

### [Modul 3: Komunikasi Protokol](./modul3/)
- Komunikasi Serial (UART)
- Protokol I²C dan implementasi
- SPI dan perangkat eksternal
- Display interfacing (OLED, LCD)

### [Modul 4: Konektivitas Wireless](./modul4/)
- Konfigurasi WiFi (Station & Access Point)
- Bluetooth Low Energy (BLE)
- Web server dan RESTful API
- Over-The-Air (OTA) updates

### [Modul 5: RTOS dan Multitasking](./modul5/)
- Konsep FreeRTOS
- Task management
- Synchronization (semaphore, mutex)
- Communication (queue, event groups)

### [Modul 6: Internet of Things](./modul6/)
- Protokol IoT (MQTT, HTTP)
- Cloud integration
- Dashboard visualization
- Security best practices

### [Modul 7: Power Management](./modul7/)
- Sleep modes
- Wake-up sources
- Battery-powered design
- Energy harvesting concepts

### [Modul 8: Edge Computing dan Machine Learning](./modul8/)
- TinyML concepts
- Sensor data processing
- Machine learning inferencing
- Praktik dengan TensorFlow Lite Micro

## 📝 Format Perkuliahan

Setiap modul terdiri dari:

- **Materi Teori**: Konsep dasar dan pengetahuan teknis
- **Tutorial Langkah-demi-Langkah**: Panduan praktis untuk implementasi
- **Latihan Praktikum**: Tugas hands-on untuk memperkuat pemahaman
- **Proyek Mini**: Aplikasi praktis dari konsep yang dipelajari
- **Quiz**: Evaluasi pemahaman materi

## 🚀 Proyek Akhir

Sebagai puncak dari mata kuliah ini, mahasiswa akan mengembangkan proyek akhir yang mengintegrasikan berbagai konsep yang telah dipelajari. Proyek ini akan melewati beberapa milestone:

1. **Proposal Proyek**: Definisi masalah dan solusi yang diusulkan
2. **Design Review**: Perencanaan hardware dan software
3. **Prototype**: Implementasi awal dan pengujian
4. **Final Demo**: Presentasi dan demonstrasi proyek lengkap
5. **Documentation**: Laporan teknis dan dokumentasi pengguna

## 🔖 Sistem Penilaian

- **Praktikum & Tugas**: 30%
- **Quiz**: 15%
- **Ujian Tengah Semester**: 20%
- **Proyek Akhir**: 25%
- **Partisipasi Kelas**: 10%

## 📚 Referensi & Sumber Daya

### Buku Teks Utama
- Kolban, N. (2022). *Kolban's Book on ESP32: Programming with Arduino IDE*. Leanpub.
- Seneviratne, P. (2022). *ESP32 Internet of Things Projects*. Packt Publishing.

### Referensi Online
- [Dokumentasi Resmi ESP32 S3](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/)
- [Arduino ESP32 Core Documentation](https://docs.espressif.com/projects/arduino-esp32/)
- [Random Nerd Tutorials - ESP32](https://randomnerdtutorials.com/esp32/)
- [ESP32 Technical Reference Manual](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

## 👥 Kontribusi

Mahasiswa dan pengajar dipersilakan untuk berkontribusi pada pengembangan materi ini melalui:

- Bug reporting: Melaporkan kesalahan atau typo
- Enhancement: Mengusulkan perbaikan atau tambahan materi
- Documentation: Membantu memperbaiki atau melengkapi dokumentasi

Untuk berkontribusi, silakan buat **pull request** atau laporkan **issue** di repositori ini.

## 📜 Lisensi

Materi dalam repositori ini dilisensikan di bawah [Lisensi MIT](LICENSE).

---

<div align="center">
  <p>
    Dikembangkan dengan 💙 untuk perkuliahan Microcontroller
    <br>
    © Departemen Informatika, Universitas Mulawarman, 2025
  </p>
</div>
