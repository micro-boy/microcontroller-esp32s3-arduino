# 🔌 Microcontroller dengan ESP32-S3

<div align="center">
  <img src="https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/_images/ESP32-S3_DevKitC-1_pinlayout.png" width="600">
  <p><em>ESP32-S3 Development Board</em></p>
  
  ![GitHub last commit](https://img.shields.io/github/last-commit/micro-boy/microcontroller-esp32s3-arduino)
  ![License](https://img.shields.io/badge/License-MIT-blue.svg)
  ![Arduino IDE](https://img.shields.io/badge/Arduino_IDE-Compatible-brightgreen)
</div>

## 📚 Deskripsi Kuliah

Selamat datang di kuliah **Microcontroller dengan ESP32-S3**! 

Kuliah ini dirancang untuk memperkenalkan Anda pada dunia menarik microcontroller modern menggunakan ESP32-S3, sebuah chip yang menggabungkan performansi tinggi, konektivitas, dan kemampuan AI. Selama perkuliahan, kita akan bergerak dari konsep dasar hingga aplikasi canggih yang relevan dengan kebutuhan industri saat ini.

Repositori ini berisi semua materi kuliah, contoh kode, skema rangkaian, dan proyek yang akan kita bahas bersama. Semua materi disusun dalam format Markdown untuk kemudahan pembacaan dan akses.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan kuliah ini, Anda akan mampu:

1. **Memahami** konsep dasar dan arsitektur microcontroller modern
2. **Mengkonfigurasi** lingkungan pengembangan Arduino IDE untuk ESP32-S3
3. **Mengimplementasikan** berbagai aplikasi input/output digital dan analog
4. **Memanfaatkan** protokol komunikasi (UART, I2C, SPI) untuk menghubungkan sensor dan aktuator
5. **Merancang** aplikasi dengan konektivitas nirkabel (WiFi dan Bluetooth)
6. **Mengembangkan** sistem multitasking dan real-time dengan FreeRTOS
7. **Menerapkan** praktik penghematan daya untuk aplikasi baterai
8. **Mengintegrasikan** machine learning sederhana pada edge device
9. **Membangun** proyek lengkap berbasis ESP32-S3 yang siap digunakan



## 📝 Daftar Modul

Kuliah ini terdiri dari 15 modul terstruktur yang membangun pengetahuan Anda secara progresif:

### Modul 1: Pengenalan dan Persiapan
- Pengenalan Microcontroller dan ESP32-S3
- Mempersiapkan Lingkungan Arduino IDE

### Modul 2: Dasar-Dasar Input/Output
- Kendali GPIO Digital
- Pemrosesan Sinyal Analog

### Modul 3: Komunikasi dan Tampilan
- Komunikasi Serial dan Protokol
- Menghubungkan Display dan Antarmuka Pengguna

### Modul 4: Pemrograman Lanjutan
- Interupsi dan Timer Hardware
- Manajemen Memori dan Penyimpanan

### Modul 5: Konektivitas Nirkabel
- WiFi dan Konektivitas Internet
- Bluetooth dan Komunikasi Nirkabel Lainnya

### Modul 6: Fitur Lanjutan ESP32-S3
- Multitasking dengan FreeRTOS
- Manajemen Daya

### Modul 7: Aplikasi Modern dan Proyek Terintegrasi
- Machine Learning pada ESP32-S3
- Keamanan dan OTA Updates
- Proyek Akhir Terintegrasi

## 💻 Prasyarat

Untuk mengikuti kuliah ini dengan optimal, Anda memerlukan:

### Pengetahuan Dasar
- Pemahaman dasar pemrograman (variabel, loop, kondisi)
- Konsep dasar elektronika (tegangan, arus, resistor)
- Tidak diperlukan pengetahuan mendalam tentang microcontroller sebelumnya

### Perangkat Keras
- Board ESP32-S3 (disarankan: ESP32-S3-DevKitC-1 atau ESP32-S3-WROOM)
- Kabel USB (Type-C atau sesuai dengan board Anda)
- Breadboard dan kabel jumper
- Komponen dasar (LED, resistor, pushbutton, potensiometer)
- Sensor umum (suhu, cahaya, dsb. - detail di setiap modul)

### Perangkat Lunak
- Arduino IDE (versi 2.0 atau yang lebih baru) 
- Driver USB sesuai dengan board ESP32-S3 Anda
- Git (opsional, untuk cloning repositori)

## 🚀 Memulai

### Kloning Repositori

```bash
git clone https://github.com/yourusername/esp32s3-microcontroller-course.git
cd esp32s3-microcontroller-course
```

### Mengatur Lingkungan

1. Instal Arduino IDE dari [arduino.cc/en/software](https://arduino.cc/en/software)
2. Tambahkan dukungan ESP32-S3 ke Arduino IDE (detail di [PERSIAPAN.md](PERSIAPAN.md))
3. Instal library yang diperlukan (detail per modul)
4. Siapkan hardware sesuai dengan modul yang sedang dipelajari

### Mulai Belajar

1. Mulai dari `modul-1/README.md` dan ikuti materi secara berurutan
2. Setiap modul berisi materi teoritis dan praktikum
3. Contoh kode tersedia di folder `code/` pada setiap modul
4. Kerjakan latihan mandiri untuk memperdalam pemahaman

## 📅 Jadwal Perkuliahan

| Minggu | Modul | Topik |
|--------|-------|-------|
| 1 | Modul 1 | Pengenalan dan Persiapan |
| 2 | Modul 2 | Dasar-Dasar Input/Output |
| 3 | Modul 3 | Komunikasi dan Tampilan |
| 4 | Modul 4 | Pemrograman Lanjutan |
| 5 | Modul 5 | Konektivitas Nirkabel |
| 6 | Modul 6 | Fitur Lanjutan ESP32-S3 |
| 7 | Modul 7 | Aplikasi Modern dan Proyek Terintegrasi |

## 📊 Penilaian

- **Tugas Praktikum**: 40%
- **Kuis Mingguan**: 20%
- **Proyek Tengah Semester**: 15%
- **Proyek Akhir**: 25%

Detail lengkap mengenai penilaian akan dijelaskan pada pertemuan pertama.

## 🔗 Sumber Daya Tambahan

- [Dokumentasi Resmi ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/index.html)
- [Arduino ESP32 GitHub Repository](https://github.com/espressif/arduino-esp32)
- [Espressif Systems Website](https://www.espressif.com/)
- [Forum Komunitas ESP32](https://esp32.com/)
- [Arduino Reference](https://www.arduino.cc/reference/en/)

## 📚 Referensi

Materi dalam perkuliahan ini disusun dengan mengacu pada sumber-sumber berikut:

1. Espressif Systems. (2023). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

2. Espressif Systems. (2023). *ESP32-S3 Datasheet*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)

3. Arduino. (2023). *Arduino Reference*. [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/)

4. Kolban, N. (2022). *Kolban's Book on ESP32*. Leanpub. [https://leanpub.com/kolban-ESP32](https://leanpub.com/kolban-ESP32)

5. Seneviratne, P. (2022). *ESP32 Internet of Things Projects: Leveraging the ESP32 Wi-Fi and Bluetooth Functionality for Building IoT Projects*. Packt Publishing.

6. Hughes, J. M. (2021). *Arduino: A Technical Reference*. O'Reilly Media.

7. Purdum, J. (2020). *Beginning C for Arduino: Learn C Programming for the Arduino*. Apress.

8. Molloy, D. (2022). *Exploring BeagleBone: Tools and Techniques for Building with Embedded Linux*. Wiley. (Referensi untuk konsep embedded systems)

9. Espressif Systems. (2023). *ESP-IDF Programming Guide*. [https://docs.espressif.com/projects/esp-idf/en/latest/](https://docs.espressif.com/projects/esp-idf/en/latest/)

10. MQTT.org. (2023). *MQTT Documentation*. [https://mqtt.org/](https://mqtt.org/) (Referensi untuk komunikasi IoT)

11. Waltenegus, D., & Christian, P. (2021). *Fundamentals of the Internet of Things: What the IoT Is and How It Works*. Artech House.

12. Harvey, D. (2021). *Circuit Design: Fundamentals and Examples*. Newnes.

Materi kuliah ini juga mengacu pada praktik terbaik industri dan komunitas pengembang ESP32 yang terus berkembang hingga tahun 2025.

## 📝 Lisensi

Materi dalam repositori ini dilisensikan di bawah [MIT License](LICENSE).



## 🤝 Kontribusi

Saran dan kontribusi dari mahasiswa untuk meningkatkan materi perkuliahan sangat dihargai. Silakan buat issue atau pull request jika Anda memiliki saran perbaikan.

Untuk diskusi lebih lanjut dan berbagi pengetahuan, Anda juga dapat bergabung dengan komunitas kami di Telegram: [Koding Indonesia](https://t.me/kodingindonesia)

---

<div align="center">
  <p>Dibuat dengan ❤️ untuk mahasiswa Teknik</p>
  <p><strong>Selamat belajar dan bereksperimen dengan ESP32-S3!</strong></p>
</div>
