# SBM_smart-plant-monitoring-system
plorian peros 2306030071
M rifky ramadhan 2306030042

# Monitoring kelembapan tanah dan intensitas cahaya tanaman

## Latar Belakang

Tanaman memerlukan kondisi lingkungan yang optimal agar dapat tumbuh dan berkembang dengan baik. Dua faktor penting yang sangat memengaruhi pertumbuhan tanaman adalah kelembapan tanah dan intensitas cahaya. Kelembapan tanah yang tidak sesuai dapat menyebabkan tanaman mengalami kekeringan atau pembusukan akar, sedangkan intensitas cahaya yang kurang atau berlebihan dapat menghambat proses fotosintesis.

Dalam praktik pertanian dan perawatan tanaman secara konvensional, pemantauan kondisi tanah dan cahaya masih banyak dilakukan secara manual dan berdasarkan perkiraan. Cara ini kurang efisien, memerlukan waktu dan tenaga, serta berpotensi menimbulkan kesalahan dalam pengambilan keputusan perawatan tanaman. Akibatnya, produktivitas tanaman menjadi tidak optimal dan penggunaan sumber daya seperti air menjadi kurang efisien.

Seiring dengan perkembangan teknologi, khususnya di bidang mikrokontroler dan Internet of Things (IoT), pemantauan kondisi lingkungan tanaman dapat dilakukan secara otomatis dan real-time. Dengan memanfaatkan sensor kelembapan tanah dan sensor intensitas cahaya, data kondisi tanaman dapat diperoleh secara akurat dan berkelanjutan. Informasi tersebut dapat digunakan sebagai dasar pengambilan keputusan dalam penyiraman dan pengaturan pencahayaan tanaman.

Oleh karena itu, diperlukan sebuah sistem Monitoring Kelembapan Tanah dan Intensitas Cahaya Tanaman yang mampu membantu pengguna dalam memantau kondisi lingkungan tanaman secara efektif dan efisien. Sistem ini diharapkan dapat meningkatkan kualitas pertumbuhan tanaman, menghemat penggunaan air, serta mendukung penerapan konsep pertanian cerdas (smart farming).

## Rumusan Masalah

Berdasarkan latar belakang yang telah diuraikan, maka rumusan masalah dalam projek ini adalah:
Bagaimana cara memantau kelembapan tanah tanaman secara akurat dan real-time?
Bagaimana cara mengukur intensitas cahaya yang diterima tanaman secara efektif?
Bagaimana merancang sistem monitoring yang dapat menampilkan data kelembapan tanah dan intensitas cahaya secara mudah dipahami?
Bagaimana sistem monitoring dapat membantu pengambilan keputusan dalam perawatan tanaman?
Bagaimana meningkatkan efisiensi penggunaan air dan kualitas pertumbuhan tanaman dengan bantuan sistem monitoring?

## Tujuan Proyek

Adapun tujuan dari projek Monitoring Kelembapan Tanah dan Intensitas Cahaya Tanaman adalah:
Merancang dan membangun sistem untuk memonitor kelembapan tanah tanaman.
Mengembangkan sistem pengukuran intensitas cahaya yang diterima tanaman.
Menyediakan informasi kondisi tanah dan cahaya secara real-time.
Membantu pengguna dalam menentukan waktu penyiraman dan perawatan tanaman.
Meningkatkan efisiensi penggunaan air serta mendukung pertumbuhan tanaman yang optimal.
Menerapkan teknologi mikrokontroler dan sensor sebagai solusi pertanian cerdas (smart farming).

## Alat

Laptop / Komputer
Digunakan untuk pemrograman mikrokontroler dan pengujian sistem.

Kabel USB
Untuk menghubungkan mikrokontroler dengan laptop.

Breadboard
Media perakitan rangkaian elektronik.

Obeng Kecil
Untuk membantu pemasangan komponen.

Tang Potong / Tang Lancip
Digunakan saat perakitan rangkaian.

Multimeter (opsional)
Untuk pengecekan tegangan dan koneksi rangkaian.

## Bahan

Mikrokontroler (Arduino Uno / ESP32)
Sebagai pusat pengendali dan pemrosesan data sensor.

Sensor Kelembapan Tanah (Soil Moisture Sensor)
Digunakan untuk mengukur tingkat kelembapan tanah tanaman.

Sensor Intensitas Cahaya (LDR atau BH1750)
Digunakan untuk mengukur intensitas cahaya yang diterima tanaman.

Resistor (±10 kΩ)
Digunakan pada rangkaian sensor LDR.

LCD 16x2 atau OLED Display (opsional)
Untuk menampilkan data hasil pengukuran.

Kabel Jumper (Male–Male / Male–Female)
Untuk menghubungkan komponen satu dengan lainnya.

Modul Relay (opsional)
Digunakan untuk mengontrol perangkat eksternal seperti pompa air.

Pompa Air DC (opsional)
Digunakan untuk penyiraman otomatis tanaman.

Adaptor / Catu Daya
Sebagai sumber tegangan untuk sistem.

## pengujian sensor

```
// ==========================
// Pin ESP32
// ==========================
const int moisturePin = 35;   // D35 - sensor kelembapan tanah
const int lightPin    = 32;   // D32 - sensor cahaya
const int relayPin    = 33;   // D33 - relay pompa
const int ledPin      = 25;   // D25 - LED indikator cahaya

// ==========================
int batasKering = 2000;

// PWM LED
const int pwmFreq = 5000;
const int pwmResolution = 8;   // 0–255

void setup() {
  Serial.begin(9600);

  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, HIGH); // pompa OFF

  // Setup PWM ESP32 (API baru)
  ledcAttach(ledPin, pwmFreq, pwmResolution);
  ledcWrite(ledPin, 0); // LED mati awal
}

void loop() {

  // ==========================
  // Baca sensor
  // ==========================
  int moistureValue = analogRead(moisturePin);
  int lightValueRaw = analogRead(lightPin);

  // ==========================
  // Cahaya → Persentase LED (0–100%)
  // Gelap → 100%
  // Terang → 0%
  // ==========================
  int ledPercent = map(lightValueRaw, 0, 4095, 100, 0);
  ledPercent = constrain(ledPercent, 0, 100);

  // Konversi % → PWM (0–255)
  int ledPWM = map(ledPercent, 0, 100, 0, 255);

  // Tulis ke LED
  ledcWrite(ledPin, ledPWM);

  // ==========================
  // Kontrol relay kelembapan
  // ==========================
  if (moistureValue >= batasKering) {
    digitalWrite(relayPin, LOW);   // tanah basah → pompa OFF
  } else {
    digitalWrite(relayPin, HIGH);  // tanah kering → pompa ON
  }

  // ==========================
  // Serial Monitor
  // ==========================
  Serial.print("Moisture: ");
  Serial.print(moistureValue);
  Serial.print(" | Cahaya ADC: ");
  Serial.print(lightValueRaw);
  Serial.print(" | LED: ");
  Serial.print(ledPercent);
  Serial.print("% | Pompa: ");
  Serial.println(moistureValue < batasKering ? "ON" : "OFF");

  delay(300);
}
```
> Dari hasil pengujian yang telah dilakukan, sensor kelembapan tanah dan sensor cahaya dapat bekerja dengan baik dan memberikan respon terhadap perubahan kondisi tanah dan cahaya. Sensor siap digunakan dalam proses monitoring tanaman.

## pengujian output (LED,RELAY)

```
const int moisturePin = A0;   // sensor kelembapan tanah
const int lightPin = A1;      // sensor cahaya
const int relayPin = 7;       // relay untuk pompa
const int ledPin = 6;         // LED indikator cahaya (PWM)

int batasKering = 600;  // ambang tanah kering

void setup() {
  Serial.begin(9600);
  pinMode(relayPin, OUTPUT);
  pinMode(ledPin, OUTPUT);

  // relay OFF saat awal (karena active LOW)
  digitalWrite(relayPin, HIGH);
}

void loop() {

  // ==========================
  // Baca sensor
  // ==========================
  int moistureValue = analogRead(moisturePin);
  int lightValueRaw = analogRead(lightPin);

  // ==========================
  // Mapping cahaya ke 0–100%
  // ==========================
  int lightPercent = map(lightValueRaw, 0, 1023, 0, 100);
  lightPercent = constrain(lightPercent, 0, 100);

  // ==========================
  // LED terbalik
  // ==========================
  int ledBrightness = map(lightPercent, 0, 100, 255, 0);
  ledBrightness = constrain(ledBrightness, 0, 255);
  analogWrite(ledPin, ledBrightness);

  // ==========================
  // Kontrol relay kelembapan
  // ==========================
  if (moistureValue >= batasKering) {
    digitalWrite(relayPin, LOW);   // tanah basah → pompa OFF
  } else {
    digitalWrite(relayPin, HIGH);  // tanah kering → pompa ON
  }

  // ==========================
  // Serial Monitor
  // ==========================
  Serial.print("Moisture: ");
  Serial.print(moistureValue);
  Serial.print(" | Cahaya: ");
  Serial.print(lightPercent);
  Serial.print("% | LED PWM: ");
  Serial.println(ledBrightness);

  delay(300);
}. Ubahkan untuk ESP 32. moisture sensor pin D35, light sensor pin D32, dan relay pin D33.

```
> Hasil pengujian menunjukkan bahwa output sistem LED dan relay bekerja sesuai kondisi aktual. Output bekerja normal tanpa kendala.

## pengujian menghubungkan ke aplikasi blynk

```
## #define BLYNK_TEMPLATE_ID "TMPL6YVr2qERf"
#define BLYNK_TEMPLATE_NAME "smart plant monitoring"
#define BLYNK_AUTH_TOKEN "u77TgRTNKTcLkj5bPFpYqiGNwjUcxMsX"
#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// ==========================
// WiFi
// ==========================
char ssid[] = "Ryan D Gracia";
char pass[] = "99999999";

// ==========================
// Pin ESP32
// ==========================
const int moisturePin = 35;   // sensor kelembapan tanah
const int lightPin    = 34;   // sensor cahaya
const int relayPin    = 33;   // relay pompa
const int ledPin      = 25;   // LED indikator cahaya

// ==========================
int batasKering = 2000;

// PWM LED
const int pwmFreq = 5000;
const int pwmResolution = 8;   // 0–255

bool manualRelay = false;   // kontrol manual dari Blynk (V4)

// ==========================
// V4 - Switch Relay dari Blynk
// ==========================
BLYNK_WRITE(V4) {
  int relayCmd = param.asInt(); // 0 / 1
  manualRelay = true;

  if (relayCmd == 1) {
    digitalWrite(relayPin, HIGH); // pompa ON
  } else {
    digitalWrite(relayPin, LOW);  // pompa OFF
  }
}

void setup() {
  Serial.begin(9600);

  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, HIGH); // pompa OFF awal

  // PWM LED ESP32 (API baru)
  ledcAttach(ledPin, pwmFreq, pwmResolution);
  ledcWrite(ledPin, 0);

  // Koneksi Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop() {

  Blynk.run();

  // ==========================
  // Baca sensor
  // ==========================
  int moistureValue = analogRead(moisturePin); // 0–4095
  int lightValueRaw = analogRead(lightPin);    // 0–4095

  // ==========================
  // Sensor → Persen (0–100%)
  // ==========================

  // Kelembapan: kering=0%, basah=100%
  int moisturePercent = map(moistureValue, 4095, 0, 0, 100);
  moisturePercent = constrain(moisturePercent, 0, 100);

  // Cahaya: gelap=0%, terang=100%
  int lightPercent = map(lightValueRaw, 0, 4095, 0, 100);
  lightPercent = constrain(lightPercent, 0, 100);

  // ==========================
  // LED fisik (PWM, terbalik)
  // ==========================
  int ledPercent = map(lightValueRaw, 0, 4095, 100, 0);
  ledPercent = constrain(ledPercent, 0, 100);

  int ledPWM = map(ledPercent, 0, 100, 0, 255);
  ledcWrite(ledPin, ledPWM);

  // ==========================
  // Kontrol relay otomatis
  // ==========================
  if (!manualRelay) {
    if (moistureValue >= batasKering) {
      digitalWrite(relayPin, LOW);   // basah → OFF
    } else {
      digitalWrite(relayPin, HIGH);  // kering → ON
    }
  }

  // ==========================
  // Kirim ke Blynk
  // ==========================
  Blynk.virtualWrite(V0, moisturePercent);        // kelembapan %
  Blynk.virtualWrite(V1, lightPercent);           // cahaya %
  Blynk.virtualWrite(V2, digitalRead(relayPin)); // relay 0/1

  int ledStatus = (ledPercent > 0) ? 1 : 0;
  Blynk.virtualWrite(V3, ledStatus);              // LED 0/1

  // ==========================
  // Serial Monitor
  // ==========================
  Serial.print("Moisture: ");
  Serial.print(moisturePercent);
  Serial.print("% | Cahaya: ");
  Serial.print(lightPercent);
  Serial.print("% | LED: ");
  Serial.print(ledPercent);
  Serial.print("% | Pompa: ");
  Serial.println(digitalRead(relayPin) == HIGH ? "ON" : "OFF");

  delay(300);
}
```
> pada kondisi awal kami selalu gagal menghubungkan hotspot hp ke wifi laptop. tapi kami terus berjuang sampai akhirnya behasil dan bisa mengontrol menggunakan hp.


<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/1c0f9530-eb73-492a-8388-16b41a863e15" />










