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

  3. Download kode dari source code sesuai project.
  4. Kunjungi website Cayenne kemudian register atau login.
  5. Tekan Add New > Device/Widget dan pilih Bring Your Own Thing pada bagian kiri.

 <img src="https://user-images.githubusercontent.com/49542850/210127143-f94a8192-7e57-48d2-85f1-ac51fb300468.png" width=600px>

  6. Install library:
     - Buka Arduino IDE
     - Buka Sketch > Include Library > Manage Libraries.
     - Cari library CayenneMQTT by myDevices kemudian install.
     - Atau dapat melalui link CayenneMQTT dan upload pada libraries di direktori projek. Rename direktori menjadi CayenneMQTT.
  7. Pilih perangkat yang sudah ditambahkan, kemudian rename board untuk mempermudah. Copy kredensial yang diberikan dan isikan dalam kode pada bagian yang sudah disediakan. Kemudian upload kode.

  <img src="https://user-images.githubusercontent.com/49542850/210127144-d112f437-7a3e-4969-af98-be98eb8568cf.png" width=600px>
 
  8. Setelah ESP terdeteksi, kemudian tekan Add New > Device/Widget dan pilih custom widget tank.

  <img src="https://user-images.githubusercontent.com/49542850/210127145-976c8dda-257f-4d08-b3e8-d7bcd965f6e5.png" width=600px>
  
  9. Masukkan Nama, Data: Tank level, Unit: Analog/Menyesuaikan, Channel: 1 (pada contoh), Min: 0, Max: 100, dan Display. Kemudian simpan.

  <img src="https://user-images.githubusercontent.com/49542850/210127136-a67a45d4-7c5a-4846-b810-5e2fe61bcdd6.png" width=600px>
  
  10. Lakukan hal yang sama untuk LED. Pilih widget tipe button, isikan Nama, Data: Digital Acuator, Unit: Digital, Channel: 0 (pada contoh), dan icon. Kemudian simpan.
  
  <img src="https://user-images.githubusercontent.com/49542850/210127139-b8c872b4-6f11-4bb1-80a1-afb11564d474.png" width=600px>

  11. Jalankan kode.
  12. LED dapat dikontrol dengan menekan icon widget LED.

  <img src="https://user-images.githubusercontent.com/49542850/210127140-b82c9394-7435-4974-85ea-009bb44f0c0f.png" width=600px>

ESP membaca data analog dari sensor water level. Pada contoh, nilai yang didapat adalah 0 hingga 1900. Nilai ini dapat bervariasi tergantung jenis air, sensor, dan keakuratan (kualitas). Nilai tersebut diubah menjadi bentuk persentase agar lebih mudah dalam pembacaan dan kemudian diupload pada Cayenne melalui protokol MQTT. Konfigurasi channel dan value antara kode dan Cayenne harus sinkron agar data dapat dibaca. Respon dari web Cayenne memiliki keterlambatan sekitar 3-5 detik, sehingga data tidak real-time. Proses upload/publish dilakukan pada fungsi CAYENNE_OUT(channel){} dengan perintah Cayenne.virtualWrite(channel, data, [type], [unit]).

Sedangkan untuk LED membutuhkan download data menggunakan fungsi CAYENNE_IN(channel){}. Pada fungsi tersebut digunakan perintah menyalakan LED dengan value berupa placeholder cayenne, yaitu digitalWrite(ledPin, !getValue.asInt()). Value tersebut didapatkan berdasarkan konfigurasi widget yaitu data digital 0/1.

### Keluaran

  <img src="https://user-images.githubusercontent.com/118702169/210160999-4c19a4b2-4912-43e2-b3c6-82c6463e0a6e.mp4" width=600px>
  
## Percobaan B. Komunikasi ESP-Sensor-LED dan Google Assistant dengan Adafruit.io
### Rangkaian & Instalasi
  1. Siapkan ESP32 dan hubungkan ke Arduino IDE. Siapkan sensor DHT (atau lainnya).
  2. Buat rangkaian berikut.
  
  <img src="https://user-images.githubusercontent.com/49542850/210141122-223f46bc-4ba4-4db5-b2c2-070a3fbec5b5.png" width=600px>

  3. Download kode dari source code sesuai project.
  4. Kunjungi website Adafruit kemudian register atau login.
  5. Pada navbar paling atas, pilih IO.
     - Pada bagian ini kita akan menggunakan Dashboard dan Feeds.
     - Dashboard adalah papan kerja atau ruang kontrol terhadap Feeds (perangkat) IoT.
     - Feeds adalah suatu grup sebagai identitas suatu atau kumpulan perangkat.
     - Feeds dapat berisi lebih dari satu perangkat. Jika kita mengubah nilai dari suatu feeds, maka seluruh perangkat yang ada pada feeds akan terpengaruh.
  
  <img src="https://user-images.githubusercontent.com/49542850/210132150-be787f19-c2ab-4ad3-b603-4e68a1f68681.png" width=600px>

  6. Tekan Dashboard dan pilih New Dashboard.
  
  <img src="https://user-images.githubusercontent.com/49542850/210132151-c1c28a93-bd85-4529-9fd7-f7369370d8f0.png" width=600px>

  7. Klik gerigi opsi dan pilih Create new block.

  <img src="https://user-images.githubusercontent.com/49542850/210132154-c00fcc31-d4bb-471f-b386-024671ae85b9.png" width=600px>

  <img src="https://user-images.githubusercontent.com/49542850/210132155-5c354b24-e9c4-4249-ba57-538a17a67f99.png" width=600px>
  
  8. Pilih Toggle dan buat feeds baru lalu centang.

  <img src="https://user-images.githubusercontent.com/49542850/210132157-ebc4e1bc-fab1-4f7a-b689-2ab988e7e55b.png" width=600px>
  
  9. Beri nama block tersebut "LED". Isikan ON value dengan 1 dan OFF value dengan 0.

  <img src="https://user-images.githubusercontent.com/49542850/210132158-2643467f-9974-4077-b178-8a76aceb3e0b.png" width=600px>
  
  10. Buat 2 block baru dengan tipe gauge, line gauge, atau sejenisnya dan buat feeds baru untuk suhu dan kelembaban.
  11. Isikan Min-Max value dan komposisi warna sebagai tanda, yang dirasa sesuai (perkiraan). (Suhu 0-50, Humidity 0-100).

  <img src="https://user-images.githubusercontent.com/49542850/210132159-a9c51b9c-4068-4493-ac76-ea9551f98c03.png" width=600px>
  
  12. Jika sudah selesai, pada dashboard, tekan gambar kunci untuk mendapat API key. Copy key dan masukkan ke dalam kode.

  <img src="https://user-images.githubusercontent.com/49542850/210132160-76c129d1-144c-4f88-a81b-ab6e177bea21.png" width=600px>
  
  13. Jalankan kode. LED dapat dikontrol melalui toggle button yang sudah dibuat dan informasi suhu dan kelembaban akan terbaca.

  <img src="https://user-images.githubusercontent.com/49542850/210132161-918a6079-ec1d-4462-ae1a-fcae2fd3d04d.png" width=600px>
  
### Integrasi IFTTT Goggle Assistant v2
  1. Buka website IFTTT dan register atau login dengan akun yang sudah ada. Pastikan akun sudah terverifikasi email.
  2. Download aplikasi Google Home pada perangkat android. Login dengan email.
  3. Tambahkan rumah atau gunakan rumah yang sudah ada.
  4. Pilih Setting (gambar gerigi pada home) > Works with google dan cari IFTTT. Jika sudah, abaikan peringatan no device compatible karena kita belum menambahkan trigger pada IFTTT.

  <img src="https://user-images.githubusercontent.com/49542850/210139114-d3ad40c5-db53-4f0f-b729-1566e858b665.png" width=600px>
  
  5. Kembali ke website IFTTT. Pada halaman utama, tekan Create.

  <img src="https://user-images.githubusercontent.com/49542850/210132148-4a129b7a-08a2-462d-8a90-ce9400b9d566.png" width=600px>

  6. Halaman ini menunjukkan perkondisian. Kita akan memasukkan google assistant pada bagian if dan adafruit pada bagian then. IF google assistant (trigger) then adafruit (execute).

  <img src="https://user-images.githubusercontent.com/49542850/210139120-7a30585a-3e57-4798-9918-1fffae175ccc.png" width=600px>
  
  7. Pilih bagian IF dan tambahkan google assistant.

  <img src="https://user-images.githubusercontent.com/49542850/210139121-deac9f87-bd3e-400a-96e3-84d1cc8e35d0.png" width=600px>
  
  8. Pilih Activate Scene dan masukkan kata kunci untuk trigger, contoh "turn on LED". Pastikan google home sudah terintregasi dengan IFTTT, jika tidak maka akan diminta untuk melakukan integrasi (connect).

  <img src="https://user-images.githubusercontent.com/49542850/210139122-93dfd336-a9ea-433a-9583-4da18948c248.png" width=600px>
  
  <img src="https://user-images.githubusercontent.com/49542850/210139123-9cac3047-9e8f-4709-8523-9f3c93a44a39.png" width=600px>
  
  9. Selanjutnya, pada bagian THEN THAT, tambahkan service adafruit.

  <img src="https://user-images.githubusercontent.com/49542850/210139124-17f35a8c-7860-4da6-8f90-ac709855528d.png" width=600px>
  
  10. Pilih Send data to adafruit dan lakukan integrasi (connect).

  <img src="https://user-images.githubusercontent.com/49542850/210139107-b9fa0bbe-ca33-4124-9eb0-0fae3f1c9478.png" width=600px>
  
  11. Isikan feed yang sudah dibuat, dalam hal ini adalah LED karena kita akan mengontrol LED melalui google assistant. Pilih data dengan nilai yang akan kita kirim. Pada contoh adalah "1" yang artinya akan memberikan trigger ON pada feed LED. Pastikan nilai tersebut sesuai dengan nilai data yang kita berikan saat membuat block di dashboard.

  <img src="https://user-images.githubusercontent.com/49542850/210139109-665aeaae-f49a-4576-90ac-2034c98fa580.png" width=600px>
  
  12. Setelah selesai, tekan Continue dan Finish.

  <img src="https://user-images.githubusercontent.com/49542850/210139110-b97c68aa-0463-4db1-bca4-cc8f98da4646.png" width=600px>
  
  13. Ulangi langkah Create untuk membuat trigger OFF.

  <img src="https://user-images.githubusercontent.com/49542850/210139111-a72df196-a113-479c-be7d-44a2ff052084.png" width=600px>
  
  14. Buka google home pada smartphone. Tekan profile > Assistant settings > Routines > New, kemudian tambahkan starter voice. Isikan trigger (voice) kemudian simpan.

  <img src="https://user-images.githubusercontent.com/49542850/210139117-048121b3-e4a2-4a16-98b9-9629078ac1ec.png" width=600px>
  
  15. Pilih actions > Adjust home devices > Add scenes > pilih actions yang telah dibuat di IFTTT.

  <img src="https://user-images.githubusercontent.com/49542850/210139119-951dcef8-7442-4bb9-92ee-dfc4a3a9f6f0.png" width=600px>
  
  16. Coba buka google assistant pada smartphone dan katakan kata trigger. Misal "Hey google, turn on LED". Jika LED dapat menyala maka google assistant sudah terintegrasi dan dapat digunakan dimanapun selama kedua device (ESP dan smartphone) terhubung dengan internet.
  
Prinsip kerja dari percobaan ini sama dengan percobaan sebelumnya (A). Adafruit.io mirip seperti Cayenne. Adafruit akan membaca data yang diterima ESP melalui sensor DHT berupa suhu dan kelembaban. Data tersebut dipublish dengan perintah Adafruit_MQTT_Publish melalui feed sehingga akan terbaca pada block gauge yang ada di dashboard. Sedangkan untuk LED dikontrol melalui dashboard adafruit. ESP board akan melakukan subscribe dengan perintah Adafruit_MQTT_Subscribe terhadap feed LED dan menerima data berupa 0 atau 1 (sesuai data yang diset saat ON/OFF pada toggle block).

Platform IFTTT mengijinkan otomasi seperti adafruit agar dapat terhubung dengan layanan google assistant. IFTTT menyediakan 2 applets (trigger perkondisian) yang masing-masing hanya dapat diisi perkondisian tunggal if-then. Dari applets ini masing-masing dibuat trigger untuk menyalakan dan mematikan LED. Platform IFTTT harus terintregasi dengan google home pada smartphone agar dapat digunakan. Jika sudah terintegrasi, maka ketika user menggunakan google assistant dengan trigger yang sudah dibuat, IFTTT akan memberikan sinyal ke feed adafruit. Jika nilai yang diberikan terbaca, hasilnya akan terlihat pada dashboard berupa toggle LED. Maka LED yang ada pada ESP akan otomatis berubah kondisi. Selama ESP dan smartphone terhubung dengan jaringan internet, maka kedua device dapat saling berkomunikasi jarak jauh melalui google assistant untuk mengontrol LED, serta memonitor suhu melalui website adafruit.io.

### Keluaran

  <img src="https://user-images.githubusercontent.com/118702169/210161208-107a5fc8-cd25-46bd-81da-bd3c12a9e04b.mp4" width=600px>
  

## Project C - Komunikasi ESP-Sensor dengan Thingspeak
### Rangkaian & Instalasi
  1. Siapkan ESP32 dan hubungkan ke Arduino IDE. Siapkan sensor DHT (atau lainnya).
  2. Buat rangkaian berikut.

  <img src="https://user-images.githubusercontent.com/49542850/210139395-95c4d10c-3d9e-4898-ba3d-0165420ff2dc.png" width=600px>
  
  3. Download kode dari source code sesuai project.
  4. Kunjungi website Thingspeak kemudian register atau login.
  5. Buat channel baru dengan New channel.

  <img src="https://user-images.githubusercontent.com/49542850/210140673-89fdda9c-7887-4ed3-b037-472364b51599.png" width=600px>
  
  6. Isikan nama dan centang 2 field untuk suhu dan kelembaban (menyesuaikan yang akan ditampilkan).

  <img src="https://user-images.githubusercontent.com/49542850/210140674-1cbc71f0-bbba-4f62-9b29-fb88cb0b38cf.png" width=600px>
  
  7. Secara default akan ada 2 chart atau grafik yang menampilkan nilai field 1 dan 2. Namun bisa juga ditambahkan widget untuk memperbagus tampilan dengan Add widget, lalu atur delay (disarankan >2s) dan informasi lainnya.

  <img src="https://user-images.githubusercontent.com/49542850/210140676-ce81a14f-13fe-46be-8cf7-71975ccd4c95.png" width=600px>
  
  8. Masuk ke menu api keys dan copy write key, masukkan ke dalam kode.

  <img src="https://user-images.githubusercontent.com/49542850/210140677-0c343ab0-2fb9-408e-adbc-b2020aa55aa7.png" width=600px>
  
  9. Masuk ke kode, ubah bagian unsigned long myChannelNumber = 1; dengan urutan channel Anda. Jika pertama, maka tulis 1, dst.
  10. Cari kode bagian ThingSpeak.setField() untuk menyesuaikan field dengan format ThingSpeak.setField(field, value).
  11. Jalankan kode dan pastikan data terbaca pada chart thingspeak.

### Keluaran

  <img src="https://user-images.githubusercontent.com/49542850/210140799-ac4b5da1-6065-4c54-89ba-ed26331e8e8c.png" width=600px>

## Percobaan D. Komunikasi Multipoint ESP-Sensor to Point dengan Cayenne
### Rangkaian & Instalasi
  1. Siapkan ESP32 dan hubungkan ke Arduino IDE. Siapkan sensor DHT (atau lainnya).
  2. Buat skema berikut.

  <img src="https://user-images.githubusercontent.com/49542850/210164985-5d752e07-237d-4214-b371-6880f32a0cd3.png" width=600px>
  
### Pengirim
  1. Download kode dari source code sesuai project.
  2. Buat rangkaian berikut.

  <img src="https://user-images.githubusercontent.com/49542850/210139395-95c4d10c-3d9e-4898-ba3d-0165420ff2dc.png" width=600px>
  
  3. Ganti mac address pada kode broadcastAddress[] = {0xff, 0xff, 0xff, 0xff, 0xff, 0xff} dengan mac address slave (koordinator).
  4. Ubah espNumber dengan angka unik 1 sampai 3 sebagai id. Antar pengirim harus memiliki id berbeda.
  5. Jalankan kode. Beberapa data yang dikirim mungkin akan fail karena terjadi jeda saat ESP penerima melakukan publish ke Cayenne.

### Penerima
  1. Download kode dari source code sesuai project. Pastikan library Cayenne sudah terinstal.
  2. Hubungkan ESP dengan Arduino IDE.
  3. Kunjungi website Cayenne kemudian register atau login.
  4. Tekan Add New > Device/Widget dan pilih Bring Your Own Thing pada bagian kiri.

  <img src="https://user-images.githubusercontent.com/49542850/210127143-f94a8192-7e57-48d2-85f1-ac51fb300468.png" width=600px>
  
  5. Pilih perangkat yang sudah ditambahkan, kemudian rename board untuk mempermudah. Copy kredensial yang diberikan dan isikan dalam kode pada bagian yang sudah disediakan. Kemudian upload kode. Pastikan ssid dan password diisi dengan hotspot/AP yang terhubung dengan internet.

  <img src="https://user-images.githubusercontent.com/49542850/210127144-d112f437-7a3e-4969-af98-be98eb8568cf.png" width=600px>
  
  6. Setelah ESP terdeteksi, kemudian tekan Add New > Device/Widget dan pilih custom value.

  <img src="https://user-images.githubusercontent.com/49542850/210127145-976c8dda-257f-4d08-b3e8-d7bcd965f6e5.png" width=600px>
  
  7. Masukkan Nama, Data: Temperature, Unit: Celcius, Channel: 1 (pada contoh), Display. Kemudian simpan. Atau klik tombol + pada widget yang akan otomatis dibuat ketika kode diupload.

  <img src="https://user-images.githubusercontent.com/49542850/210165004-60f57c80-eae8-45f4-824c-75fe34515efb.png" width=600px>
  
  8. Lakukan hal yang sama untuk setiap informasi yang akan ditampilkan Cayenne. Pada contoh digunakan 4 channel.
  9. Jalankan kode. Kode dapat digunakan untuk menerima hingga 3 ESP berbeda dengan struktur data yang sama.
  
### Keluaran

   <img src="https://user-images.githubusercontent.com/49542850/210165345-acf2ac33-956f-49f4-a5cb-c0dae72cace2.png" width=600px>
   
   <img src="https://user-images.githubusercontent.com/118702169/210168226-30885f94-3d9e-4a7d-830c-427b088942a9.mp4" width=600px>
   
## Kesimpulan
  - MQTT merupakan protokol yang memungkinkan ESP untuk dapat berkomunikasi dengan sistem yang terdapat pada cloud seperti Cayenne, Adafruit IO, dan ThingSpeak.
  - Cayenne, Adafruit IO, dan ThingSpeak merupakan beberapa platform untuk memonitoring perangkat IoT secara cloud melalui web service. Beberapa platform menyediakan fitur beragam mulai dari monitoring, controlling, hingga autonomous action.
  - Cayenne memiliki fitur utama yaitu monitoring dan controlling. Cayenne mampu membaca data yang dikirimkan ESP melalui channel-channel yang dibuat pada Cayenne untuk membedakan data serta memberikan input kepada ESP (komunikasi 2 arah). Cayenne juga mampu melakukan autonomous action seperti jika salah satu data memenuhi syarat, maka cayenne dapat mengubah data2 atau memberikan feedback dan seterusnya.
  - Adafruit memiliki fitur yang lebih langkap dengan klasifikasi setiap perangkat kedalam feeds. Sehingga setiap kali data dari ESP dikirim, data diteruskan pada group (feeds) yang ada. Adafruit juga memiliki fitur control seperti toggle dan dapat terintegrasi dengan platform autonomous seperti IFTTT.
  - IFTTT merupakan salah satu platform/protokol otomasi yang terhubung dengan berbagai services. Salah satunya adalah google assistant yang memungkinkan kontrol melalui smartphone. IFTTT menghubungkan service seperti google assistant dengan adafruit agar dapat mengontrol dashboard pada adafruit.
  - ThingSpeak merupakan salah satu platform IoT yang sangat dasar. Platform ini hanya memiliki fitur monitoring (sejauh yang saya pahami) dan tidak selengkap Adafruit IO maupun Cayenne. Untuk pengiriman data pada ThingSpeak dibedakan dengan suatu field untuk masing-masing data.
  - Protokol ESPNOW memungkinkan ESP dapat berkomunikasi satu sama lain melalui sebuah kanal jaringan (WiFi). Akan tetapi, ESPNOW kurang cocok digunakan bersamaan dengan MQTT atau WiFi mode station. Karena saat MQTT melakukan komunikasi dengan server cloud, ESPNOW akan terhenti dan data yang dikirim akan gagal. Akan tetapi setelah proses upload selesai, ESPNOW dapat berkomunikasi kembali.
   



