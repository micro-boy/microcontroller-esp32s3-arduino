# 📚 Modul 2: Pemrograman dan Peripheral Dasar ESP32 S3

<div align="center">
  
  ![ESP32 S3 GPIO](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/_images/ESP32-S3_DevKitC-1_pinlayout.jpg)

  *Diagram Pin ESP32-S3 Development Board*
</div>

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan modul ini, Anda diharapkan dapat:
- Memahami struktur program embedded pada platform Arduino
- Mengimplementasikan operasi GPIO untuk input dan output
- Menerapkan teknik debouncing untuk input button
- Menggunakan timer dan PWM untuk kontrol perangkat
- Mengkonfigurasi ADC dan membaca sensor analog
- Menerapkan pola state machine sederhana

---

## 📖 Minggu 3: Fundamental Pemrograman Embedded

### 🔍 Struktur Program dalam Sistem Tertanam

#### Karakteristik Program Embedded

Program pada sistem tertanam memiliki beberapa karakteristik khas yang membedakannya dari aplikasi desktop atau mobile:

1. **Loop Tak Terbatas**: Program dijalankan terus-menerus selama perangkat menyala
2. **Resource Terbatas**: Memori, daya komputasi, dan daya listrik sangat terbatas
3. **Interaksi dengan Hardware**: Berhubungan langsung dengan peripheral dan perangkat luar
4. **Real-time Constraints**: Sering memiliki persyaratan waktu yang ketat untuk respons
5. **Keandalan Tinggi**: Harus tetap berfungsi tanpa error atau reset

#### Struktur Program Arduino

Pada platform Arduino, setiap program (sketch) memiliki struktur dasar dengan dua fungsi utama:

```cpp
// Deklarasi variabel global dan include library
#include <Arduino.h>

// Fungsi setup() - dijalankan sekali saat board dinyalakan atau di-reset
void setup() {
  // Inisialisasi:
  // - Konfigurasi pin
  // - Inisialisasi komunikasi serial
  // - Setup sensor dan aktuator
  // - Inisialisasi variabel
}

// Fungsi loop() - dijalankan berulang-ulang setelah setup() selesai
void loop() {
  // Kode utama yang dijalankan berulang-ulang:
  // - Baca sensor
  // - Proses data
  // - Aktifkan aktuator
  // - Komunikasi dengan perangkat luar
}
```

> 💡 **Insight**: Struktur ini disebut sebagai *"Super Loop"* atau *"Cyclic Executive Model"* dalam desain sistem embedded. Model ini sederhana dan deterministik, meskipun memiliki keterbatasan untuk task yang memerlukan timing presisi.

#### Pola Program Embedded

Ada beberapa pola pemrograman yang umum digunakan dalam sistem tertanam:

1. **Polling vs Interrupt**

   ```cpp
   // Pendekatan Polling - memeriksa perubahan secara terus-menerus
   void loop() {
     buttonState = digitalRead(buttonPin);
     if (buttonState == LOW) {
       // Button ditekan, lakukan aksi
     }
   }
   
   // Pendekatan Interrupt - bereaksi saat event terjadi
   void setup() {
     pinMode(buttonPin, INPUT_PULLUP);
     attachInterrupt(digitalPinToInterrupt(buttonPin), handleButton, FALLING);
   }
   
   void handleButton() {
     // Button ditekan, lakukan aksi
   }
   ```

2. **Blocking vs Non-blocking**

   ```cpp
   // Pendekatan Blocking - menghentikan eksekusi program selama operasi
   void loop() {
     digitalWrite(ledPin, HIGH);
     delay(1000);  // Program berhenti selama 1 detik
     digitalWrite(ledPin, LOW);
     delay(1000);  // Program berhenti lagi selama 1 detik
   }
   
   // Pendekatan Non-blocking - menggunakan timestamp untuk timing
   unsigned long previousMillis = 0;
   const long interval = 1000;
   int ledState = LOW;
   
   void loop() {
     unsigned long currentMillis = millis();
     
     if (currentMillis - previousMillis >= interval) {
       previousMillis = currentMillis;
       
       // Toggle LED
       ledState = (ledState == LOW) ? HIGH : LOW;
       digitalWrite(ledPin, ledState);
     }
     
     // Kode lain dapat berjalan disini tanpa tertunda
   }
   ```

3. **State Machine**

   ```cpp
   // Implementasi State Machine sederhana
   enum State {
     STATE_IDLE,
     STATE_ACTIVE,
     STATE_ERROR
   };
   
   State currentState = STATE_IDLE;
   
   void loop() {
     // Baca input
     readSensors();
     
     // Update state berdasarkan input dan state saat ini
     switch (currentState) {
       case STATE_IDLE:
         updateIdleState();
         break;
       case STATE_ACTIVE:
         updateActiveState();
         break;
       case STATE_ERROR:
         updateErrorState();
         break;
     }
     
     // Update output berdasarkan state saat ini
     updateOutputs();
   }
   ```

### 🔌 Operasi GPIO pada ESP32 S3

#### Konfigurasi dan Jenis Pin

ESP32 S3 memiliki 45 GPIO pin yang dapat diprogram. Beberapa pin memiliki fungsi khusus:

<div align="center">
  
| Tipe Pin | Deskripsi | Pin ESP32 S3 |
|---------|-----------|--------------|
| Digital I/O | Pin input/output digital standar | Mayoritas pin |
| ADC | Analog-to-Digital Converter | GPIO1-14 |
| DAC | Digital-to-Analog Converter | GPIO17, GPIO18 |
| Touch | Capacitive touch sensors | GPIO1-14 |
| UART | Serial communication | Berbagai pin, konfigurasi fleksibel |
| I²C | I²C communication | Berbagai pin, konfigurasi fleksibel |
| SPI | SPI communication | Berbagai pin, konfigurasi fleksibel |
| PWM | Pulse Width Modulation | Semua GPIO digital |

</div>

> ⚠️ **Perhatian**: Beberapa pin memiliki keterbatasan atau fungsi khusus saat booting. Selalu periksa diagram pin dan dokumentasi sebelum menggunakan pin untuk fungsi tertentu.

#### Operasi Digital I/O

```cpp
// Konfigurasi pin sebagai output
void setup() {
  pinMode(LED_PIN, OUTPUT);  // Siapkan pin sebagai output
  
  // Alternatif untuk pinMode
  // ledcAttachPin(LED_PIN, CHANNEL); // Untuk PWM
}

// Operasi output dasar
void setOutputExample() {
  digitalWrite(LED_PIN, HIGH);  // Set pin ke level HIGH (3.3V)
  delay(1000);
  digitalWrite(LED_PIN, LOW);   // Set pin ke level LOW (0V)
  delay(1000);
}

// Konfigurasi pin sebagai input
void setupInputExample() {
  pinMode(BUTTON_PIN, INPUT);         // Input standar tanpa pull-up/down
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Input dengan resistor pull-up internal
  // pinMode(BUTTON_PIN, INPUT_PULLDOWN); // Input dengan pull-down (ESP32 only)
}

// Membaca input digital
void readInputExample() {
  int buttonState = digitalRead(BUTTON_PIN);  // Baca status pin (HIGH atau LOW)
  
  if (buttonState == HIGH) {
    // Pin dalam keadaan HIGH
  } else {
    // Pin dalam keadaan LOW
  }
}
```

#### Teknik Debouncing

Debouncing adalah teknik untuk mengatasi fluktuasi sinyal yang terjadi ketika saklar mekanik (seperti tombol) ditekan. Tanpa debouncing, satu penekanan tombol bisa terbaca sebagai multiple presses.

1. **Software Debouncing dengan Delay**

   ```cpp
   // Debouncing sederhana dengan delay
   void handleButtonPress() {
     if (digitalRead(BUTTON_PIN) == LOW) {  // Button ditekan (active low dengan pull-up)
       // Lakukan aksi
       digitalWrite(LED_PIN, !digitalRead(LED_PIN));  // Toggle LED
       
       delay(50);  // Tunggu 50ms untuk menghindari bouncing
       
       // Tunggu sampai button dilepas
       while (digitalRead(BUTTON_PIN) == LOW) {
         // Do nothing
       }
       
       delay(50);  // Debounce saat button dilepas
     }
   }
   ```

2. **Debouncing dengan Timestamp**

   ```cpp
   // Debouncing dengan timestamp untuk pendekatan non-blocking
   unsigned long lastDebounceTime = 0;  // Waktu terakhir button berubah status
   unsigned long debounceDelay = 50;    // Waktu debounce dalam ms
   int lastButtonState = HIGH;          // Status button terakhir
   int buttonState = HIGH;              // Status button saat ini (diinisialisasi HIGH untuk pull-up)
   
   void debounceButton() {
     // Baca status button saat ini
     int reading = digitalRead(BUTTON_PIN);
     
     // Cek apakah button baru saja berubah status
     if (reading != lastButtonState) {
       // Reset timer debounce
       lastDebounceTime = millis();
     }
     
     // Cek apakah sudah lewat waktu debounce
     if ((millis() - lastDebounceTime) > debounceDelay) {
       // Jika status button telah berubah dan stabil
       if (reading != buttonState) {
         buttonState = reading;
         
         // Hanya jalankan aksi saat button ditekan (LOW dengan INPUT_PULLUP)
         if (buttonState == LOW) {
           // Lakukan aksi saat button ditekan
           digitalWrite(LED_PIN, !digitalRead(LED_PIN));  // Toggle LED
         }
       }
     }
     
     // Simpan status button untuk perbandingan di iterasi berikutnya
     lastButtonState = reading;
   }
   ```

### 🧠 State Machine Pattern

State machine (mesin status) adalah teknik pemrograman yang sangat berguna untuk aplikasi embedded. Mesin status memungkinkan program berjalan melalui berbagai kondisi (state) yang terdefinisi dengan jelas, dengan transisi antar-state yang terkontrol.

```cpp
// Implementasi state machine untuk kontrol lampu dengan tiga mode
enum LightState {
  STATE_OFF,       // Lampu mati
  STATE_LOW,       // Kecerahan rendah
  STATE_MEDIUM,    // Kecerahan sedang
  STATE_HIGH,      // Kecerahan tinggi
  STATE_BLINKING   // Mode berkedip
};

LightState currentState = STATE_OFF;  // Status awal
unsigned long lastUpdateTime = 0;     // Untuk timing non-blocking
int ledPin = 2;                      // Pin LED
int buttonPin = 4;                   // Pin button
bool ledState = false;               // Status LED untuk mode berkedip

void setup() {
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  Serial.println("State Machine Example");
}

void loop() {
  // Handle button press dengan debouncing
  static bool lastButtonState = HIGH;
  static unsigned long lastDebounceTime = 0;
  
  int reading = digitalRead(buttonPin);
  
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }
  
  if ((millis() - lastDebounceTime) > 50) {  // 50ms debounce time
    if (reading == LOW && lastButtonState == HIGH) {  // Button ditekan
      // Transisi state
      switch (currentState) {
        case STATE_OFF:
          currentState = STATE_LOW;
          Serial.println("State: LOW");
          break;
        case STATE_LOW:
          currentState = STATE_MEDIUM;
          Serial.println("State: MEDIUM");
          break;
        case STATE_MEDIUM:
          currentState = STATE_HIGH;
          Serial.println("State: HIGH");
          break;
        case STATE_HIGH:
          currentState = STATE_BLINKING;
          Serial.println("State: BLINKING");
          break;
        case STATE_BLINKING:
          currentState = STATE_OFF;
          Serial.println("State: OFF");
          break;
      }
    }
  }
  
  lastButtonState = reading;
  
  // Update output berdasarkan state saat ini
  switch (currentState) {
    case STATE_OFF:
      digitalWrite(ledPin, LOW);
      break;
    case STATE_LOW:
      analogWrite(ledPin, 64);  // ~25% kecerahan
      break;
    case STATE_MEDIUM:
      analogWrite(ledPin, 127);  // ~50% kecerahan
      break;
    case STATE_HIGH:
      analogWrite(ledPin, 255);  // 100% kecerahan
      break;
    case STATE_BLINKING:
      // Non-blocking blinking
      if (millis() - lastUpdateTime > 500) {  // Toggle setiap 500ms
        lastUpdateTime = millis();
        ledState = !ledState;
        digitalWrite(ledPin, ledState ? HIGH : LOW);
      }
      break;
  }
}
```

### 💻 Latihan Praktikum: Implementasi State Machine

#### Tujuan:
- Memahami konsep state machine
- Mengimplementasikan transisi state berdasarkan input
- Menerapkan debouncing untuk input button

#### Latihan: State Machine untuk Lampu Lalu Lintas

1. Buat state machine untuk mengontrol lampu lalu lintas (merah, kuning, hijau)
2. Implementasikan timing yang tepat untuk setiap state
3. Tambahkan button untuk switch antara mode normal dan mode berkedip (emergency mode)

```cpp
// Starter code untuk lampu lalu lintas
enum TrafficLightState {
  STATE_RED,
  STATE_RED_YELLOW,
  STATE_GREEN,
  STATE_YELLOW,
  STATE_EMERGENCY
};

// Pin untuk LED
const int RED_PIN = 14;
const int YELLOW_PIN = 12;
const int GREEN_PIN = 13;
const int BUTTON_PIN = 4;

TrafficLightState currentState = STATE_RED;
unsigned long lastStateChangeTime = 0;
bool emergencyMode = false;

// Durasi setiap state dalam ms
const unsigned long RED_DURATION = 5000;      // 5 detik
const unsigned long RED_YELLOW_DURATION = 2000; // 2 detik
const unsigned long GREEN_DURATION = 5000;    // 5 detik
const unsigned long YELLOW_DURATION = 2000;   // 2 detik
const unsigned long BLINK_DURATION = 500;     // 0.5 detik untuk kedipan

void setup() {
  pinMode(RED_PIN, OUTPUT);
  pinMode(YELLOW_PIN, OUTPUT);
  pinMode(GREEN_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  Serial.begin(115200);
  Serial.println("Traffic Light State Machine");
}

void loop() {
  // Bagian 1: Handle button press untuk toggle emergency mode
  static bool lastButtonState = HIGH;
  static unsigned long lastDebounceTime = 0;
  int reading = digitalRead(BUTTON_PIN);
  
  // Implementasikan debouncing dan toggle emergencyMode
  // ...

  // Bagian 2: Update state berdasarkan timing
  unsigned long currentTime = millis();
  
  if (!emergencyMode) {
    // Mode normal - ganti state berdasarkan timing
    switch (currentState) {
      case STATE_RED:
        // Lampu merah menyala, yang lain mati
        digitalWrite(RED_PIN, HIGH);
        digitalWrite(YELLOW_PIN, LOW);
        digitalWrite(GREEN_PIN, LOW);
        
        // Cek waktu untuk transisi state
        if (currentTime - lastStateChangeTime >= RED_DURATION) {
          currentState = STATE_RED_YELLOW;
          lastStateChangeTime = currentTime;
          Serial.println("State: RED+YELLOW");
        }
        break;
        
      // Tambahkan case untuk STATE_RED_YELLOW, STATE_GREEN, dan STATE_YELLOW
      // ...
    }
  } else {
    // Mode emergency - kedipkan lampu kuning
    // ...
  }
}
```

---

## 📖 Minggu 4: Timer dan PWM

### ⏱️ Memahami Timer Hardware

#### Konsep Dasar Timer

Timer adalah peripheral hardware yang menghitung pulsa clock untuk mengukur waktu dengan presisi. Timer dapat digunakan untuk:

- Menghasilkan delay yang presisi
- Mengukur interval waktu
- Menghasilkan sinyal PWM (Pulse Width Modulation)
- Menangkap timing dari sinyal input
- Membuat interrupt periodik

ESP32 S3 memiliki timer hardware yang sangat fleksibel, tetapi Arduino framework menyediakan abstraksi yang memudahkan penggunaannya.

#### Fungsi Timing pada Arduino

```cpp
// Fungsi timing dasar
void timingFunctions() {
  // delay() - blocking delay dalam milliseconds
  delay(1000);  // Tunggu 1 detik, blocking
  
  // delayMicroseconds() - blocking delay dalam microseconds
  delayMicroseconds(10);  // Tunggu 10 microseconds, blocking
  
  // millis() - mendapatkan timestamp dalam milliseconds sejak board reset
  unsigned long currentMillis = millis();
  
  // micros() - mendapatkan timestamp dalam microseconds sejak board reset
  unsigned long currentMicros = micros();
}
```

#### Implementasi Timing Non-blocking

```cpp
// Implementasi multiple timing event non-blocking
unsigned long previousMillis1 = 0;
unsigned long previousMillis2 = 0;
unsigned long previousMillis3 = 0;

const long interval1 = 1000;  // 1 detik
const long interval2 = 2000;  // 2 detik
const long interval3 = 5000;  // 5 detik

void setup() {
  Serial.begin(115200);
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
}

void loop() {
  unsigned long currentMillis = millis();
  
  // Task 1: Toggle LED_1 setiap 1 detik
  if (currentMillis - previousMillis1 >= interval1) {
    previousMillis1 = currentMillis;
    digitalWrite(LED_1, !digitalRead(LED_1));
    Serial.println("Task 1 executed");
  }
  
  // Task 2: Toggle LED_2 setiap 2 detik
  if (currentMillis - previousMillis2 >= interval2) {
    previousMillis2 = currentMillis;
    digitalWrite(LED_2, !digitalRead(LED_2));
    Serial.println("Task 2 executed");
  }
  
  // Task 3: Toggle LED_3 setiap 5 detik
  if (currentMillis - previousMillis3 >= interval3) {
    previousMillis3 = currentMillis;
    digitalWrite(LED_3, !digitalRead(LED_3));
    Serial.println("Task 3 executed");
  }
  
  // Kode lain dapat dieksekusi disini tanpa blocking
}
```

> 💡 **Insight**: Penggunaan millis() untuk timing non-blocking adalah teknik dasar namun sangat kuat untuk multitasking sederhana pada sistem yang tidak menggunakan RTOS.

### 📊 Pulse Width Modulation (PWM)

#### Konsep PWM

PWM (Pulse Width Modulation) adalah teknik untuk menghasilkan sinyal digital yang dapat diinterpretasikan sebagai nilai analog dengan mengubah proporsi waktu sinyal HIGH terhadap satu siklus penuh (periode). Rasio waktu HIGH terhadap periode disebut *duty cycle*.

<div align="center">
  
  ![PWM Duty Cycle](https://lastminuteengineers.com/wp-content/uploads/arduino/Arduino-PWM-Duty-Cycle-Animation.gif)

  *Ilustrasi Duty Cycle PWM*
</div>

Aplikasi umum PWM:
- Kontrol kecerahan LED
- Kontrol kecepatan motor
- Menghasilkan sinyal audio
- Komunikasi dengan sensor tertentu

#### Implementasi PWM pada ESP32 S3

ESP32 S3 memiliki peripheral LED Control (LEDC) yang sangat canggih untuk menghasilkan sinyal PWM. Arduino framework menyediakan abstraksi dengan fungsi `analogWrite()` tetapi juga memungkinkan akses ke fitur LEDC yang lebih lanjut.

```cpp
// Implementasi dasar PWM
const int LED_PIN = 2;    // Pin LED
const int BUTTON_PIN = 4; // Pin Button

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Untuk ESP32, analogWrite() tersedia melalui library "analogWrite"
  // atau dapat menggunakan LEDC
  
  // Setup LEDC untuk PWM
  ledcSetup(0, 5000, 8);  // Channel 0, 5000 Hz, 8-bit resolution (0-255)
  ledcAttachPin(LED_PIN, 0);  // Attach LED_PIN ke channel 0
}

void loop() {
  // Fade in LED
  Serial.println("Fade in");
  for (int dutyCycle = 0; dutyCycle <= 255; dutyCycle++) {
    ledcWrite(0, dutyCycle);  // Set duty cycle
    delay(5);  // Kecepatan fade
  }
  
  delay(500);  // Jeda saat full brightness
  
  // Fade out LED
  Serial.println("Fade out");
  for (int dutyCycle = 255; dutyCycle >= 0; dutyCycle--) {
    ledcWrite(0, dutyCycle);  // Set duty cycle
    delay(5);  // Kecepatan fade
  }
  
  delay(500);  // Jeda saat fully off
}
```

#### Kontrol Servo dengan PWM

Servo motor dikontrol dengan pulsa PWM, dengan lebar pulsa menentukan posisi servo.

```cpp
#include <ESP32Servo.h>

Servo myServo;
const int SERVO_PIN = 13;  // Pin untuk servo
const int POT_PIN = 36;    // Pin untuk potentiometer (ADC)

void setup() {
  Serial.begin(115200);
  
  // Konfigurasi servo
  ESP32PWM::allocateTimer(0);  // Alokasikan timer untuk servo library
  myServo.setPeriodHertz(50);  // Standard 50Hz servo
  myServo.attach(SERVO_PIN, 500, 2400);  // Attaches pin, min/max pulsewidth
  
  Serial.println("Servo Control with Potentiometer");
}

void loop() {
  // Baca nilai potentiometer (0-4095 pada ESP32 ADC)
  int potValue = analogRead(POT_PIN);
  
  // Map nilai potentiometer ke sudut servo (0-180)
  int angle = map(potValue, 0, 4095, 0, 180);
  
  // Set sudut servo
  myServo.write(angle);
  
  // Debug output
  Serial.print("Potentiometer: ");
  Serial.print(potValue);
  Serial.print(" | Angle: ");
  Serial.println(angle);
  
  delay(15);  // Sedikit delay untuk stabilitas
}
```

### 💻 Latihan Praktikum: Kontrol RGB LED dengan PWM

#### Tujuan:
- Memahami prinsip PWM
- Mengimplementasikan kontrol PWM untuk RGB LED
- Membuat transisi warna yang halus

#### Latihan: RGB LED Color Mixer

```cpp
// RGB LED Color Mixer
const int RED_PIN = 25;    // Pin untuk R channel
const int GREEN_PIN = 26;  // Pin untuk G channel
const int BLUE_PIN = 27;   // Pin untuk B channel

// Untuk ESP32 PWM setup
const int RED_CHANNEL = 0;
const int GREEN_CHANNEL = 1;
const int BLUE_CHANNEL = 2;

const int FREQ = 5000;       // Frekuensi PWM 5KHz
const int RESOLUTION = 8;    // 8-bit resolution (0-255)

void setup() {
  Serial.begin(115200);
  
  // Setup PWM channels
  ledcSetup(RED_CHANNEL, FREQ, RESOLUTION);
  ledcSetup(GREEN_CHANNEL, FREQ, RESOLUTION);
  ledcSetup(BLUE_CHANNEL, FREQ, RESOLUTION);
  
  // Attach pins to channels
  ledcAttachPin(RED_PIN, RED_CHANNEL);
  ledcAttachPin(GREEN_PIN, GREEN_CHANNEL);
  ledcAttachPin(BLUE_PIN, BLUE_CHANNEL);
  
  Serial.println("RGB LED Color Mixer");
}

void loop() {
  // 1. Red to Yellow transition
  for (int i = 0; i <= 255; i++) {
    setColor(255, i, 0);  // Increase Green
    delay(5);
  }
  
  // 2. Yellow to Green transition
  for (int i = 255; i >= 0; i--) {
    setColor(i, 255, 0);  // Decrease Red
    delay(5);
  }
  
  // 3. Green to Cyan transition
  for (int i = 0; i <= 255; i++) {
    setColor(0, 255, i);  // Increase Blue
    delay(5);
  }
  
  // 4. Cyan to Blue transition
  for (int i = 255; i >= 0; i--) {
    setColor(0, i, 255);  // Decrease Green
    delay(5);
  }
  
  // 5. Blue to Magenta transition
  for (int i = 0; i <= 255; i++) {
    setColor(i, 0, 255);  // Increase Red
    delay(5);
  }
  
  // 6. Magenta to Red transition
  for (int i = 255; i >= 0; i--) {
    setColor(255, 0, i);  // Decrease Blue
    delay(5);
  }
}

// Helper function to set RGB color
void setColor(int red, int green, int blue) {
  ledcWrite(RED_CHANNEL, red);
  ledcWrite(GREEN_CHANNEL, green);
  ledcWrite(BLUE_CHANNEL, blue);
  
  // Optional: Serial output untuk debug
  // Serial.printf("R: %d, G: %d, B: %d\n", red, green, blue);
}
```

---

## 📖 Minggu 5: Analog Interfaces dan Sensor

### 📈 Analog-to-Digital Conversion (ADC)

#### Prinsip ADC

ADC (Analog-to-Digital Converter) mengubah sinyal analog (tegangan kontinu) menjadi nilai digital yang dapat diproses oleh microcontroller. ESP32 S3 memiliki ADC 12-bit yang dapat mengukur tegangan dari 0V hingga 3.3V.

<div align="center">
  
  ![ADC Principle](https://www.researchgate.net/publication/337742273/figure/fig1/AS:831935457226752@1575278213842/Principle-of-analog-to-digital-converter-ADC.png)

  *Prinsip Kerja ADC*
</div>

#### Konfigurasi ADC pada ESP32 S3

ESP32 S3 memiliki dua blok ADC (ADC1 dan ADC2) dengan total 20 channel. Arduino framework menyederhanakan penggunaan ADC dengan fungsi `analogRead()`.

```cpp
// Membaca nilai ADC dasar
const int ANALOG_PIN = 36;  // GPIO36 / ADC1_0

void setup() {
  Serial.begin(115200);
  // Konfigurasi ADC
  analogReadResolution(12);  // Set resolusi ke 12-bit (0-4095)
  
  // Untuk konfigurasi lanjutan, dapat menggunakan:
  // analogSetWidth(12);         // Bit resolution
  // analogSetAttenuation(ADC_11db); // Input attenuation
  
  Serial.println("ADC Reading Example");
}

void loop() {
  // Baca nilai ADC
  int rawValue = analogRead(ANALOG_PIN);
  
  // Konversi ke tegangan (0-3.3V)
  float voltage = rawValue * (3.3 / 4095.0);
  
  Serial.print("Raw ADC: ");
  Serial.print(rawValue);
  Serial.print(", Voltage: ");
  Serial.print(voltage, 2);  // 2 decimal places
  Serial.println("V");
  
  delay(500);
}
```

#### Teknik Filtering dan Smoothing

Nilai ADC sering kali mengandung noise. Beberapa teknik filtering dapat digunakan untuk mendapatkan nilai yang lebih stabil:

```cpp
// Smoothing ADC dengan Moving Average
const int ANALOG_PIN = 36;
const int NUM_READINGS = 10;

int readings[NUM_READINGS];  // Array untuk menyimpan readings
int readIndex = 0;           // Index reading saat ini
int total = 0;               // Running total
int average = 0;             // Nilai rata-rata

void setup() {
  Serial.begin(115200);
  
  // Inisialisasi array readings dengan 0
  for (int i = 0; i < NUM_READINGS; i++) {
    readings[i] = 0;
  }
}

void loop() {
  // Subtrak reading terakhir
  total = total - readings[readIndex];
  
  // Baca dari sensor
  readings[readIndex] = analogRead(ANALOG_PIN);
  
  // Tambahkan ke total
  total = total + readings[readIndex];
  
  // Maju ke posisi berikutnya di array
  readIndex = readIndex + 1;
  
  // Reset index jika sudah di akhir array
  if (readIndex >= NUM_READINGS) {
    readIndex = 0;
  }
  
  // Hitung rata-rata
  average = total / NUM_READINGS;
  
  // Konversi ke tegangan
  float voltage = average * (3.3 / 4095.0);
  
  Serial.print("Raw: ");
  Serial.print(readings[readIndex]);
  Serial.print(", Average: ");
  Serial.print(average);
  Serial.print(", Voltage: ");
  Serial.print(voltage, 2);
  Serial.println("V");
  
  delay(100);
}
```

#### Pembacaan Multiple ADC Channels

```cpp
// Membaca multiple ADC channels
const int SENSOR1_PIN = 36;  // ADC1_0
const int SENSOR2_PIN = 39;  // ADC1_3
const int SENSOR3_PIN = 34;  // ADC1_6

void setup() {
  Serial.begin(115200);
  Serial.println("Multiple ADC Channels Example");
}

void loop() {
  // Baca nilai dari ketiga sensor
  int sensor1Value = analogRead(SENSOR1_PIN);
  int sensor2Value = analogRead(SENSOR2_PIN);
  int sensor3Value = analogRead(SENSOR3_PIN);
  
  // Konversi ke tegangan
  float voltage1 = sensor1Value * (3.3 / 4095.0);
  float voltage2 = sensor2Value * (3.3 / 4095.0);
  float voltage3 = sensor3Value * (3.3 / 4095.0);
  
  // Print hasil
  Serial.print("Sensor 1: ");
  Serial.print(sensor1Value);
  Serial.print(" (");
  Serial.print(voltage1, 2);
  Serial.print("V), Sensor 2: ");
  Serial.print(sensor2Value);
  Serial.print(" (");
  Serial.print(voltage2, 2);
  Serial.print("V), Sensor 3: ");
  Serial.print(sensor3Value);
  Serial.print(" (");
  Serial.print(voltage3, 2);
  Serial.println("V)");
  
  delay(500);
}
```

### 🌡️ Interfacing dengan Sensor Analog

#### Sensor Analog Dasar

1. **Sensor Cahaya (LDR/Photoresistor)**

   ```cpp
   // Sensor cahaya dengan LDR
   const int LDR_PIN = 36;
   
   void setup() {
     Serial.begin(115200);
     Serial.println("Light Sensor Example");
   }
   
   void loop() {
     int lightLevel = analogRead(LDR_PIN);
     
     // Map nilai ADC ke persentase (0-100%)
     int lightPercent = map(lightLevel, 0, 4095, 0, 100);
     
     Serial.print("Light level: ");
     Serial.print(lightLevel);
     Serial.print(" (");
     Serial.print(lightPercent);
     Serial.println("%)");
     
     // Contoh kontrol berdasarkan level cahaya
     if (lightPercent < 30) {
       Serial.println("It's dark, turning ON the light");
       // digitalWrite(LIGHT_PIN, HIGH);
     } else {
       Serial.println("It's bright, turning OFF the light");
       // digitalWrite(LIGHT_PIN, LOW);
     }
     
     delay(1000);
   }
   ```

2. **Sensor Suhu (Thermistor)**

   ```cpp
   // Membaca suhu dengan thermistor
   const int THERMISTOR_PIN = 36;
   
   // Konstanta thermistor
   const float BETA = 3950;  // Nilai Beta thermistor
   const float R0 = 10000;   // Nilai resistor pada 25°C
   const float T0 = 298.15;  // Temperatur referensi dalam Kelvin (25°C)
   const float SERIES_RESISTOR = 10000;  // Resistor seri 10K
   
   void setup() {
     Serial.begin(115200);
     Serial.println("Thermistor Temperature Sensor");
   }
   
   void loop() {
     // Baca nilai ADC
     int rawADC = analogRead(THERMISTOR_PIN);
     
     // Konversi ADC ke resistansi
     float resistance = SERIES_RESISTOR / ((4095.0 / rawADC) - 1.0);
     
     // Konversi resistansi ke suhu (dalam Kelvin) menggunakan persamaan Beta
     float steinhart = resistance / R0;
     steinhart = log(steinhart);
     steinhart /= BETA;
     steinhart += 1.0 / T0;
     steinhart = 1.0 / steinhart;
     
     // Konversi Kelvin ke Celsius
     float tempC = steinhart - 273.15;
     
     // Konversi Celsius ke Fahrenheit
     float tempF = tempC * 9.0 / 5.0 + 32.0;
     
     Serial.print("Temperature: ");
     Serial.print(tempC, 2);
     Serial.print("°C / ");
     Serial.print(tempF, 2);
     Serial.println("°F");
     
     delay(1000);
   }
   ```

3. **Sensor Jarak (Infrared Analog)**

   ```cpp
   // Sensor jarak Sharp GP2Y0A21YK atau sejenisnya
   const int IR_PIN = 36;
   
   void setup() {
     Serial.begin(115200);
     Serial.println("IR Distance Sensor Example");
   }
   
   void loop() {
     // Baca nilai ADC
     int rawValue = analogRead(IR_PIN);
     
     // Convert the analog voltage to distance (cm)
     // Formula ini spesifik untuk sensor tertentu, sesuaikan dengan datasheet
     float volts = rawValue * (3.3 / 4095.0);
     float distance = 29.988 * pow(volts, -1.173); // Contoh formula, sesuaikan
     
     Serial.print("Raw ADC: ");
     Serial.print(rawValue);
     Serial.print(", Voltage: ");
     Serial.print(volts, 2);
     Serial.print("V, Distance: ");
     Serial.print(distance, 1);
     Serial.println(" cm");
     
     delay(500);
   }
   ```

### 💻 Latihan Praktikum: Sistem Monitoring Lingkungan

#### Tujuan:
- Mengimplementasikan pembacaan multiple sensor analog
- Mengaplikasikan teknik filtering data
- Membuat sistem responsif terhadap perubahan kondisi lingkungan

#### Latihan: Environmental Monitor

```cpp
// Environmental Monitoring System
const int TEMP_PIN = 36;     // Pin untuk sensor suhu (thermistor)
const int LIGHT_PIN = 39;    // Pin untuk sensor cahaya (LDR)
const int MOISTURE_PIN = 34; // Pin untuk sensor kelembaban tanah
const int LED_PIN = 2;       // Onboard LED

// Konfigurasi thermistor (sesuaikan dengan komponen Anda)
const float BETA = 3950;
const float R0 = 10000;
const float T0 = 298.15;
const float SERIES_RESISTOR = 10000;

// Threshold values
const int LIGHT_THRESHOLD_LOW = 30;    // Persentase
const int MOISTURE_THRESHOLD_LOW = 30; // Persentase
const float TEMP_THRESHOLD_HIGH = 30.0; // Celsius

// Status alerts
bool tempAlert = false;
bool lightAlert = false;
bool moistureAlert = false;

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("Environmental Monitoring System");
  Serial.println("--------------------------------");
}

void loop() {
  // 1. Baca dan proses data sensor suhu
  float temperature = readTemperature();
  
  // 2. Baca dan proses data sensor cahaya
  int lightPercent = readLightLevel();
  
  // 3. Baca dan proses data sensor kelembaban
  int moisturePercent = readMoistureLevel();
  
  // 4. Update status alerts
  tempAlert = (temperature > TEMP_THRESHOLD_HIGH);
  lightAlert = (lightPercent < LIGHT_THRESHOLD_LOW);
  moistureAlert = (moisturePercent < MOISTURE_THRESHOLD_LOW);
  
  // 5. Tampilkan data dan status
  displayData(temperature, lightPercent, moisturePercent);
  
  // 6. Indikasi visual jika ada alert (LED berkedip)
  if (tempAlert || lightAlert || moistureAlert) {
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
    delay(100);
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
  }
  
  delay(2000);  // Interval pembacaan sensor
}

// Fungsi untuk membaca dan mengkonversi data suhu
float readTemperature() {
  // Implementasikan pembacaan thermistor
  // Dengan filtering jika perlu
  int rawADC = analogRead(TEMP_PIN);
  
  // Konversi ADC ke resistansi
  float resistance = SERIES_RESISTOR / ((4095.0 / rawADC) - 1.0);
  
  // Konversi resistansi ke suhu (Kelvin) dengan formula Beta
  float steinhart = resistance / R0;
  steinhart = log(steinhart);
  steinhart /= BETA;
  steinhart += 1.0 / T0;
  steinhart = 1.0 / steinhart;
  
  // Konversi Kelvin ke Celsius
  float tempC = steinhart - 273.15;
  
  return tempC;
}

// Fungsi untuk membaca level cahaya sebagai persentase
int readLightLevel() {
  // Implementasikan pembacaan LDR
  // Dengan filtering jika perlu
  int rawValue = analogRead(LIGHT_PIN);
  
  // Map nilai ADC (0-4095) ke persentase (0-100%)
  int lightPercent = map(rawValue, 0, 4095, 100, 0); // Invert so 0 = dark, 100 = bright
  
  return lightPercent;
}

// Fungsi untuk membaca kelembaban tanah sebagai persentase
int readMoistureLevel() {
  // Implementasikan pembacaan moisture sensor
  // Dengan filtering jika perlu
  int rawValue = analogRead(MOISTURE_PIN);
  
  // Sesuaikan range berdasarkan kalibrasi sensor Anda
  // Misalnya, nilai terendah (kering) = 3500, nilai tertinggi (basah) = 1500
  int moisturePercent = map(rawValue, 3500, 1500, 0, 100);
  
  // Constrain untuk memastikan output dalam range 0-100%
  moisturePercent = constrain(moisturePercent, 0, 100);
  
  return moisturePercent;
}

// Fungsi untuk menampilkan data dan status
void displayData(float temp, int light, int moisture) {
  Serial.println("--------------------------");
  Serial.print("Temperature: ");
  Serial.print(temp, 1);
  Serial.print("°C ");
  Serial.println(tempAlert ? "[ALERT: HOT]" : "");
  
  Serial.print("Light Level: ");
  Serial.print(light);
  Serial.print("% ");
  Serial.println(lightAlert ? "[ALERT: DARK]" : "");
  
  Serial.print("Soil Moisture: ");
  Serial.print(moisture);
  Serial.print("% ");
  Serial.println(moistureAlert ? "[ALERT: DRY]" : "");
  Serial.println("--------------------------");
}
```

---

## 📝 Tugas Minggu Ini

1. **Implementasi State Machine**: Buat state machine dengan minimal 4 state yang melibatkan input pengguna (button) dan output visual (LED) dengan transisi yang jelas.

2. **LED Dimmer dengan PWM**: Implementasikan dimmer LED dengan kontrol PWM menggunakan potentiometer dan juga dengan button untuk secara bertahap mengubah kecerahan.

3. **Praktik Sensor Analog**: Pilihlah satu jenis sensor analog (suhu, cahaya, kelembaban, jarak, dll) dan buat sistem yang responsif terhadap perubahan kondisi yang dideteksi.

4. **Tantangan Integrasi**: Integrasikan state machine, PWM, dan pembacaan sensor dalam satu proyek yang terkoordinasi, misalnya "Lampu Otomatis Berbasis Kondisi" yang menyesuaikan kecerahan dan mode berdasarkan input sensor.

## 📚 Referensi dan Bacaan Lebih Lanjut

- Espressif Systems. (2022). *ESP32-S3 Technical Reference Manual* [Online]. Tersedia: https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf

- Purdum, J. (2022). *Beginning C for Arduino: Learn C Programming for the Arduino and Compatible Microcontrollers* (3rd ed.). Apress. doi:10.1007/978-1-4842-7748-0

- Horowitz, P., & Hill, W. (2015). *The Art of Electronics* (3rd ed.). Cambridge University Press.

- Boxall, J. (2022). *Arduino Workshop: A Hands-On Introduction with 65 Projects* (2nd ed.). No Starch Press.

- Hughes, J. M. (2021). *Practical Electronics for Inventors* (4th ed.). McGraw-Hill Education.

- Margolis, M. (2023). *Arduino Cookbook: Recipes to Begin, Expand, and Enhance Your Projects* (3rd ed.). O'Reilly Media.

- Random Nerd Tutorials. (2023). *ESP32 PWM with Arduino IDE* [Online]. Tersedia: https://randomnerdtutorials.com/esp32-pwm-arduino-ide/

- Last Minute Engineers. (2023). *Interface Analog Sensors with ESP32* [Online]. Tersedia: https://lastminuteengineers.com/esp32-adc-arduino-tutorial/

---

<div align="center">
  
  ![QR Code](https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=https://github.com/yourusername/esp32-course)
  
  Scan untuk mengakses repositori lengkap
  
  *© 2025 Mata Kuliah Microcontroller*
</div>
