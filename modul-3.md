# 📘 Modul 3: Komunikasi dan Tampilan dengan ESP32-S3

<div align="center">
  <img src="https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/_images/esp32-s3-korvo-2-v3.0-top-view.png" width="400">
  
  <p><em>ESP32-S3 - Menghubungkan Dunia Digital Melalui Protokol Komunikasi dan Antarmuka Visual</em></p>
  
  <p>
    <img src="https://img.shields.io/badge/Tingkat-Pemula_Lanjutan-yellow" alt="Tingkat: Pemula Lanjutan">
    <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
    <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
  </p>
</div>

## 📋 Daftar Isi

- [Tujuan Pembelajaran](#-tujuan-pembelajaran)
- [Komunikasi Serial dan Protokol](#-komunikasi-serial-dan-protokol)
  - [UART: Komunikasi Serial Dasar](#uart-komunikasi-serial-dasar)
  - [I2C: Komunikasi Multi-Perangkat](#i2c-komunikasi-multi-perangkat)
  - [SPI: Komunikasi Cepat](#spi-komunikasi-cepat)
  - [Pemilihan Protokol yang Tepat](#pemilihan-protokol-yang-tepat)
- [Menghubungkan Display dan Antarmuka Pengguna](#-menghubungkan-display-dan-antarmuka-pengguna)
  - [OLED Display dengan I2C](#oled-display-dengan-i2c)
  - [LCD Display dengan I2C](#lcd-display-dengan-i2c)
  - [Desain Antarmuka Pengguna Sederhana](#desain-antarmuka-pengguna-sederhana)
- [Praktikum 1: Stasiun Cuaca Mini dengan Display](#-praktikum-1-stasiun-cuaca-mini-dengan-display)
- [Praktikum 2: Sistem Dashboard Multi-Sensor](#-praktikum-2-sistem-dashboard-multi-sensor)
- [Latihan Mandiri](#-latihan-mandiri)
- [Sumber Referensi](#-sumber-referensi)

---

## 🎯 Tujuan Pembelajaran

Setelah mempelajari modul ini, Anda diharapkan dapat:

1. Menjelaskan dan mengimplementasikan protokol komunikasi serial (UART, I2C, SPI)
2. Menghubungkan ESP32-S3 dengan sensor dan perangkat eksternal menggunakan berbagai protokol
3. Mengintegrasikan dan mengontrol display OLED dan LCD
4. Merancang antarmuka pengguna sederhana pada display
5. Mengembangkan aplikasi yang mengombinasikan input sensor dan output visual
6. Mengatasi masalah umum dalam komunikasi antar-perangkat

---

## 🔄 Komunikasi Serial dan Protokol

Komunikasi antar perangkat adalah komponen penting dalam sistem embedded modern. ESP32-S3 mendukung berbagai protokol komunikasi yang memungkinkan pertukaran data dengan sensor, aktuator, display, dan komputer.

### UART: Komunikasi Serial Dasar

UART (Universal Asynchronous Receiver-Transmitter) adalah protokol komunikasi serial asinkron paling dasar dan serbaguna.

<div align="center">
  <img src="https://i.imgur.com/AjfBDj9.png" width="500">
  <p><em>Komunikasi UART antara Dua Perangkat</em></p>
</div>

**Karakteristik UART:**
- **Komponen**: TX (Transmit) dan RX (Receive)
- **Kecepatan**: Biasanya 9600 - 115200 baud (bit per detik)
- **Jumlah Wire**: Minimal 2 wire (TX/RX) + ground
- **Mode Komunikasi**: Full-duplex (dua arah sekaligus)
- **Pengalamatan**: Point-to-point (satu-ke-satu)

ESP32-S3 memiliki tiga perangkat UART hardware:
- UART0: Biasanya digunakan untuk komunikasi dengan komputer via USB
- UART1 dan UART2: Tersedia untuk aplikasi pengguna

**Menggunakan UART di ESP32-S3:**

```cpp
// Inisialisasi UART2 (GPIO17: TX, GPIO18: RX)
#define UART_TX_PIN 17
#define UART_RX_PIN 18
#define UART_BAUD_RATE 9600

void setup() {
  // Inisialisasi Serial monitor (UART0)
  Serial.begin(115200);
  
  // Inisialisasi UART2
  Serial2.begin(UART_BAUD_RATE, SERIAL_8N1, UART_RX_PIN, UART_TX_PIN);
  
  Serial.println("Inisialisasi UART selesai");
}

void loop() {
  // Mengirim data ke perangkat melalui UART2
  Serial2.println("Hello from ESP32-S3!");
  
  // Menerima data dari perangkat melalui UART2
  if (Serial2.available()) {
    String receivedData = Serial2.readStringUntil('\n');
    Serial.print("Data diterima: ");
    Serial.println(receivedData);
  }
  
  delay(1000);
}
```

**Aplikasi UART Umum:**
- Debugging (Serial Monitor)
- Komunikasi dengan GPS, modul Bluetooth, atau modul GSM
- Komunikasi dengan microcontroller lain
- Koneksi ke komputer

> 💡 **Tip**: Saat menggunakan level logika yang berbeda (misalnya, perangkat 5V dengan ESP32-S3 yang 3.3V), gunakan level shifter untuk menghindari kerusakan.

### I2C: Komunikasi Multi-Perangkat

I2C (Inter-Integrated Circuit) adalah protokol komunikasi serial sinkron yang memungkinkan banyak perangkat berbagi bus yang sama.

<div align="center">
  <img src="https://i.imgur.com/bHrJnrq.png" width="500">
  <p><em>Konfigurasi Bus I2C dengan Multiple Slave</em></p>
</div>

**Karakteristik I2C:**
- **Komponen**: SDA (Serial Data) dan SCL (Serial Clock)
- **Kecepatan**: Standard (100 kHz), Fast (400 kHz), Fast Plus (1 MHz)
- **Jumlah Wire**: 2 wire + power + ground
- **Mode Komunikasi**: Half-duplex
- **Pengalamatan**: Master-Slave dengan alamat unik (7-bit atau 10-bit)
- **Jumlah Device**: Hingga 127 perangkat (alamat 7-bit)

ESP32-S3 mendukung dua controller I2C hardware yang dapat dikonfigurasi ke hampir semua pin GPIO.

**Menggunakan I2C di ESP32-S3:**

```cpp
#include <Wire.h>

#define I2C_SDA_PIN 21
#define I2C_SCL_PIN 22
#define I2C_FREQUENCY 400000  // 400 kHz

void setup() {
  Serial.begin(115200);
  
  // Inisialisasi I2C
  Wire.begin(I2C_SDA_PIN, I2C_SCL_PIN, I2C_FREQUENCY);
  
  Serial.println("Scanning I2C bus...");
  scanI2CDevices();
}

void loop() {
  delay(5000);
}

// Fungsi untuk memindai perangkat I2C yang terhubung
void scanI2CDevices() {
  byte error, address;
  int deviceCount = 0;
  
  for (address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    
    if (error == 0) {
      Serial.print("Perangkat I2C ditemukan pada alamat 0x");
      if (address < 16) {
        Serial.print("0");
      }
      Serial.print(address, HEX);
      Serial.println();
      deviceCount++;
    }
  }
  
  if (deviceCount == 0) {
    Serial.println("Tidak ditemukan perangkat I2C");
  } else {
    Serial.print("Jumlah perangkat ditemukan: ");
    Serial.println(deviceCount);
  }
}
```

**Menggunakan I2C untuk Komunikasi dengan Sensor:**

```cpp
#include <Wire.h>
#include <Adafruit_BME280.h>

#define I2C_SDA_PIN 21
#define I2C_SCL_PIN 22
#define BME280_ADDR 0x76  // Cek alamat sensor BME280 Anda (0x76 atau 0x77)

Adafruit_BME280 bme;

void setup() {
  Serial.begin(115200);
  Wire.begin(I2C_SDA_PIN, I2C_SCL_PIN);
  
  // Inisialisasi sensor BME280
  if (!bme.begin(BME280_ADDR, &Wire)) {
    Serial.println("Tidak dapat menemukan sensor BME280!");
    while (1) delay(10);
  }
  
  Serial.println("Sensor BME280 ditemukan!");
}

void loop() {
  // Baca data dari sensor
  float temperature = bme.readTemperature();
  float pressure = bme.readPressure() / 100.0F;  // hPa
  float humidity = bme.readHumidity();
  
  // Tampilkan data
  Serial.print("Suhu: ");
  Serial.print(temperature);
  Serial.println(" °C");
  
  Serial.print("Tekanan: ");
  Serial.print(pressure);
  Serial.println(" hPa");
  
  Serial.print("Kelembaban: ");
  Serial.print(humidity);
  Serial.println(" %");
  
  Serial.println("--------------------");
  delay(2000);
}
```

**Aplikasi I2C Umum:**
- Sensor (suhu, tekanan, kelembaban, akselerometer, dll.)
- EEPROM dan penyimpanan lainnya
- Display (OLED, LCD)
- Expander I/O
- Real-Time Clock (RTC)

> 💡 **Tip**: I2C memerlukan resistor pull-up pada kedua jalur (SDA dan SCL). Beberapa modul sudah memiliki resistor pull-up bawaan, tapi jika menggunakan banyak perangkat atau kabel panjang, Anda mungkin perlu menambahkan resistor eksternal (biasanya 4.7kΩ).

### SPI: Komunikasi Cepat

SPI (Serial Peripheral Interface) adalah protokol komunikasi serial sinkron yang menawarkan kecepatan tinggi untuk pertukaran data.

<div align="center">
  <img src="https://i.imgur.com/QnJZGkI.png" width="500">
  <p><em>Konfigurasi SPI dengan Multiple Slave</em></p>
</div>

**Karakteristik SPI:**
- **Komponen**: MOSI (Master Out Slave In), MISO (Master In Slave Out), SCK (Serial Clock), CS/SS (Chip Select)
- **Kecepatan**: Sangat cepat, hingga beberapa MHz
- **Jumlah Wire**: 3 wire shared (MOSI, MISO, SCK) + 1 wire per slave (CS) + power + ground
- **Mode Komunikasi**: Full-duplex
- **Pengalamatan**: Master-Slave dengan pin CS terpisah untuk setiap slave
- **Jumlah Device**: Dibatasi oleh jumlah pin GPIO yang tersedia

ESP32-S3 memiliki hingga 3 perangkat SPI hardware:
- SPI0/SPI1: Digunakan internal untuk flash memory
- SPI2/SPI3: Tersedia untuk aplikasi pengguna

**Menggunakan SPI di ESP32-S3:**

```cpp
#include <SPI.h>

#define SPI_SCK_PIN  18
#define SPI_MISO_PIN 19
#define SPI_MOSI_PIN 23
#define SPI_CS_PIN   5

void setup() {
  Serial.begin(115200);
  
  // Konfigurasi pin SPI
  pinMode(SPI_CS_PIN, OUTPUT);
  digitalWrite(SPI_CS_PIN, HIGH);  // Inactive
  
  // Inisialisasi SPI
  SPI.begin(SPI_SCK_PIN, SPI_MISO_PIN, SPI_MOSI_PIN);
  SPI.setFrequency(1000000);  // 1 MHz
  SPI.setDataMode(SPI_MODE0);
  SPI.setBitOrder(MSBFIRST);
  
  Serial.println("SPI diinisialisasi");
}

void loop() {
  // Contoh: Mengirim data ke perangkat SPI
  byte dataToSend = 0xA5;
  
  // Aktifkan slave
  digitalWrite(SPI_CS_PIN, LOW);
  
  // Kirim data dan baca respons
  byte response = SPI.transfer(dataToSend);
  
  // Nonaktifkan slave
  digitalWrite(SPI_CS_PIN, HIGH);
  
  Serial.print("Data terkirim: 0x");
  Serial.print(dataToSend, HEX);
  Serial.print(", Respons: 0x");
  Serial.println(response, HEX);
  
  delay(1000);
}
```

**Aplikasi SPI Umum:**
- Display (LCD, TFT)
- Kartu SD dan penyimpanan lainnya
- Sensor kecepatan tinggi
- Penerima dan pemancar radio (nRF24L01, RFM95)
- Modul ethernet (W5500, ENC28J60)

> 💡 **Tip**: Untuk aplikasi dengan beberapa slave SPI, Anda hanya perlu satu set MOSI, MISO, dan SCK. Setiap slave memerlukan pin CS terpisah. Cukup aktifkan pin CS untuk perangkat yang ingin Anda ajak berkomunikasi.

### Pemilihan Protokol yang Tepat

Pemilihan protokol komunikasi tergantung pada kebutuhan spesifik aplikasi Anda:

| Protokol | Kelebihan | Kekurangan | Cocok untuk |
|----------|-----------|------------|-------------|
| **UART** | - Sederhana dan didukung luas<br>- Full-duplex<br>- Tidak perlu clock | - Point-to-point<br>- Tidak ada mekanisme pengecekan error bawaan<br>- Kecepatan terbatas | - Debugging<br>- Komunikasi dengan komputer<br>- Modul GPS, Bluetooth, dll. |
| **I2C** | - Mendukung banyak perangkat<br>- Hanya butuh 2 kabel<br>- Pengalamatan perangkat | - Lebih lambat dari SPI<br>- Half-duplex<br>- Pull-up resistor diperlukan | - Sensor<br>- EEPROM<br>- Display karakter<br>- Aplikasi dengan banyak perangkat |
| **SPI** | - Sangat cepat<br>- Full-duplex<br>- Sederhana secara elektrikal | - Membutuhkan lebih banyak pin<br>- Tidak ada standar pengalamatan<br>- Jarak terbatas | - Display grafis<br>- Kartu SD<br>- Sensor kecepatan tinggi<br>- Aplikasi yang membutuhkan throughput tinggi |

> 💡 **Pertimbangan Praktis**:
> - **Ketersediaan Library**: Periksa apakah ada library yang mendukung perangkat Anda
> - **Pin Tersedia**: Jika pin GPIO terbatas, I2C mungkin lebih baik
> - **Jarak**: Untuk jarak lebih dari 10cm, pertimbangkan untuk menurunkan kecepatan atau gunakan protokol yang lebih robust
> - **Konsumsi Daya**: I2C dapat mengkonsumsi lebih sedikit daya karena aktivitas bus yang lebih sedikit

---

## 🖥️ Menghubungkan Display dan Antarmuka Pengguna

Display adalah cara yang efektif untuk menyajikan informasi dari sistem embedded tanpa memerlukan komputer. ESP32-S3 dapat dengan mudah terhubung ke berbagai jenis display.

### OLED Display dengan I2C

Display OLED (Organic Light Emitting Diode) sangat populer karena kontras tinggi, sudut pandang lebar, dan konsumsi daya rendah. Display OLED I2C yang paling umum digunakan adalah berdasarkan controller SSD1306 dengan resolusi 128x64 atau 128x32 piksel.

<div align="center">
  <img src="https://i.imgur.com/uokr5ve.jpg" width="400">
  <p><em>Display OLED 128x64 dengan controller SSD1306</em></p>
</div>

**Menghubungkan OLED ke ESP32-S3:**

| OLED Pin | ESP32-S3 Pin |
|----------|--------------|
| VCC      | 3.3V         |
| GND      | GND          |
| SCL      | GPIO22       |
| SDA      | GPIO21       |

**Menggunakan OLED dengan Library Adafruit:**

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define OLED_SDA_PIN 21
#define OLED_SCL_PIN 22
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1  // Reset pin # (atau -1 jika berbagi reset Arduino)
#define OLED_ADDR     0x3C  // Alamat I2C (0x3C atau 0x3D)

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  Serial.begin(115200);
  
  Wire.begin(OLED_SDA_PIN, OLED_SCL_PIN);
  
  // Inisialisasi display OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, OLED_ADDR)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;); // Loop forever
  }
  
  // Bersihkan display
  display.clearDisplay();
  
  // Tampilkan teks "Hello, World!"
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println(F("Hello, World!"));
  display.display();
  
  delay(2000);
  
  // Demonstrasi beberapa kemampuan dasar
  testBasicDrawing();
}

void loop() {
  // Tampilkan counter sederhana
  static int counter = 0;
  
  display.clearDisplay();
  
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println(F("OLED Demo"));
  
  display.setTextSize(2);
  display.setCursor(0, 16);
  display.print(F("Count: "));
  display.println(counter);
  
  display.setTextSize(1);
  display.setCursor(0, 40);
  display.print(F("Runtime: "));
  display.print(millis() / 1000);
  display.println(F(" s"));
  
  display.display();
  
  counter++;
  delay(1000);
}

void testBasicDrawing() {
  display.clearDisplay();
  
  // Gambar bingkai
  display.drawRect(0, 0, display.width(), display.height(), SSD1306_WHITE);
  
  // Gambar titik
  for (int i = 0; i < 5; i++) {
    display.drawPixel(random(10, 118), random(10, 54), SSD1306_WHITE);
  }
  
  // Gambar garis
  display.drawLine(10, 10, 50, 30, SSD1306_WHITE);
  
  // Gambar lingkaran
  display.drawCircle(90, 20, 10, SSD1306_WHITE);
  
  // Gambar kotak terisi
  display.fillRect(70, 40, 30, 15, SSD1306_WHITE);
  
  // Gambar segitiga
  display.drawTriangle(30, 40, 10, 60, 50, 60, SSD1306_WHITE);
  
  display.display();
  delay(3000);
}
```

**Fitur Library Adafruit_SSD1306:**
- **Fungsi Teks**: setCursor(), setTextSize(), setTextColor(), println(), print()
- **Fungsi Gambar Dasar**: drawPixel(), drawLine(), drawRect(), fillRect(), drawCircle(), fillCircle(), drawTriangle(), fillTriangle()
- **Fungsi Bitmap**: drawBitmap() untuk menampilkan ikon dan gambar kustom
- **Fungsi Kontrol Display**: clearDisplay(), display(), invertDisplay()

> 💡 **Tip**: Untuk menghemat memori, Anda dapat menggunakan library alternatif yang lebih ringan seperti `SSD1306Ascii` jika hanya membutuhkan tampilan teks.

### LCD Display dengan I2C

LCD karakter berbasis controller HD44780 dengan adapter I2C adalah pilihan hemat untuk menampilkan informasi berbasis teks. Format yang paling umum adalah 16x2 (16 karakter, 2 baris) atau 20x4 (20 karakter, 4 baris).

<div align="center">
  <img src="https://i.imgur.com/fBF4WrM.jpg" width="400">
  <p><em>LCD 16x2 dengan Adapter I2C</em></p>
</div>

**Menghubungkan LCD I2C ke ESP32-S3:**

| LCD I2C Pin | ESP32-S3 Pin |
|-------------|--------------|
| VCC         | 5V (atau 3.3V, cek spesifikasi) |
| GND         | GND          |
| SCL         | GPIO22       |
| SDA         | GPIO21       |

**Menggunakan LCD I2C dengan Library LiquidCrystal_I2C:**

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define LCD_SDA_PIN 21
#define LCD_SCL_PIN 22
#define LCD_ADDR    0x27  // Alamat I2C, biasanya 0x27 atau 0x3F
#define LCD_COLS    16
#define LCD_ROWS    2

LiquidCrystal_I2C lcd(LCD_ADDR, LCD_COLS, LCD_ROWS);

void setup() {
  Serial.begin(115200);
  
  Wire.begin(LCD_SDA_PIN, LCD_SCL_PIN);
  
  // Inisialisasi LCD
  lcd.init();
  lcd.backlight();
  
  // Tampilkan pesan selamat datang
  lcd.setCursor(0, 0);
  lcd.print("Hello, ESP32-S3!");
  lcd.setCursor(0, 1);
  lcd.print("LCD Test");
  
  delay(2000);
}

void loop() {
  // Tampilkan counter dan waktu berjalan
  static int counter = 0;
  
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("Count: ");
  lcd.print(counter);
  
  lcd.setCursor(0, 1);
  lcd.print("Time: ");
  lcd.print(millis() / 1000);
  lcd.print("s");
  
  counter++;
  delay(1000);
  
  // Demonstrasi scrolling teks untuk pesan panjang
  if (counter % 10 == 0) {
    scrollText("Ini adalah contoh pesan yang cukup panjang untuk demonstrasi scrolling teks pada LCD.", 1);
  }
}

// Fungsi untuk menampilkan teks yang panjang (melebihi lebar LCD) dengan efek scrolling
void scrollText(String text, int row) {
  int textLength = text.length();
  
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Scrolling Text:");
  
  // Scroll teks dari kanan ke kiri
  for (int i = 0; i <= textLength - LCD_COLS; i++) {
    lcd.setCursor(0, row);
    lcd.print(text.substring(i, i + LCD_COLS));
    delay(300);
  }
  
  delay(1000);
}
```

**Fungsi Utama LCD:**
- **Kontrol Dasar**: init(), clear(), home(), noDisplay(), display(), noBlink(), blink(), noCursor(), cursor()
- **Kontrol Backlight**: backlight(), noBacklight()
- **Posisi Kursor**: setCursor(col, row)
- **Output**: print(), write()
- **Karakter Kustom**: createChar() untuk membuat hingga 8 karakter kustom

> 💡 **Tip**: Meskipun LCD I2C lebih mudah untuk dihubungkan (hanya 4 kabel), mereka biasanya lebih lambat daripada LCD paralel. Jika Anda perlu memperbarui display dengan cepat, pertimbangkan untuk menggunakan mode paralel.

### Desain Antarmuka Pengguna Sederhana

Desain antarmuka pengguna (UI) yang baik sangat penting untuk membuat sistem embedded Anda mudah digunakan. Berikut beberapa prinsip dan pola desain UI untuk display terbatas:

**Prinsip Desain UI untuk Display Terbatas:**
1. **Konsistensi**: Gunakan pola yang sama untuk tindakan yang sama
2. **Hirarki**: Tunjukkan informasi penting dengan ukuran lebih besar atau posisi menonjol
3. **Feedback**: Berikan konfirmasi visual untuk tindakan pengguna
4. **Keterbacaan**: Pilih ukuran font yang sesuai dan spasi yang cukup

**Pola Umum untuk Antarmuka OLED/LCD:**

1. **Dashboard**:
   - Tampilkan beberapa nilai sensor pada satu layar
   - Gunakan ikon atau simbol untuk menghemat ruang
   - Perbarui nilai secara berkala

```cpp
void drawDashboard() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println("Smart Home Monitor");
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Suhu
  display.setCursor(0, 12);
  display.print("Temp: ");
  display.print(temperature, 1);
  display.println(" C");
  
  // Kelembaban
  display.setCursor(0, 22);
  display.print("Hum: ");
  display.print(humidity, 0);
  display.println(" %");
  
  // Tekanan
  display.setCursor(0, 32);
  display.print("Press: ");
  display.print(pressure, 0);
  display.println(" hPa");
  
  // Status
  display.setCursor(0, 45);
  display.print("Status: ");
  display.println(alarmActive ? "ALERT!" : "Normal");
  
  // Waktu berjalan
  display.setCursor(0, 55);
  display.print("Runtime: ");
  display.print(millis() / 60000);
  display.println("m");
  
  display.display();
}
```

2. **Menu Navigasi**:
   - Tampilkan daftar opsi yang dapat dipilih
   - Gunakan indikator atau highlight untuk menunjukkan pilihan saat ini
   - Navigasi dengan tombol (atas, bawah, pilih)

```cpp
// Variabel menu
const int NUM_MENU_ITEMS = 4;
const char* menuItems[] = {"Sensors", "Settings", "Data Log", "About"};
int currentMenuItem = 0;

void drawMenu() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setCursor(32, 0);
  display.println("MAIN MENU");
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Menu items
  for (int i = 0; i < NUM_MENU_ITEMS; i++) {
    if (i == currentMenuItem) {
      // Highlight item terpilih
      display.fillRect(0, 12 + i*10, display.width(), 10, SSD1306_WHITE);
      display.setTextColor(SSD1306_BLACK, SSD1306_WHITE);
    } else {
      display.setTextColor(SSD1306_WHITE);
    }
    
    display.setCursor(5, 14 + i*10);
    display.print(menuItems[i]);
    
    // Reset text color
    display.setTextColor(SSD1306_WHITE);
  }
  
  display.display();
}

void handleMenuNavigation() {
  // Dengan asumsi ada tombol untuk UP, DOWN dan SELECT
  if (digitalRead(UP_BUTTON) == LOW) {
    // Navigasi ke atas
    if (currentMenuItem > 0) {
      currentMenuItem--;
    } else {
      currentMenuItem = NUM_MENU_ITEMS - 1;  // Wrap around
    }
    drawMenu();
    delay(200);  // Debounce sederhana
  }
  
  if (digitalRead(DOWN_BUTTON) == LOW) {
    // Navigasi ke bawah
    if (currentMenuItem < NUM_MENU_ITEMS - 1) {
      currentMenuItem++;
    } else {
      currentMenuItem = 0;  // Wrap around
    }
    drawMenu();
    delay(200);  // Debounce sederhana
  }
  
  if (digitalRead(SELECT_BUTTON) == LOW) {
    // Aksi untuk menu terpilih
    handleMenuSelection(currentMenuItem);
    delay(200);  // Debounce sederhana
  }
}
```

3. **Grafik Trend**:
   - Tampilkan perubahan nilai sepanjang waktu
   - Beri label sumbu dan skalakan dengan tepat
   - Pertimbangkan rentang yang bermakna

```cpp
const int GRAPH_WIDTH = 100;  // Lebar grafik dalam piksel
const int GRAPH_HEIGHT = 30;  // Tinggi grafik dalam piksel
const int GRAPH_X = 14;       // Posisi X awal grafik
const int GRAPH_Y = 25;       // Posisi Y awal grafik (bagian bawah grafik)

int tempHistory[GRAPH_WIDTH];  // Array menyimpan data historis
int historyIndex = 0;

void addTemperatureToHistory(float temp) {
  // Konversi ke nilai yang dapat ditampilkan dalam grafik
  int graphValue = map(constrain(temp, 15, 35), 15, 35, 0, GRAPH_HEIGHT);
  
  tempHistory[historyIndex] = graphValue;
  historyIndex = (historyIndex + 1) % GRAPH_WIDTH;
}

void drawTemperatureGraph() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println("Temperature Trend");
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Gambar sumbu
  display.drawLine(GRAPH_X, GRAPH_Y - GRAPH_HEIGHT, GRAPH_X, GRAPH_Y, SSD1306_WHITE);  // Y-axis
  display.drawLine(GRAPH_X, GRAPH_Y, GRAPH_X + GRAPH_WIDTH, GRAPH_Y, SSD1306_WHITE);  // X-axis
  
  // Label sumbu
  display.setCursor(0, GRAPH_Y - GRAPH_HEIGHT);
  display.print("35");
  display.setCursor(0, GRAPH_Y - 1);
  display.print("15");
  
  // Gambar data grafik
  for (int i = 0; i < GRAPH_WIDTH; i++) {
    int x = GRAPH_X + i;
    int dataIndex = (historyIndex + i) % GRAPH_WIDTH;
    
    if (tempHistory[dataIndex] > 0) {  // Data valid
      int y = GRAPH_Y - tempHistory[dataIndex];
      display.drawPixel(x, y, SSD1306_WHITE);
    }
  }
  
  // Tampilkan nilai saat ini
  display.setCursor(GRAPH_X + GRAPH_WIDTH + 5, GRAPH_Y - GRAPH_HEIGHT);
  display.print(temperature, 1);
  display.println("C");
  
  display.display();
}
```

> 💡 **Tip**: Untuk antarmuka pengguna yang lebih kompleks, pertimbangkan library seperti [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI) untuk display TFT, atau [LVGL](https://github.com/lvgl/lvgl) untuk UI yang lebih kaya.

---

## 🌤️ Praktikum 1: Stasiun Cuaca Mini dengan Display

Mari membuat stasiun cuaca mini yang mengukur suhu, kelembaban, dan tekanan udara, lalu menampilkan data pada display OLED.

**Tujuan Praktikum:**
- Mengintegrasikan sensor BME280 (suhu, kelembaban, tekanan) melalui I2C
- Menampilkan data sensor pada display OLED
- Membuat antarmuka dengan beberapa layar/tampilan
- Mengimplementasikan navigasi tampilan dengan tombol

**Komponen yang Dibutuhkan:**
- Board ESP32-S3
- Sensor BME280
- Display OLED 128x64 (controller SSD1306)
- 2 Tombol push button
- Kabel jumper
- Breadboard

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/j03PBZy.png" width="600">
  <p><em>Rangkaian Stasiun Cuaca Mini</em></p>
</div>

**Kode Program:**

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_BME280.h>

// Pin konfigurasi
#define I2C_SDA_PIN     21
#define I2C_SCL_PIN     22
#define BUTTON_NEXT_PIN 14
#define BUTTON_ACTION_PIN 27

// Konfigurasi display
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
#define OLED_ADDR     0x3C

// Konfigurasi BME280
#define BME_ADDR      0x76  // Cek alamat sensor Anda (biasanya 0x76 atau 0x77)

// Konfigurasi aplikasi
#define NUM_SCREENS 4  // Jumlah layar yang tersedia
#define LONG_PRESS_MS 1000  // Waktu untuk long press dalam milidetik

// Inisialisasi objek
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
Adafruit_BME280 bme;

// Variabel sensor
float temperature, humidity, pressure, altitude;

// Variabel untuk navigasi
int currentScreen = 0;
bool configMode = false;

// Variabel untuk history data (grafik)
const int GRAPH_WIDTH = 100;
const int GRAPH_HEIGHT = 30;
const int GRAPH_X = 14;
const int GRAPH_Y = 55;
int tempHistory[GRAPH_WIDTH];
int historyIndex = 0;

// Variabel sistem
unsigned long lastSensorUpdate = 0;
unsigned long lastButtonCheck = 0;
unsigned long buttonPressStart = 0;
bool nextButtonPressed = false;
bool actionButtonPressed = false;

void setup() {
  Serial.begin(115200);
  
  // Inisialisasi I2C
  Wire.begin(I2C_SDA_PIN, I2C_SCL_PIN);
  
  // Inisialisasi pin tombol
  pinMode(BUTTON_NEXT_PIN, INPUT_PULLUP);
  pinMode(BUTTON_ACTION_PIN, INPUT_PULLUP);
  
  // Inisialisasi display OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, OLED_ADDR)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }
  
  // Inisialisasi BME280
  if (!bme.begin(BME_ADDR, &Wire)) {
    Serial.println(F("BME280 not found"));
    // Tampilkan error pada display
    display.clearDisplay();
    display.setTextSize(1);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0, 0);
    display.println(F("BME280 not found!"));
    display.println(F("Check wiring and"));
    display.println(F("I2C address."));
    display.display();
    for (;;);
  }
  
  // Tampilkan splash screen
  showSplashScreen();
  
  // Inisialisasi array history
  for (int i = 0; i < GRAPH_WIDTH; i++) {
    tempHistory[i] = 0;
  }
  
  // Update sensor data pertama kali
  updateSensorData();
}

void loop() {
  // Update sensor secara berkala (setiap 2 detik)
  if (millis() - lastSensorUpdate > 2000) {
    updateSensorData();
    lastSensorUpdate = millis();
  }
  
  // Cek input tombol (debouncing sederhana)
  if (millis() - lastButtonCheck > 50) {
    checkButtons();
    lastButtonCheck = millis();
  }
  
  // Update display sesuai layar saat ini
  if (!configMode) {
    switch (currentScreen) {
      case 0:
        showDashboard();
        break;
      case 1:
        showTemperatureDetail();
        break;
      case 2:
        showHumidityDetail();
        break;
      case 3:
        showPressureDetail();
        break;
    }
  } else {
    showConfigScreen();
  }
}

// Fungsi update data sensor
void updateSensorData() {
  temperature = bme.readTemperature();
  humidity = bme.readHumidity();
  pressure = bme.readPressure() / 100.0F;  // hPa
  
  // Perkiraan ketinggian berdasarkan tekanan udara standar pada permukaan laut (1013.25 hPa)
  altitude = bme.readAltitude(1013.25);
  
  // Tambahkan nilai suhu ke history untuk grafik
  int graphValue = map(constrain(temperature, 15, 35), 15, 35, 0, GRAPH_HEIGHT);
  tempHistory[historyIndex] = graphValue;
  historyIndex = (historyIndex + 1) % GRAPH_WIDTH;
  
  // Print ke Serial untuk debugging
  Serial.print("Temp: ");
  Serial.print(temperature);
  Serial.print("°C, Humidity: ");
  Serial.print(humidity);
  Serial.print("%, Pressure: ");
  Serial.print(pressure);
  Serial.println(" hPa");
}

// Fungsi cek tombol dengan deteksi short dan long press
void checkButtons() {
  // Baca status tombol saat ini
  bool nextState = digitalRead(BUTTON_NEXT_PIN) == LOW;
  bool actionState = digitalRead(BUTTON_ACTION_PIN) == LOW;
  
  // Deteksi perubahan status tombol Next
  if (nextState != nextButtonPressed) {
    nextButtonPressed = nextState;
    
    if (nextState) {
      // Tombol ditekan, catat waktu
      buttonPressStart = millis();
    } else {
      // Tombol dilepas, cek durasi
      if (millis() - buttonPressStart < LONG_PRESS_MS) {
        // Short press - pindah ke layar berikutnya
        if (!configMode) {
          currentScreen = (currentScreen + 1) % NUM_SCREENS;
        } else {
          // Logic untuk navigasi dalam mode config
        }
      }
      // Long press dideteksi di bagian lain
    }
  }
  
  // Deteksi perubahan status tombol Action
  if (actionState != actionButtonPressed) {
    actionButtonPressed = actionState;
    
    if (actionState) {
      // Tombol ditekan, catat waktu
      buttonPressStart = millis();
    } else {
      // Tombol dilepas, cek durasi
      if (millis() - buttonPressStart < LONG_PRESS_MS) {
        // Short press - tindakan sesuai konteks saat ini
        // Di mode normal: aksi untuk layar saat ini
        // Di mode config: konfirmasi setting
      }
    }
  }
  
  // Deteksi long press pada tombol Next
  if (nextButtonPressed && (millis() - buttonPressStart > LONG_PRESS_MS)) {
    // Toggle mode config
    configMode = !configMode;
    // Reset timer untuk menghindari multiple triggers
    buttonPressStart = millis();
  }
}

// Splash screen
void showSplashScreen() {
  display.clearDisplay();
  
  display.setTextColor(SSD1306_WHITE);
  display.setTextSize(2);
  display.setCursor(10, 5);
  display.println(F("WEATHER"));
  display.setCursor(30, 25);
  display.println(F("STATION"));
  
  display.setTextSize(1);
  display.setCursor(25, 50);
  display.println(F("ESP32-S3 BME280"));
  
  display.display();
  delay(2000);
}

// Layar dashboard utama
void showDashboard() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(25, 0);
  display.println(F("WEATHER STATION"));
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Suhu
  display.setCursor(0, 12);
  display.print(F("Temp: "));
  display.print(temperature, 1);
  display.println(F(" C"));
  
  // Kelembaban
  display.setCursor(0, 22);
  display.print(F("Humidity: "));
  display.print(humidity, 0);
  display.println(F(" %"));
  
  // Tekanan
  display.setCursor(0, 32);
  display.print(F("Pressure: "));
  display.print(pressure, 0);
  display.println(F(" hPa"));
  
  // Ketinggian
  display.setCursor(0, 42);
  display.print(F("Altitude: ~"));
  display.print(altitude, 0);
  display.println(F(" m"));
  
  // Page indicator
  display.setCursor(0, 56);
  display.print(F("Page "));
  display.print(currentScreen + 1);
  display.print(F("/"));
  display.print(NUM_SCREENS);
  
  display.display();
}

// Layar detail suhu
void showTemperatureDetail() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(15, 0);
  display.println(F("TEMPERATURE DETAIL"));
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Nilai suhu besar
  display.setTextSize(2);
  display.setCursor(30, 14);
  display.print(temperature, 1);
  display.println(F(" C"));
  
  // Grafik suhu
  display.setTextSize(1);
  
  // Gambar sumbu
  display.drawLine(GRAPH_X, GRAPH_Y - GRAPH_HEIGHT, GRAPH_X, GRAPH_Y, SSD1306_WHITE);  // Y-axis
  display.drawLine(GRAPH_X, GRAPH_Y, GRAPH_X + GRAPH_WIDTH, GRAPH_Y, SSD1306_WHITE);  // X-axis
  
  // Label sumbu
  display.setCursor(0, GRAPH_Y - GRAPH_HEIGHT);
  display.print(F("35C"));
  display.setCursor(0, GRAPH_Y - 1);
  display.print(F("15C"));
  
  // Gambar data
  for (int i = 0; i < GRAPH_WIDTH; i++) {
    int x = GRAPH_X + i;
    int dataIndex = (historyIndex + i) % GRAPH_WIDTH;
    
    if (tempHistory[dataIndex] > 0) {  // Data valid
      int y = GRAPH_Y - tempHistory[dataIndex];
      display.drawPixel(x, y, SSD1306_WHITE);
    }
  }
  
  // Page indicator
  display.setCursor(0, 56);
  display.print(F("Page "));
  display.print(currentScreen + 1);
  display.print(F("/"));
  display.print(NUM_SCREENS);
  
  display.display();
}

// Layar detail kelembaban
void showHumidityDetail() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(20, 0);
  display.println(F("HUMIDITY DETAIL"));
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Nilai kelembaban besar
  display.setTextSize(2);
  display.setCursor(30, 14);
  display.print(humidity, 0);
  display.println(F(" %"));
  
  // Interpretasi kelembaban
  display.setTextSize(1);
  display.setCursor(10, 35);
  display.print(F("Status: "));
  
  if (humidity < 30) {
    display.println(F("Too Dry"));
  } else if (humidity < 40) {
    display.println(F("Dry"));
  } else if (humidity < 60) {
    display.println(F("Comfortable"));
  } else if (humidity < 70) {
    display.println(F("Humid"));
  } else {
    display.println(F("Too Humid"));
  }
  
  // Page indicator
  display.setCursor(0, 56);
  display.print(F("Page "));
  display.print(currentScreen + 1);
  display.print(F("/"));
  display.print(NUM_SCREENS);
  
  display.display();
}

// Layar detail tekanan
void showPressureDetail() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(20, 0);
  display.println(F("PRESSURE DETAIL"));
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Nilai tekanan besar
  display.setTextSize(2);
  display.setCursor(15, 14);
  display.print(pressure, 0);
  display.println(F(" hPa"));
  
  // Interpretasi tekanan (perkiraan cuaca sederhana)
  display.setTextSize(1);
  display.setCursor(10, 35);
  display.print(F("Forecast: "));
  
  if (pressure < 1000) {
    display.println(F("Stormy"));
  } else if (pressure < 1013) {
    display.println(F("Rain"));
  } else if (pressure < 1023) {
    display.println(F("Changing"));
  } else {
    display.println(F("Fair Weather"));
  }
  
  // Ketinggian
  display.setCursor(10, 45);
  display.print(F("Alt: ~"));
  display.print(altitude, 0);
  display.println(F(" m"));
  
  // Page indicator
  display.setCursor(0, 56);
  display.print(F("Page "));
  display.print(currentScreen + 1);
  display.print(F("/"));
  display.print(NUM_SCREENS);
  
  display.display();
}

// Layar konfigurasi (implementasi sederhana)
void showConfigScreen() {
  display.clearDisplay();
  
  // Header
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(25, 0);
  display.println(F("CONFIGURATION"));
  display.drawLine(0, 9, display.width(), 9, SSD1306_WHITE);
  
  // Status mode config
  display.setCursor(10, 20);
  display.println(F("Configuration Mode"));
  display.setCursor(10, 30);
  display.println(F("Long press NEXT"));
  display.setCursor(10, 40);
  display.println(F("to return to normal"));
  
  display.display();
}
```

**Penjelasan:**
1. Program mengintegrasikan sensor BME280 dan display OLED SSD1306 melalui I2C
2. Sistem memiliki empat layar/tampilan yang berbeda:
   - Dashboard utama (menampilkan semua data)
   - Detail suhu (dengan grafik)
   - Detail kelembaban (dengan interpretasi)
   - Detail tekanan (dengan perkiraan cuaca sederhana)
3. Navigasi antar layar menggunakan tombol dengan fitur:
   - Short press tombol Next: pindah ke layar berikutnya
   - Long press tombol Next: masuk/keluar mode konfigurasi
4. Program juga mencatat data historis untuk ditampilkan sebagai grafik trend suhu
5. Pengelolaan waktu non-blocking untuk update sensor dan debouncing tombol

> 💡 **Pengembangan**: Coba tambahkan fitur seperti alarm suhu tinggi/rendah, menampilkan waktu dan tanggal dengan modul RTC, atau menyimpan data sensor ke kartu SD secara berkala.

---

## 📊 Praktikum 2: Sistem Dashboard Multi-Sensor

Mari membuat sistem dashboard berbasis LCD karakter untuk memantau beberapa sensor dan mengontrol perangkat melalui menu interaktif.

**Tujuan Praktikum:**
- Membuat sistem dengan beberapa sensor yang terhubung via I2C
- Menampilkan data sensor pada LCD karakter
- Mengimplementasikan sistem menu navigasi
- Mengontrol perangkat melalui menu

**Komponen yang Dibutuhkan:**
- Board ESP32-S3
- LCD 16x2 atau 20x4 dengan adapter I2C
- Sensor BME280
- Potensiometer analog
- 3 Tombol push button (UP, DOWN, SELECT)
- 1 LED (atau relay) untuk kontrol output
- Kabel jumper
- Breadboard

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/JLjlJ0k.png" width="600">
  <p><em>Rangkaian Sistem Dashboard Multi-Sensor</em></p>
</div>

**Kode Program:**

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Adafruit_BME280.h>

// Pin konfigurasi
#define I2C_SDA_PIN 21
#define I2C_SCL_PIN 22
#define POT_PIN 34          // Potensiometer pada pin analog
#define BTN_UP_PIN 14
#define BTN_DOWN_PIN 27
#define BTN_SELECT_PIN 26
#define LED_PIN 13

// Konfigurasi LCD
#define LCD_ADDR 0x27       // Alamat I2C LCD (biasanya 0x27 atau 0x3F)
#define LCD_COLS 16
#define LCD_ROWS 2

// Konfigurasi BME280
#define BME_ADDR 0x76       // Alamat I2C BME280 (biasanya 0x76 atau 0x77)

// Inisialisasi objek
LiquidCrystal_I2C lcd(LCD_ADDR, LCD_COLS, LCD_ROWS);
Adafruit_BME280 bme;

// Variabel sensor
float temperature, humidity, pressure;
int potValue, potPercent;

// Variabel untuk menu
enum MenuState {
  MENU_MAIN,
  MENU_SENSORS,
  MENU_SETTINGS,
  MENU_CONTROL,
  MENU_ABOUT
};

MenuState currentMenu = MENU_MAIN;
MenuState previousMenu = MENU_MAIN;
int menuItemSelected = 0;

// Definisi menu
const int MAIN_MENU_ITEMS = 4;
const char* mainMenuItems[MAIN_MENU_ITEMS] = {
  "Sensors",
  "Settings",
  "Control",
  "About"
};

const int SENSORS_MENU_ITEMS = 3;
const char* sensorsMenuItems[SENSORS_MENU_ITEMS] = {
  "Temperature",
  "Humidity",
  "Pressure"
};

const int SETTINGS_MENU_ITEMS = 3;
const char* settingsMenuItems[SETTINGS_MENU_ITEMS] = {
  "Brightness",
  "Contrast",
  "Back"
};

const int CONTROL_MENU_ITEMS = 3;
const char* controlMenuItems[CONTROL_MENU_ITEMS] = {
  "LED On",
  "LED Off",
  "Back"
};

// Variabel sistem
unsigned long lastSensorUpdate = 0;
unsigned long lastButtonCheck = 0;
unsigned long lastMenuUpdate = 0;
bool btnUpPressed = false;
bool btnDownPressed = false;
bool btnSelectPressed = false;
bool ledState = false;
int lcdBacklight = 1;  // 0=off, 1=on

// Sub-menu untuk menampilkan detail sensor
enum SensorDetailView {
  VIEW_NONE,
  VIEW_TEMPERATURE,
  VIEW_HUMIDITY,
  VIEW_PRESSURE
};

SensorDetailView currentSensorView = VIEW_NONE;

void setup() {
  Serial.begin(115200);
  
  // Inisialisasi I2C
  Wire.begin(I2C_SDA_PIN, I2C_SCL_PIN);
  
  // Inisialisasi pin
  pinMode(POT_PIN, INPUT);
  pinMode(BTN_UP_PIN, INPUT_PULLUP);
  pinMode(BTN_DOWN_PIN, INPUT_PULLUP);
  pinMode(BTN_SELECT_PIN, INPUT_PULLUP);
  pinMode(LED_PIN, OUTPUT);
  
  digitalWrite(LED_PIN, LOW);
  
  // Inisialisasi LCD
  lcd.init();
  lcd.backlight();
  
  // Inisialisasi BME280
  if (!bme.begin(BME_ADDR)) {
    Serial.println("BME280 not found!");
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("BME280 not found!");
    lcd.setCursor(0, 1);
    lcd.print("Check wiring!");
    
    // Wait for a while then try to continue anyway
    delay(3000);
  }
  
  // Tampilkan splash screen
  showSplashScreen();
  
  // Update sensor data pertama kali
  updateSensorData();
}

void loop() {
  // Update sensor secara berkala (setiap 2 detik)
  if (millis() - lastSensorUpdate > 2000) {
    updateSensorData();
    lastSensorUpdate = millis();
  }
  
  // Cek input tombol (debouncing sederhana)
  if (millis() - lastButtonCheck > 100) {
    checkButtons();
    lastButtonCheck = millis();
  }
  
  // Update menu secara berkala (refreshes)
  if (millis() - lastMenuUpdate > 200) {
    updateDisplay();
    lastMenuUpdate = millis();
  }
}

// Fungsi update data sensor
void updateSensorData() {
  // Baca dari BME280
  temperature = bme.readTemperature();
  humidity = bme.readHumidity();
  pressure = bme.readPressure() / 100.0F;  // hPa
  
  // Baca dari potensiometer
  potValue = analogRead(POT_PIN);
  potPercent = map(potValue, 0, 4095, 0, 100);
  
  // Print ke Serial untuk debugging
  Serial.println("Sensor Data:");
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" °C");
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");
  Serial.print("Pressure: ");
  Serial.print(pressure);
  Serial.println(" hPa");
  Serial.print("Potentiometer: ");
  Serial.print(potPercent);
  Serial.println(" %");
  Serial.println("--------------------");
}

// Fungsi cek tombol
void checkButtons() {
  // Baca status tombol saat ini
  bool upState = digitalRead(BTN_UP_PIN) == LOW;
  bool downState = digitalRead(BTN_DOWN_PIN) == LOW;
  bool selectState = digitalRead(BTN_SELECT_PIN) == LOW;
  
  // Deteksi perubahan status UP
  if (upState && !btnUpPressed) {
    btnUpPressed = true;
    handleUpButton();
  } else if (!upState && btnUpPressed) {
    btnUpPressed = false;
  }
  
  // Deteksi perubahan status DOWN
  if (downState && !btnDownPressed) {
    btnDownPressed = true;
    handleDownButton();
  } else if (!downState && btnDownPressed) {
    btnDownPressed = false;
  }
  
  // Deteksi perubahan status SELECT
  if (selectState && !btnSelectPressed) {
    btnSelectPressed = true;
    handleSelectButton();
  } else if (!selectState && btnSelectPressed) {
    btnSelectPressed = false;
  }
}

// Handler untuk tombol UP
void handleUpButton() {
  if (currentSensorView != VIEW_NONE) {
    // Jika dalam view detail sensor, kembali ke menu sensors
    currentSensorView = VIEW_NONE;
    currentMenu = MENU_SENSORS;
    return;
  }
  
  // Navigasi menu
  switch (currentMenu) {
    case MENU_MAIN:
      if (menuItemSelected > 0) {
        menuItemSelected--;
      } else {
        menuItemSelected = MAIN_MENU_ITEMS - 1;  // Wrap around
      }
      break;
      
    case MENU_SENSORS:
      if (menuItemSelected > 0) {
        menuItemSelected--;
      } else {
        menuItemSelected = SENSORS_MENU_ITEMS - 1;
      }
      break;
      
    case MENU_SETTINGS:
      if (menuItemSelected > 0) {
        menuItemSelected--;
      } else {
        menuItemSelected = SETTINGS_MENU_ITEMS - 1;
      }
      break;
      
    case MENU_CONTROL:
      if (menuItemSelected > 0) {
        menuItemSelected--;
      } else {
        menuItemSelected = CONTROL_MENU_ITEMS - 1;
      }
      break;
  }
}

// Handler untuk tombol DOWN
void handleDownButton() {
  if (currentSensorView != VIEW_NONE) {
    // Jika dalam view detail sensor, kembali ke menu sensors
    currentSensorView = VIEW_NONE;
    currentMenu = MENU_SENSORS;
    return;
  }
  
  // Navigasi menu
  switch (currentMenu) {
    case MENU_MAIN:
      menuItemSelected = (menuItemSelected + 1) % MAIN_MENU_ITEMS;
      break;
      
    case MENU_SENSORS:
      menuItemSelected = (menuItemSelected + 1) % SENSORS_MENU_ITEMS;
      break;
      
    case MENU_SETTINGS:
      menuItemSelected = (menuItemSelected + 1) % SETTINGS_MENU_ITEMS;
      break;
      
    case MENU_CONTROL:
      menuItemSelected = (menuItemSelected + 1) % CONTROL_MENU_ITEMS;
      break;
  }
}

// Handler untuk tombol SELECT
void handleSelectButton() {
  // Jika dalam view detail sensor, kembali ke menu sensors
  if (currentSensorView != VIEW_NONE) {
    currentSensorView = VIEW_NONE;
    currentMenu = MENU_SENSORS;
    return;
  }
  
  // Aksi sesuai menu saat ini
  switch (currentMenu) {
    case MENU_MAIN:
      previousMenu = MENU_MAIN;
      menuItemSelected = 0;
      
      switch (menuItemSelected) {
        case 0:  // Sensors
          currentMenu = MENU_SENSORS;
          break;
        case 1:  // Settings
          currentMenu = MENU_SETTINGS;
          break;
        case 2:  // Control
          currentMenu = MENU_CONTROL;
          break;
        case 3:  // About
          showAboutScreen();
          delay(3000);  // Tampilkan sebentar
          // Kembali ke main menu
          break;
      }
      break;
      
    case MENU_SENSORS:
      switch (menuItemSelected) {
        case 0:  // Temperature
          currentSensorView = VIEW_TEMPERATURE;
          break;
        case 1:  // Humidity
          currentSensorView = VIEW_HUMIDITY;
          break;
        case 2:  // Pressure
          currentSensorView = VIEW_PRESSURE;
          break;
      }
      break;
      
    case MENU_SETTINGS:
      switch (menuItemSelected) {
        case 0:  // Brightness
          // Toggle backlight
          lcdBacklight = !lcdBacklight;
          if (lcdBacklight) {
            lcd.backlight();
          } else {
            lcd.noBacklight();
          }
          break;
        case 1:  // Contrast
          // Not implemented - would need external hardware
          break;
        case 2:  // Back
          currentMenu = MENU_MAIN;
          menuItemSelected = 0;
          break;
      }
      break;
      
    case MENU_CONTROL:
      switch (menuItemSelected) {
        case 0:  // LED On
          ledState = true;
          digitalWrite(LED_PIN, HIGH);
          break;
        case 1:  // LED Off
          ledState = false;
          digitalWrite(LED_PIN, LOW);
          break;
        case 2:  // Back
          currentMenu = MENU_MAIN;
          menuItemSelected = 0;
          break;
      }
      break;
  }
}

// Fungsi update display
void updateDisplay() {
  // Jika dalam view detail sensor
  if (currentSensorView != VIEW_NONE) {
    displaySensorDetail();
    return;
  }
  
  // Update display sesuai menu saat ini
  switch (currentMenu) {
    case MENU_MAIN:
      displayMainMenu();
      break;
      
    case MENU_SENSORS:
      displaySensorsMenu();
      break;
      
    case MENU_SETTINGS:
      displaySettingsMenu();
      break;
      
    case MENU_CONTROL:
      displayControlMenu();
      break;
  }
}

// Tampilkan menu utama
void displayMainMenu() {
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("MAIN MENU");
  
  lcd.setCursor(0, 1);
  lcd.print(">");
  lcd.print(mainMenuItems[menuItemSelected]);
}

// Tampilkan menu sensors
void displaySensorsMenu() {
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("SENSORS");
  
  lcd.setCursor(0, 1);
  lcd.print(">");
  lcd.print(sensorsMenuItems[menuItemSelected]);
}

// Tampilkan menu settings
void displaySettingsMenu() {
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("SETTINGS");
  
  lcd.setCursor(0, 1);
  lcd.print(">");
  lcd.print(settingsMenuItems[menuItemSelected]);
  
  // Tampilkan status backlight
  if (menuItemSelected == 0) {  // Brightness option
    lcd.setCursor(LCD_COLS - 3, 1);
    lcd.print(lcdBacklight ? "ON" : "OFF");
  }
}

// Tampilkan menu control
void displayControlMenu() {
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("CONTROL");
  
  lcd.setCursor(0, 1);
  lcd.print(">");
  lcd.print(controlMenuItems[menuItemSelected]);
  
  // Tampilkan status LED
  if (menuItemSelected < 2) {  // LED options
    lcd.setCursor(LCD_COLS - 3, 1);
    lcd.print(ledState ? "ON" : "OFF");
  }
}

// Tampilkan detail sensor
void displaySensorDetail() {
  lcd.clear();
  
  switch (currentSensorView) {
    case VIEW_TEMPERATURE:
      lcd.setCursor(0, 0);
      lcd.print("Temperature");
      lcd.setCursor(0, 1);
      lcd.print(temperature, 1);
      lcd.print(" C");
      break;
      
    case VIEW_HUMIDITY:
      lcd.setCursor(0, 0);
      lcd.print("Humidity");
      lcd.setCursor(0, 1);
      lcd.print(humidity, 1);
      lcd.print(" %");
      break;
      
    case VIEW_PRESSURE:
      lcd.setCursor(0, 0);
      lcd.print("Pressure");
      lcd.setCursor(0, 1);
      lcd.print(pressure, 0);
      lcd.print(" hPa");
      break;
      
    default:
      break;
  }
}

// Tampilkan splash screen
void showSplashScreen() {
  lcd.clear();
  
  // Baris pertama
  lcd.setCursor(0, 0);
  lcd.print("ESP32-S3 Control");
  
  // Baris kedua
  lcd.setCursor(0, 1);
  lcd.print("System v1.0");
  
  delay(2000);
}

// Tampilkan about screen
void showAboutScreen() {
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("ESP32-S3 Project");
  
  lcd.setCursor(0, 1);
  lcd.print("(c) 2023");
}
```

**Penjelasan:**
1. Program mengintegrasikan sensor BME280, potensiometer analog, dan LCD karakter via I2C
2. Implementasi sistem menu navigasi dengan 4 menu utama:
   - Sensors: untuk melihat pembacaan sensor (suhu, kelembaban, tekanan)
   - Settings: untuk mengatur LCD (backlight on/off)
   - Control: untuk mengontrol LED (on/off)
   - About: informasi tentang sistem
3. Tiga tombol digunakan untuk navigasi dan kontrol:
   - UP: navigasi ke item menu sebelumnya
   - DOWN: navigasi ke item menu berikutnya
   - SELECT: pilih item menu saat ini atau kembali dari view detail
4. Program menggunakan pendekatan state machine untuk manajemen menu dan tampilan
5. Update sensor dan tampilan dilakukan secara non-blocking

> 💡 **Pengembangan**: Coba tambahkan fitur seperti penyimpanan pengaturan di EEPROM, kontrol multiple output (misalnya beberapa LED atau relay), atau tampilkan grafik batang sederhana menggunakan karakter kustom LCD.

---

## 🏋️ Latihan Mandiri

Sekarang saatnya Anda mencoba sendiri untuk memastikan pemahaman. Cobalah tantangan-tantangan berikut:

### Latihan 1: Komunikasi Multi-Sensor dengan I2C

Buat sistem yang menghubungkan minimal dua sensor berbeda melalui I2C dan menampilkan datanya pada Serial Monitor.

**Petunjuk:**
- Gunakan sensor BME280 dan BH1750 (sensor cahaya) atau MPU6050 (sensor gerakan)
- Cek alamat I2C masing-masing sensor dengan I2C scanner
- Pastikan pembacaan sensor berfungsi dan tampilkan data secara terorganisir
- Bonus: Implementasikan alarm saat nilai sensor melewati ambang batas tertentu

### Latihan 2: Jam Digital dengan RTC

Buat jam digital sederhana menggunakan modul Real-Time Clock (RTC DS3231 atau DS1307) dan tampilkan pada LCD atau OLED.

**Petunjuk:**
- Hubungkan modul RTC via I2C
- Tampilkan waktu (jam:menit:detik) dan tanggal
- Implementasikan tombol untuk mengatur waktu
- Bonus: Tambahkan fitur alarm pada waktu tertentu

### Latihan 3: Kontrol Motor Servo dengan SPI

Buat sistem yang mengontrol beberapa motor servo menggunakan ekspander I/O SPI seperti MCP23S17.

**Petunjuk:**
- Hubungkan ekspander I/O MCP23S17 melalui SPI
- Konfigurasikan pin ekspander sebagai output
- Gunakan PWM dari pin ekspander untuk mengontrol 2-3 servo
- Implementasikan pola gerakan yang berbeda
- Bonus: Gunakan potensiometer untuk mengontrol posisi servo

### Latihan 4: Menu Terstruktur dengan Navigasi

Kembangkan sistem menu terstruktur multi-level dengan navigasi yang intuitif pada display OLED atau LCD.

**Petunjuk:**
- Buat minimal 3 level menu (menu utama, sub-menu, halaman detail)
- Implementasikan navigasi dengan 3 tombol (up, down, select)
- Terapkan transisi visual antara menu
- Buat pengaturan yang dapat disimpan di memori non-volatile
- Bonus: Implementasikan animasi atau ikon kustom

---

## 📚 Sumber Referensi

1. Espressif Systems. (2023). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

2. Espressif Systems. (2023). *ESP-IDF Programming Guide: I2C Driver*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/i2c.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/i2c.html)

3. Espressif Systems. (2023). *ESP-IDF Programming Guide: SPI Driver*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/spi_master.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/spi_master.html)

4. Adafruit. (2023). *Adafruit SSD1306 Library*. [https://github.com/adafruit/Adafruit_SSD1306](https://github.com/adafruit/Adafruit_SSD1306)

5. Adafruit. (2023). *Adafruit BME280 Library*. [https://github.com/adafruit/Adafruit_BME280_Library](https://github.com/adafruit/Adafruit_BME280_Library)

6. Malpartida, F. (2022). *LCD Library for Arduino*. [https://github.com/fmalpartida/New-LiquidCrystal](https://github.com/fmalpartida/New-LiquidCrystal)

7. Kolban, N. (2023). *ESP32 Technical Tutorials: I2C Communication*. Kolban's Book on ESP32.

8. SPI Protocol: NXP Semiconductors. (2022). *UM10204: I2C-bus specification and user manual*. [https://www.nxp.com/docs/en/user-guide/UM10204.pdf](https://www.nxp.com/docs/en/user-guide/UM10204.pdf)

9. SPI Protocol: Motorola, Inc. (1994). *SPI Block Guide*. 

10. RandomNerdTutorials. (2023). *Complete Guide for OLED Display with ESP32*. [https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/](https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/)

11. Lewis, M. (2022). *User Interface Design for Embedded Systems*. Practical Electronics, 35(3), 112-125.

12. Wilson, S. (2022). *Serial Communication Protocols in IoT Devices*. Embedded Systems Journal, 28(4), 67-82.

---

**Modul berikutnya**: Pemrograman Lanjutan dengan ESP32-S3
