# 📘 Modul 2: Dasar-Dasar Input/Output dengan ESP32-S3

<div align="center">
  <img src="https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/_images/esp32-s3-devkitc-1-v1.1_top_labled.png" width="400">
  
  <p><em>ESP32-S3 - Interaksi dengan Dunia Luar Melalui Input dan Output</em></p>
  
  <p>
    <img src="https://img.shields.io/badge/Tingkat-Pemula-brightgreen" alt="Tingkat: Pemula">
    <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
    <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
  </p>
</div>

## 📋 Daftar Isi

- [Tujuan Pembelajaran](#-tujuan-pembelajaran)
- [Kendali GPIO Digital](#-kendali-gpio-digital)
  - [Memahami GPIO](#memahami-gpio)
  - [Mengonfigurasi Pin sebagai Input dan Output](#mengonfigurasi-pin-sebagai-input-dan-output)
  - [Pull-up dan Pull-down Resistor](#pull-up-dan-pull-down-resistor)
  - [Membaca Tombol dan Mengontrol LED](#membaca-tombol-dan-mengontrol-led)
  - [Debouncing: Menstabilkan Input](#debouncing-menstabilkan-input)
- [Pemrosesan Sinyal Analog](#-pemrosesan-sinyal-analog)
  - [Memahami Sinyal Analog vs Digital](#memahami-sinyal-analog-vs-digital)
  - [Membaca Sensor Analog dengan ADC](#membaca-sensor-analog-dengan-adc)
  - [Menghasilkan Sinyal Analog dengan PWM](#menghasilkan-sinyal-analog-dengan-pwm)
  - [Kalibrasi Sensor](#kalibrasi-sensor)
- [Praktikum 1: Sistem Kontrol Lampu Interaktif](#-praktikum-1-sistem-kontrol-lampu-interaktif)
- [Praktikum 2: Monitoring Lingkungan](#-praktikum-2-monitoring-lingkungan)
- [Latihan Mandiri](#%EF%B8%8F-latihan-mandiri)
- [Sumber Referensi](#-sumber-referensi)

---

## 🎯 Tujuan Pembelajaran

Setelah mempelajari modul ini, Anda diharapkan dapat:

1. Mengonfigurasi dan menggunakan pin GPIO sebagai input dan output digital
2. Memahami dan mengimplementasikan resistor pull-up dan pull-down
3. Membaca input dari tombol dan mengontrol LED dengan teknik debouncing
4. Membaca sensor analog menggunakan ADC (Analog-to-Digital Converter)
5. Menghasilkan sinyal analog menggunakan PWM (Pulse Width Modulation)
6. Mengimplementasikan kalibrasi sensor sederhana
7. Membangun sistem kontrol sederhana yang mengintegrasikan input dan output

---

## 🔌 Kendali GPIO Digital

### Memahami GPIO

GPIO (General Purpose Input/Output) adalah pin pada microcontroller yang dapat dikonfigurasi sebagai input atau output untuk berinteraksi dengan dunia luar. Pada ESP32-S3, terdapat 45 pin GPIO yang dapat diprogram.

<div align="center">
  <img src="https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/_images/ESP32-S3_DevKitC-1_pinlayout.png" width="600">
  <p><em>Pinout ESP32-S3 DevKitC</em></p>
</div>

Karakteristik GPIO pada ESP32-S3:

- **Jumlah Pin**: 45 pin GPIO (GPIO0 - GPIO48, dengan beberapa nomor yang tidak digunakan)
- **Tegangan Kerja**: 3.3V (tidak toleran terhadap 5V)
- **Arus Maksimum**: 40 mA per pin
- **Mode**: Input, Output, atau Fungsi Khusus (UART, SPI, I2C, dll.)
- **Fitur Tambahan**: Pull-up/pull-down internal, interrupt, kapasitif sentuh

> 💡 **Penting**: Tidak semua pin GPIO tersedia untuk digunakan secara bebas. Beberapa pin memiliki fungsi khusus atau terhubung ke komponen pada board. Selalu periksa dokumentasi board Anda.

### Mengonfigurasi Pin sebagai Input dan Output

Dalam Arduino IDE, kita menggunakan fungsi `pinMode()` untuk mengonfigurasi pin GPIO:

```cpp
// Mengonfigurasi pin sebagai output
pinMode(pin_number, OUTPUT);

// Mengonfigurasi pin sebagai input
pinMode(pin_number, INPUT);

// Mengonfigurasi pin sebagai input dengan pull-up internal
pinMode(pin_number, INPUT_PULLUP);

// Khusus ESP32: Mengonfigurasi pin sebagai input dengan pull-down internal
pinMode(pin_number, INPUT_PULLDOWN);
```

**Contoh Konfigurasi:**

```cpp
void setup() {
  // LED terhubung ke GPIO2, dikonfigurasi sebagai output
  pinMode(2, OUTPUT);
  
  // Tombol terhubung ke GPIO5, dikonfigurasi sebagai input dengan pull-up
  pinMode(5, INPUT_PULLUP);
  
  // Sensor terhubung ke GPIO4, dikonfigurasi sebagai input biasa
  pinMode(4, INPUT);
}
```

### Pull-up dan Pull-down Resistor

Pull-up dan pull-down resistor digunakan untuk memastikan pin input memiliki nilai yang stabil saat tidak ada sinyal eksternal yang masuk.

<div align="center">
  <img src="https://i.imgur.com/WmQhN9R.png" width="500">
  <p><em>Ilustrasi Pull-up dan Pull-down Resistor</em></p>
</div>

**Pull-up Resistor**:
- Menghubungkan pin ke VCC (3.3V) melalui resistor (biasanya 10kΩ)
- Pin akan bernilai HIGH saat tidak dihubungkan (default)
- Pin menjadi LOW saat dihubungkan ke GND (misalnya saat tombol ditekan)

**Pull-down Resistor**:
- Menghubungkan pin ke GND melalui resistor
- Pin akan bernilai LOW saat tidak dihubungkan (default)
- Pin menjadi HIGH saat dihubungkan ke VCC

ESP32-S3 memiliki pull-up dan pull-down resistor internal yang dapat diaktifkan dalam software:

```cpp
// Mengaktifkan resistor pull-up internal
pinMode(pin_number, INPUT_PULLUP);

// Mengaktifkan resistor pull-down internal
pinMode(pin_number, INPUT_PULLDOWN);

// Atau menggunakan fungsi khusus ESP32
gpio_pullup_en(pin_number);   // Mengaktifkan pull-up
gpio_pulldown_en(pin_number); // Mengaktifkan pull-down
```

> 💡 **Tip**: Gunakan resistor pull-up internal untuk membaca tombol. Anda tidak perlu menambahkan resistor fisik, yang menyederhanakan rangkaian Anda.

### Membaca Tombol dan Mengontrol LED

**Membaca Status Pin Input**:

```cpp
// Membaca status pin input
int buttonState = digitalRead(buttonPin);

// buttonState akan bernilai HIGH atau LOW
if (buttonState == HIGH) {
  // Tombol tidak ditekan (jika menggunakan pull-up)
} else {
  // Tombol ditekan (jika menggunakan pull-up)
}
```

**Mengontrol Pin Output**:

```cpp
// Menyalakan LED (jika anoda terhubung ke pin GPIO)
digitalWrite(ledPin, HIGH);

// Mematikan LED (jika anoda terhubung ke pin GPIO)
digitalWrite(ledPin, LOW);
```

**Contoh Menghubungkan Tombol dan LED**:

```cpp
const int buttonPin = 5;  // Tombol terhubung ke pin GPIO5
const int ledPin = 2;     // LED terhubung ke pin GPIO2
int buttonState = 0;      // Variabel untuk menyimpan status tombol

void setup() {
  pinMode(ledPin, OUTPUT);       // Inisialisasi pin LED sebagai output
  pinMode(buttonPin, INPUT_PULLUP); // Inisialisasi pin tombol sebagai input dengan pull-up
}

void loop() {
  // Membaca status tombol
  buttonState = digitalRead(buttonPin);

  // Jika tombol ditekan (LOW dengan pull-up), nyalakan LED
  if (buttonState == LOW) {
    digitalWrite(ledPin, HIGH);  // Nyalakan LED
  } else {
    digitalWrite(ledPin, LOW);   // Matikan LED
  }
}
```

### Debouncing: Menstabilkan Input

Tombol mekanis sering mengalami "bouncing" - osilasi cepat antara HIGH dan LOW saat ditekan atau dilepas. Ini dapat menyebabkan beberapa pembacaan yang tidak diinginkan.

<div align="center">
  <img src="https://i.imgur.com/YieQBWw.png" width="500">
  <p><em>Fenomena Bouncing pada Tombol Mekanis</em></p>
</div>

**Metode Debouncing dengan Delay**:

```cpp
const int buttonPin = 5;
const int ledPin = 2;
int ledState = LOW;
int lastButtonState = HIGH;
unsigned long lastDebounceTime = 0;
unsigned long debounceDelay = 50;  // Delay debouncing dalam milidetik

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, ledState);
}

void loop() {
  // Baca status tombol saat ini
  int reading = digitalRead(buttonPin);

  // Jika status berubah, reset timer debouncing
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }

  // Jika tombol stabil selama waktu debounce
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika status tombol berubah
    if (reading != lastButtonState) {
      // Jika tombol ditekan (LOW dengan pull-up)
      if (reading == LOW) {
        // Toggle status LED
        ledState = !ledState;
        digitalWrite(ledPin, ledState);
      }
    }
  }

  // Simpan pembacaan saat ini untuk dibandingkan pada iterasi berikutnya
  lastButtonState = reading;
}
```

**Debouncing dengan Library**:

ESP32 Arduino memiliki library yang dapat membantu untuk debouncing, seperti `Bounce2`:

```cpp
#include <Bounce2.h>

const int buttonPin = 5;
const int ledPin = 2;
int ledState = LOW;

// Instantiasi objek Bounce
Bounce debouncer = Bounce();

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin, OUTPUT);
  
  // Atur pin dan interval debounce
  debouncer.attach(buttonPin);
  debouncer.interval(50); // Interval debounce dalam milidetik
}

void loop() {
  // Update status debouncer
  debouncer.update();
  
  // Jika tombol ditekan (state berubah dari HIGH ke LOW)
  if (debouncer.fell()) {
    // Toggle status LED
    ledState = !ledState;
    digitalWrite(ledPin, ledState);
  }
}
```

> 💡 **Tip**: Debouncing adalah keterampilan penting untuk aplikasi dunia nyata. Tanpa debouncing, tombol Anda mungkin merespons beberapa kali untuk satu penekanan.

---

## 📊 Pemrosesan Sinyal Analog

### Memahami Sinyal Analog vs Digital

**Sinyal Digital**: Hanya memiliki dua nilai: HIGH atau LOW (1 atau 0).
**Sinyal Analog**: Memiliki nilai yang bervariasi secara kontinu dalam rentang tertentu.

<div align="center">
  <img src="https://i.imgur.com/oGXt2ZA.png" width="500">
  <p><em>Perbandingan Sinyal Digital dan Analog</em></p>
</div>

Dunia nyata bersifat analog:
- Suhu berubah secara bertahap, bukan hanya "panas" atau "dingin"
- Intensitas cahaya memiliki berbagai tingkat, bukan hanya "terang" atau "gelap"
- Tekanan udara bervariasi secara kontinu

Untuk berinteraksi dengan dunia analog, ESP32-S3 menggunakan:
- **ADC (Analog-to-Digital Converter)**: Mengubah sinyal analog menjadi nilai digital
- **DAC (Digital-to-Analog Converter)**: Mengubah nilai digital menjadi sinyal analog
- **PWM (Pulse Width Modulation)**: Mensimulasikan output analog menggunakan sinyal digital

### Membaca Sensor Analog dengan ADC

ESP32-S3 memiliki ADC 12-bit, artinya dapat mengukur tegangan analog dan mengubahnya menjadi nilai digital 0-4095 (2^12).

**Karakteristik ADC ESP32-S3**:
- **Resolusi**: 12-bit (nilai 0-4095)
- **Channel**: 20 channel ADC (GPIO1 - GPIO20)
- **Tegangan Input**: 0V - 3.3V
- **Akurasi**: Tidak linier secara sempurna, memerlukan kalibrasi untuk pengukuran presisi

**Membaca Nilai Analog:**

```cpp
// Membaca nilai analog dari pin GPIO6
int sensorValue = analogRead(6);

// sensorValue akan berada dalam rentang 0-4095
// 0 = 0V, 4095 = 3.3V
```

**Mengonversi Nilai ADC ke Tegangan:**

```cpp
// Membaca nilai analog
int sensorValue = analogRead(6);

// Mengonversi ke tegangan (dalam volt)
float voltage = sensorValue * (3.3 / 4095.0);
```

**Contoh Program Pembacaan Sensor Analog:**

```cpp
const int sensorPin = 6;  // Sensor terhubung ke GPIO6
float voltage;
int sensorValue;

void setup() {
  Serial.begin(115200);
  delay(1000);  // Berikan waktu Serial Monitor untuk siap
  
  // Konfigurasi ADC (opsional, menggunakan nilai default jika tidak disetel)
  analogSetWidth(12);  // Setel resolusi 12-bit
  analogSetAttenuation(ADC_11db);  // Setel atenuasi, mempengaruhi rentang tegangan
}

void loop() {
  // Baca nilai sensor
  sensorValue = analogRead(sensorPin);
  
  // Konversi ke tegangan
  voltage = sensorValue * (3.3 / 4095.0);
  
  // Tampilkan ke Serial Monitor
  Serial.print("Nilai ADC: ");
  Serial.print(sensorValue);
  Serial.print(", Tegangan: ");
  Serial.print(voltage, 2);  // 2 angka di belakang koma
  Serial.println(" V");
  
  delay(500);  // Pembacaan setiap 0.5 detik
}
```

**Meningkatkan Akurasi Pembacaan:**

Untuk pengukuran yang lebih akurat:
1. Gunakan rata-rata dari beberapa pembacaan untuk mengurangi noise
2. Gunakan kalibrasi untuk mengkompensasi ketidaklinieran ADC

```cpp
const int sensorPin = 6;
const int numReadings = 10;  // Jumlah pembacaan untuk dirata-rata

float getAverageReading() {
  long total = 0;
  
  // Ambil beberapa sampel dan jumlahkan
  for (int i = 0; i < numReadings; i++) {
    total += analogRead(sensorPin);
    delay(10);  // Jeda kecil antara pembacaan
  }
  
  // Kembalikan nilai rata-rata
  return total / (float)numReadings;
}

void loop() {
  float avgValue = getAverageReading();
  float voltage = avgValue * (3.3 / 4095.0);
  
  Serial.print("Nilai Rata-rata: ");
  Serial.print(avgValue);
  Serial.print(", Tegangan: ");
  Serial.print(voltage, 3);
  Serial.println(" V");
  
  delay(500);
}
```

> ⚠️ **Catatan**: Performa ADC ESP32-S3 bervariasi antar chip dan dapat dipengaruhi oleh suhu. Untuk pengukuran presisi tinggi, pertimbangkan untuk menggunakan ADC eksternal.

### Menghasilkan Sinyal Analog dengan PWM

PWM (Pulse Width Modulation) adalah teknik untuk mensimulasikan output analog menggunakan sinyal digital dengan mengubah lebar pulsa.

<div align="center">
  <img src="https://i.imgur.com/dTQWApx.png" width="500">
  <p><em>Pulse Width Modulation dengan Nilai Duty Cycle Berbeda</em></p>
</div>

**Karakteristik PWM ESP32-S3**:
- **Resolusi**: Dapat dikonfigurasi hingga 16-bit (0-65535)
- **Frekuensi**: Dapat diatur dari beberapa Hz hingga sekitar 80 MHz
- **Channel**: 16 channel PWM independen
- **Pins**: Hampir semua GPIO dapat digunakan untuk PWM

**Menggunakan analogWrite() untuk PWM Sederhana:**

```cpp
// Contoh PWM sederhana menggunakan analogWrite()
const int ledPin = 2;  // LED terhubung ke GPIO2

void setup() {
  // Tidak perlu pinMode() untuk analogWrite()
}

void loop() {
  // Nilai dutyCycle dari 0-255
  for (int dutyCycle = 0; dutyCycle <= 255; dutyCycle++) {
    analogWrite(ledPin, dutyCycle);
    delay(10);
  }
  
  // Dari terang ke redup
  for (int dutyCycle = 255; dutyCycle >= 0; dutyCycle--) {
    analogWrite(ledPin, dutyCycle);
    delay(10);
  }
}
```

**Menggunakan LEDC untuk Kontrol PWM Lanjutan:**

ESP32 Arduino menyediakan API `ledc` untuk kontrol PWM yang lebih fleksibel:

```cpp
const int ledPin = 2;
const int freq = 5000;     // Frekuensi PWM 5 kHz
const int ledChannel = 0;  // Channel PWM (0-15)
const int resolution = 8;  // Resolusi 8-bit (0-255)

void setup() {
  // Konfigurasi LEDC
  ledcSetup(ledChannel, freq, resolution);
  
  // Hubungkan channel dengan pin GPIO
  ledcAttachPin(ledPin, ledChannel);
}

void loop() {
  // Tingkatkan duty cycle dari 0 ke nilai maksimum
  for (int dutyCycle = 0; dutyCycle < (1 << resolution); dutyCycle++) {
    ledcWrite(ledChannel, dutyCycle);
    delay(15);
  }
  
  // Turunkan duty cycle dari nilai maksimum ke 0
  for (int dutyCycle = (1 << resolution) - 1; dutyCycle >= 0; dutyCycle--) {
    ledcWrite(ledChannel, dutyCycle);
    delay(15);
  }
}
```

> 💡 **Tip**: Gunakan `ledc` saat Anda membutuhkan kontrol yang lebih presisi atas frekuensi dan resolusi PWM, terutama untuk aplikasi seperti kontrol motor servo, menghasilkan nada audio, atau dimming LED berkecepatan tinggi.

### Kalibrasi Sensor

Dalam aplikasi dunia nyata, sensor sering membutuhkan kalibrasi untuk memberikan hasil yang akurat dan bermakna.

**Kalibrasi Linier Sederhana:**

```cpp
const int sensorPin = 6;

// Nilai kalibrasi (contoh untuk sensor suhu)
// Anda perlu menentukan nilai-nilai ini melalui eksperimen
const float rawLow = 1000;    // Nilai ADC saat suhu terendah
const float rawHigh = 3500;   // Nilai ADC saat suhu tertinggi
const float tempLow = 20.0;   // Suhu terendah dalam Celsius
const float tempHigh = 80.0;  // Suhu tertinggi dalam Celsius

void setup() {
  Serial.begin(115200);
  delay(1000);
}

void loop() {
  // Baca nilai sensor
  float rawValue = analogRead(sensorPin);
  
  // Konversi ke suhu menggunakan pemetaan linier
  float temperature = mapFloat(rawValue, rawLow, rawHigh, tempLow, tempHigh);
  
  Serial.print("Nilai ADC: ");
  Serial.print(rawValue);
  Serial.print(", Suhu: ");
  Serial.print(temperature, 1);  // 1 angka di belakang koma
  Serial.println(" °C");
  
  delay(1000);
}

// Fungsi untuk memetakan nilai float (seperti map() asli tetapi dengan presisi floating-point)
float mapFloat(float x, float in_min, float in_max, float out_min, float out_max) {
  return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}
```

**Penyimpanan Nilai Kalibrasi di EEPROM:**

Untuk aplikasi serius, Anda mungkin ingin menyimpan nilai kalibrasi di memori non-volatile:

```cpp
#include <Preferences.h>

Preferences preferences;
const int sensorPin = 6;

// Nilai kalibrasi default
float rawLow = 1000;
float rawHigh = 3500;
float tempLow = 20.0;
float tempHigh = 80.0;

void setup() {
  Serial.begin(115200);
  delay(1000);
  
  // Inisialisasi Preferences untuk menyimpan/membaca data kalibrasi
  preferences.begin("sensor_cal", false);
  
  // Baca nilai kalibrasi dari penyimpanan non-volatile (jika ada)
  rawLow = preferences.getFloat("rawLow", rawLow);
  rawHigh = preferences.getFloat("rawHigh", rawHigh);
  tempLow = preferences.getFloat("tempLow", tempLow);
  tempHigh = preferences.getFloat("tempHigh", tempHigh);
  
  Serial.println("Nilai kalibrasi saat ini:");
  Serial.printf("Raw: %.1f - %.1f, Temp: %.1f - %.1f\n", rawLow, rawHigh, tempLow, tempHigh);
  Serial.println("Kirim 'c' untuk masuk mode kalibrasi");
}

void loop() {
  // Baca nilai sensor
  float rawValue = analogRead(sensorPin);
  float temperature = mapFloat(rawValue, rawLow, rawHigh, tempLow, tempHigh);
  
  Serial.printf("ADC: %.1f, Suhu: %.1f °C\n", rawValue, temperature);
  
  // Periksa apakah ada perintah kalibrasi
  if (Serial.available() > 0) {
    char cmd = Serial.read();
    if (cmd == 'c') {
      calibrateSensor();
    }
  }
  
  delay(1000);
}

void calibrateSensor() {
  Serial.println("\n=== MODE KALIBRASI ===");
  
  // Kalibrasi titik rendah
  Serial.println("Tempatkan sensor pada suhu terendah, lalu kirim nilai suhu aktual");
  while (!Serial.available()) {
    // Tunggu input
    delay(100);
  }
  tempLow = Serial.parseFloat();
  Serial.read(); // Buang karakter newline
  rawLow = analogRead(sensorPin);
  
  // Kalibrasi titik tinggi
  Serial.println("Tempatkan sensor pada suhu tertinggi, lalu kirim nilai suhu aktual");
  while (!Serial.available()) {
    // Tunggu input
    delay(100);
  }
  tempHigh = Serial.parseFloat();
  Serial.read(); // Buang karakter newline
  rawHigh = analogRead(sensorPin);
  
  // Simpan nilai kalibrasi
  preferences.putFloat("rawLow", rawLow);
  preferences.putFloat("rawHigh", rawHigh);
  preferences.putFloat("tempLow", tempLow);
  preferences.putFloat("tempHigh", tempHigh);
  
  Serial.println("Kalibrasi selesai! Nilai baru:");
  Serial.printf("Raw: %.1f - %.1f, Temp: %.1f - %.1f\n", rawLow, rawHigh, tempLow, tempHigh);
}

float mapFloat(float x, float in_min, float in_max, float out_min, float out_max) {
  return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}
```

> 💡 **Aplikasi Praktis**: Kalibrasi sangat penting untuk sensor seperti soil moisture, gas, pH, dan sensor suhu yang akurat. Simpan nilai kalibrasi di memori non-volatile agar tidak perlu mengkalibrasi ulang setiap restart.

---

## 🔍 Praktikum 1: Sistem Kontrol Lampu Interaktif

Mari kita gabungkan pengetahuan tentang input dan output digital untuk membuat sistem kontrol lampu interaktif.

**Tujuan Praktikum:**
- Membuat sistem dengan beberapa mode pencahayaan
- Menggunakan tombol untuk mengubah mode
- Mengimplementasikan debouncing tombol

**Komponen yang Dibutuhkan:**
- Board ESP32-S3
- 3 LED (merah, hijau, biru) dengan resistor 220Ω
- 1 Tombol push button
- Kabel jumper
- Breadboard

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/zExBprJ.png" width="600">
  <p><em>Rangkaian Sistem Kontrol Lampu Interaktif</em></p>
</div>

**Kode Program:**

```cpp
// Pin untuk LED dan tombol
const int redPin = 13;    // LED merah terhubung ke GPIO13
const int greenPin = 12;  // LED hijau terhubung ke GPIO12
const int bluePin = 14;   // LED biru terhubung ke GPIO14
const int buttonPin = 5;  // Tombol terhubung ke GPIO5

// Variabel untuk debouncing
unsigned long lastDebounceTime = 0;
unsigned long debounceDelay = 50;
int buttonState = HIGH;       // State saat ini (HIGH = tidak ditekan dengan pull-up)
int lastButtonState = HIGH;   // State terakhir yang dibaca

// Variabel untuk mode pencahayaan
int lightMode = 0;  // Mode pencahayaan saat ini (0-5)
unsigned long previousMillis = 0;  // Untuk efek blink non-blocking
const long blinkInterval = 500;    // Interval kedip untuk mode 3

void setup() {
  // Inisialisasi Serial untuk debugging
  Serial.begin(115200);
  
  // Inisialisasi pin LED sebagai output
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  
  // Inisialisasi pin tombol sebagai input dengan pull-up
  pinMode(buttonPin, INPUT_PULLUP);
  
  // Semua LED mati pada awalnya
  digitalWrite(redPin, LOW);
  digitalWrite(greenPin, LOW);
  digitalWrite(bluePin, LOW);
  
  Serial.println("Sistem Kontrol Lampu Interaktif");
  Serial.println("Tekan tombol untuk mengubah mode");
}

void loop() {
  // Membaca status tombol (dengan debouncing)
  int reading = digitalRead(buttonPin);
  
  // Jika status berubah, reset timer debouncing
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }
  
  // Jika tombol stabil selama waktu debounce
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika status tombol berubah
    if (reading != buttonState) {
      buttonState = reading;
      
      // Jika tombol ditekan (LOW dengan pull-up)
      if (buttonState == LOW) {
        // Ubah mode pencahayaan
        lightMode = (lightMode + 1) % 6;  // 6 mode (0-5)
        
        Serial.print("Mode berubah ke: ");
        Serial.println(lightMode);
      }
    }
  }
  
  // Simpan status pembacaan untuk perbandingan berikutnya
  lastButtonState = reading;
  
  // Terapkan mode pencahayaan yang sesuai
  applyLightMode(lightMode);
}

void applyLightMode(int mode) {
  switch (mode) {
    case 0:  // Semua LED mati
      digitalWrite(redPin, LOW);
      digitalWrite(greenPin, LOW);
      digitalWrite(bluePin, LOW);
      break;
      
    case 1:  // Hanya LED merah menyala
      digitalWrite(redPin, HIGH);
      digitalWrite(greenPin, LOW);
      digitalWrite(bluePin, LOW);
      break;
      
    case 2:  // Hanya LED hijau menyala
      digitalWrite(redPin, LOW);
      digitalWrite(greenPin, HIGH);
      digitalWrite(bluePin, LOW);
      break;
      
    case 3:  // Hanya LED biru menyala
      digitalWrite(redPin, LOW);
      digitalWrite(greenPin, LOW);
      digitalWrite(bluePin, HIGH);
      break;
      
    case 4:  // Semua LED menyala
      digitalWrite(redPin, HIGH);
      digitalWrite(greenPin, HIGH);
      digitalWrite(bluePin, HIGH);
      break;
      
    case 5:  // Mode berkedip (semua LED)
      // Implementasi non-blocking blink
      unsigned long currentMillis = millis();
      
      if (currentMillis - previousMillis >= blinkInterval) {
        // Simpan waktu terakhir berkedip
        previousMillis = currentMillis;
        
        // Toggle semua LED
        static bool ledStatus = false;
        ledStatus = !ledStatus;
        
        digitalWrite(redPin, ledStatus);
        digitalWrite(greenPin, ledStatus);
        digitalWrite(bluePin, ledStatus);
      }
      break;
  }
}
```

**Penjelasan:**
1. Program mendefinisikan pin untuk 3 LED dan 1 tombol
2. Tombol dikonfigurasi dengan pull-up internal
3. Implementasi debouncing untuk memastikan penekanan tombol terdeteksi dengan benar
4. 6 mode pencahayaan berbeda:
   - Mode 0: Semua LED mati
   - Mode 1: Hanya LED merah menyala
   - Mode 2: Hanya LED hijau menyala
   - Mode 3: Hanya LED biru menyala
   - Mode 4: Semua LED menyala
   - Mode 5: Mode berkedip (semua LED berkedip bersamaan)
5. Setiap penekanan tombol mengganti mode secara siklik
6. Mode berkedip diimplementasikan dengan cara non-blocking (tidak menggunakan delay)

> 💡 **Pengembangan**: Coba modifikasi kode untuk menambahkan mode lain seperti fade in/out menggunakan PWM atau pola kedipan yang lebih kompleks.

---

## 📈 Praktikum 2: Monitoring Lingkungan

Mari kita gunakan ADC untuk membuat sistem monitoring lingkungan sederhana.

**Tujuan Praktikum:**
- Membaca nilai dari sensor analog (LDR dan potensiometer)
- Menampilkan data ke Serial Monitor dan Serial Plotter
- Menggunakan LED untuk menunjukkan level cahaya

**Komponen yang Dibutuhkan:**
- Board ESP32-S3
- 1 LDR (Light Dependent Resistor)
- 1 Potensiometer 10kΩ
- 1 Resistor 10kΩ (untuk voltage divider dengan LDR)
- 3 LED (merah, kuning, hijau) dengan resistor 220Ω
- Kabel jumper
- Breadboard

**Rangkaian:**

<div align="center">
  <img src="https://i.imgur.com/JZWLjxz.png" width="600">
  <p><em>Rangkaian Sistem Monitoring Lingkungan</em></p>
</div>

**Kode Program:**

```cpp
// Pin untuk sensor analog
const int ldrPin = 1;        // LDR terhubung ke GPIO1 (ADC)
const int potentiometerPin = 2;  // Potensiometer terhubung ke GPIO2 (ADC)

// Pin untuk LED indikator
const int redLedPin = 13;    // LED merah (level cahaya rendah)
const int yellowLedPin = 12; // LED kuning (level cahaya sedang)
const int greenLedPin = 14;  // LED hijau (level cahaya tinggi)

// Variabel untuk pembacaan sensor
int ldrValue = 0;
int potValue = 0;
float lightPercent = 0;
float threshold1 = 30.0; // Threshold untuk LED merah-kuning
float threshold2 = 70.0; // Threshold untuk LED kuning-hijau

// Variabel untuk rata-rata bergerak
const int numReadings = 10;
int ldrReadings[numReadings];
int readIndex = 0;
int total = 0;
int average = 0;

// Variabel untuk visualisasi data
bool plotterMode = false; // Mode default: Serial Monitor

void setup() {
  // Inisialisasi Serial untuk output
  Serial.begin(115200);
  delay(1000);
  
  // Inisialisasi pin LED sebagai output
  pinMode(redLedPin, OUTPUT);
  pinMode(yellowLedPin, OUTPUT);
  pinMode(greenLedPin, OUTPUT);
  
  // Inisialisasi array untuk rata-rata bergerak
  for (int i = 0; i < numReadings; i++) {
    ldrReadings[i] = 0;
  }
  
  Serial.println("Sistem Monitoring Lingkungan");
  Serial.println("Kirim 'p' untuk toggle mode Serial Plotter");
  Serial.println("Kirim 't' diikuti angka untuk mengubah threshold (contoh: t30,70)");
}

void loop() {
  // Periksa input dari Serial
  checkSerialCommands();
  
  // Baca nilai sensor
  ldrValue = readLDRSmoothed();
  potValue = analogRead(potentiometerPin);
  
  // Konversi ke persentase cahaya (0-100%)
  lightPercent = (ldrValue / 4095.0) * 100.0;
  
  // Update threshold dari potensiometer jika diperlukan
  // (Uncomment jika Anda ingin mengontrol threshold secara dinamis)
  // float potPercent = (potValue / 4095.0) * 100.0;
  // threshold1 = potPercent * 0.5; // 0-50%
  // threshold2 = 50.0 + (potPercent * 0.5); // 50-100%
  
  // Update LED indikator
  updateLEDs(lightPercent);
  
  // Tampilkan data
  if (plotterMode) {
    // Mode Serial Plotter: output hanya nilai untuk visualisasi grafis
    Serial.print(lightPercent);
    Serial.print(" ");
    Serial.print(threshold1);
    Serial.print(" ");
    Serial.println(threshold2);
  } else {
    // Mode Serial Monitor: output dengan teks deskriptif
    Serial.print("Level Cahaya: ");
    Serial.print(lightPercent, 1);
    Serial.print("% (Raw: ");
    Serial.print(ldrValue);
    Serial.println(")");
    
    Serial.print("Posisi Potensiometer: ");
    Serial.print((potValue / 4095.0) * 100.0, 1);
    Serial.print("% (Raw: ");
    Serial.print(potValue);
    Serial.println(")");
    
    Serial.print("Thresholds: ");
    Serial.print(threshold1, 1);
    Serial.print("%, ");
    Serial.print(threshold2, 1);
    Serial.println("%");
    
    Serial.println("----------------------------");
  }
  
  // Delay antara pembacaan
  delay(plotterMode ? 100 : 1000); // Update lebih cepat dalam mode plotter
}

// Membaca LDR dengan rata-rata bergerak untuk stabilitas lebih baik
int readLDRSmoothed() {
  total = total - ldrReadings[readIndex];
  ldrReadings[readIndex] = analogRead(ldrPin);
  total = total + ldrReadings[readIndex];
  readIndex = (readIndex + 1) % numReadings;
  
  return total / numReadings;
}

// Update LED berdasarkan level cahaya
void updateLEDs(float lightLevel) {
  // Reset semua LED
  digitalWrite(redLedPin, LOW);
  digitalWrite(yellowLedPin, LOW);
  digitalWrite(greenLedPin, LOW);
  
  // Atur LED yang sesuai
  if (lightLevel < threshold1) {
    digitalWrite(redLedPin, HIGH); // Cahaya rendah
  } else if (lightLevel < threshold2) {
    digitalWrite(yellowLedPin, HIGH); // Cahaya sedang
  } else {
    digitalWrite(greenLedPin, HIGH); // Cahaya tinggi
  }
}

// Periksa perintah dari Serial
void checkSerialCommands() {
  if (Serial.available() > 0) {
    char cmd = Serial.read();
    
    if (cmd == 'p' || cmd == 'P') {
      // Toggle mode plotter
      plotterMode = !plotterMode;
      if (!plotterMode) {
        Serial.println("\nMode Serial Monitor aktif");
      }
    }
    else if (cmd == 't' || cmd == 'T') {
      // Format: t30,70 untuk set threshold1=30% dan threshold2=70%
      String threshStr = Serial.readStringUntil('\n');
      int commaIndex = threshStr.indexOf(',');
      
      if (commaIndex > 0) {
        threshold1 = threshStr.substring(0, commaIndex).toFloat();
        threshold2 = threshStr.substring(commaIndex + 1).toFloat();
        
        Serial.print("Threshold diubah: ");
        Serial.print(threshold1);
        Serial.print("%, ");
        Serial.print(threshold2);
        Serial.println("%");
      }
    }
    
    // Buang karakter yang tersisa
    while (Serial.available() > 0) {
      Serial.read();
    }
  }
}
```

**Penjelasan:**
1. Program membaca nilai dari dua sensor analog:
   - LDR untuk mengukur level cahaya
   - Potensiometer yang bisa digunakan untuk mengontrol threshold
2. Implementasi rata-rata bergerak (moving average) untuk stabilisasi pembacaan LDR
3. Konversi nilai ADC ke persentase (0-100%)
4. LED indikator menunjukkan level cahaya:
   - LED merah: cahaya rendah (di bawah threshold1)
   - LED kuning: cahaya sedang (antara threshold1 dan threshold2)
   - LED hijau: cahaya tinggi (di atas threshold2)
5. Dua mode tampilan data:
   - Serial Monitor: menampilkan data deskriptif
   - Serial Plotter: menampilkan grafik nilai
6. Threshold dapat diubah melalui perintah Serial

**Menggunakan Serial Plotter:**

Untuk melihat visualisasi grafis dari data sensor:
1. Unggah kode ke ESP32-S3
2. Buka Serial Monitor dan kirim 'p' untuk beralih ke mode plotter
3. Tutup Serial Monitor dan buka Serial Plotter (Tools > Serial Plotter)
4. Anda akan melihat grafik level cahaya dan threshold

<div align="center">
  <img src="https://i.imgur.com/2xObU85.png" width="600">
  <p><em>Visualisasi Data dengan Serial Plotter</em></p>
</div>

> 💡 **Pengembangan**: Coba tambahkan sensor lain seperti sensor suhu (NTC thermistor atau DS18B20) atau sensor kelembaban tanah untuk membuat stasiun pemantauan lingkungan yang lebih lengkap.

---

## 🏋️ Latihan Mandiri

Sekarang saatnya Anda mencoba sendiri untuk memastikan pemahaman. Cobalah tantangan-tantangan berikut:

### Latihan 1: LED Dimmer dengan Potensiometer

Buat rangkaian yang menggunakan potensiometer untuk mengontrol kecerahan LED menggunakan PWM.

**Petunjuk:**
- Gunakan potensiometer untuk input analog
- Konversi nilai analog (0-4095) ke nilai PWM (0-255 atau sesuai resolusi yang dipilih)
- Gunakan fungsi `ledcWrite()` untuk mengontrol kecerahan LED
- Bonus: Tambahkan "animasi" pada perubahan kecerahan (transisi halus)

### Latihan 2: Saklar Sentuh Kapasitif

Manfaatkan kemampuan touch sensing ESP32-S3 untuk membuat saklar yang diaktifkan dengan sentuhan.

**Petunjuk:**
- Gunakan fungsi `touchRead()` untuk membaca sensor sentuh
- Tentukan threshold yang sesuai untuk deteksi sentuhan
- Implementasikan toggle state (sentuh sekali untuk ON, sentuh lagi untuk OFF)
- Bonus: Tambahkan beberapa pad sentuh untuk mengontrol berbagai fungsi

### Latihan 3: Alarm Batas Analog

Buat sistem yang membunyikan alarm (LED berkedip dan/atau buzzer) saat pembacaan sensor analog melewati batas tertentu.

**Petunjuk:**
- Gunakan LDR, sensor suhu, atau potensiometer sebagai input
- Tetapkan nilai batas atas dan batas bawah
- Aktifkan alarm saat nilai melewati salah satu batas
- Implementasikan hysteresis untuk mencegah alarm "chattering" di sekitar nilai batas
- Bonus: Tambahkan tombol untuk mengatur ulang alarm dan/atau mengubah nilai batas

### Latihan 4: Monitor Baterai Sederhana

Buat sistem pemantauan tegangan baterai sederhana menggunakan pembagi tegangan.

**Petunjuk:**
- Buat pembagi tegangan untuk menurunkan tegangan baterai ke rentang aman untuk ADC
- Kalibrasi pembacaan ADC untuk menampilkan tegangan yang benar
- Tampilkan persentase baterai berdasarkan tegangan (misalnya: 3.7V = 100%, 3.0V = 0% untuk baterai Li-ion)
- Gunakan LED sebagai indikator level baterai
- Bonus: Implementasikan peringatan saat baterai hampir habis

---

## 📚 Sumber Referensi

1. Espressif Systems. (2023). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

2. Espressif Systems. (2023). *ESP32-S3 Datasheet*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)

3. Arduino. (2023). *Arduino Reference*. [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/)

4. Espressif Systems. (2023). *ESP-IDF Programming Guide: Analog to Digital Converter*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/adc.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/adc.html)

5. Espressif Systems. (2023). *ESP-IDF Programming Guide: Pulse Width Modulation*. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/ledc.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/ledc.html)

6. RandomNerdTutorials. (2023). *ESP32 GPIO Pins*. [https://randomnerdtutorials.com/esp32-pinout-reference-gpios/](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)

7. Jones, D. (2021). *Debouncing Mechanisms in Embedded Systems*. Electronics Design Journal, 45(3), 78-92.

8. Wilson, J. (2022). *Sensor Calibration Techniques for IoT Devices*. Practical Electronics, 34(2), 45-52.

---

[➡️ Lanjut ke Modul 3: Komunikasi dan Tampilan dengan ESP32-S3](https://github.com/micro-boy/microcontroller-esp32s3-arduino/blob/main/modul-3.md)
---
