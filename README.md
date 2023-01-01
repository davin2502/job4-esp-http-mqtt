Davin Marselon/4.31.20.1.07/TE-3B

# Jobsheet 4 Komunikasi HTTP dan MQTT
Pada jobsheet 4 terdapat 4 project yaitu komunikasi sensor dan feedback menggunakan plaform cayenne, adafruit.io, thingspeak, serta komunikasi terpusat menggunakan ESP-NOW dan cayenne.

## Alat dan Bahan
  - ESP32
  - Arduino IDE (Terinstal ESP32)
  - Library DHT dan Adafruit unified sensor
  - Breadboard
  - Kabel Jumper
  - Resistor < 220Ω
  - LED
  - Sensor DHT
  - Sensor Water Level

## Instalasi ESP-32
  1. Buka Arduino IDE
     - Masuk ke Preferences
     - Isikan board url dengan link : https://dl.espressif.com/dl/package_esp32_index.json dan simpan
     - Buka Tools > Board > Boards Manager
     - Cari ESP32, by Espressif. Kemudian instal
     - Pilih flash mode DIO/QIU menyesuaikan
  2. Jalankan program .ino
  3. Jika terdapat error saat uploading, tekan dan tahan tombol Boot pada ESP32 saat upload, hingga Connecting selesai

## Instalasi DHT & Adafruit Libraries
  1. Buka Arduino IDE
  2. Buka Sketch > Include Library > Library Manager
  3. Cari DHT sensor library by Adafruit, kemudian instal. Atau dapat melalui link DHT Library dan upload pada libraries di direktori projek. Rename direktori menjadi DHT_sensor_library.
  4. Instal juga library Adafruit unified sensor by Adafruit. Atau dapat melalui link Adafruit Sensor dan upload pada libraries di direktori projek. Rename direktori menjadi Adafruit_Unified_Sensor.
  
## Percobaan A. Komunikasi ESP-Sensor-LED dengan Cayenne
### Rangkaian & Instalasi
  1. Siapkan ESP32 dan hubungkan ke Arduino IDE. Siapkan sensor water level (atau dht).
  2. Buat rangkaian berikut.
  
  <img src="https://user-images.githubusercontent.com/49542850/210127141-16c8ccac-03bc-4d1c-b08f-6b6acf302fbf.png" width=600px>


Download kode dari source code sesuai project.
Kunjungi website Cayenne kemudian register atau login.
Tekan Add New > Device/Widget dan pilih Bring Your Own Thing pada bagian kiri.


Install library:
Buka Arduino IDE
Buka Sketch > Include Library > Manage Libraries.
Cari library CayenneMQTT by myDevices kemudian install.
Atau dapat melalui link CayenneMQTT dan upload pada libraries di direktori projek. Rename direktori menjadi CayenneMQTT.
Pilih perangkat yang sudah ditambahkan, kemudian rename board untuk mempermudah. Copy kredensial yang diberikan dan isikan dalam kode pada bagian yang sudah disediakan. Kemudian upload kode.


Setelah ESP terdeteksi, kemudian tekan Add New > Device/Widget dan pilih custom widget tank.


Masukkan Nama, Data: Tank level, Unit: Analog/Menyesuaikan, Channel: 1 (pada contoh), Min: 0, Max: 100, dan Display. Kemudian simpan.


Lakukan hal yang sama untuk LED. Pilih widget tipe button, isikan Nama, Data: Digital Acuator, Unit: Digital, Channel: 0 (pada contoh), dan icon. Kemudian simpan.


Jalankan kode.
LED dapat dikontrol dengan menekan icon widget LED.


Nb. Untuk versi sensor DHT dapat mendownload kode untuk DHT dan konfigurasi Cayenne serupa. Widget type value, data temperature dan humidity.

Penjelasan
ESP membaca data analog dari sensor water level. Pada contoh, nilai yang didapat adalah 0 hingga 1900. Nilai ini dapat bervariasi tergantung jenis air, sensor, dan keakuratan (kualitas). Nilai tersebut diubah menjadi bentuk persentase agar lebih mudah dalam pembacaan dan kemudian diupload pada Cayenne melalui protokol MQTT. Konfigurasi channel dan value antara kode dan Cayenne harus sinkron agar data dapat dibaca. Respon dari web Cayenne memiliki keterlambatan sekitar 3-5 detik, sehingga data tidak real-time. Proses upload/publish dilakukan pada fungsi CAYENNE_OUT(channel){} dengan perintah Cayenne.virtualWrite(channel, data, [type], [unit]).

Sedangkan untuk LED membutuhkan download data menggunakan fungsi CAYENNE_IN(channel){}. Pada fungsi tersebut digunakan perintah menyalakan LED dengan value berupa placeholder cayenne, yaitu digitalWrite(ledPin, !getValue.asInt()). Value tersebut didapatkan berdasarkan konfigurasi widget yaitu data digital 0/1.

Keluaran
