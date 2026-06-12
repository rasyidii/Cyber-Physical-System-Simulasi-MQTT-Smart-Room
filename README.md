# MQTT Smart Room Monitoring

Implementasi protokol MQTT menggunakan Python dan Eclipse Mosquitto Broker untuk simulasi sistem **Smart Room Monitoring** pada mata kuliah Cyber Physical System.

## Deskripsi

Proyek ini mensimulasikan komunikasi IoT menggunakan pola **Publish-Subscribe** MQTT. Sistem terdiri dari:

- Publisher (sensor dan aktuator)
- Mosquitto Broker
- Subscriber (monitoring)

Data yang dipertukarkan meliputi:

- Suhu ruangan
- Kelembapan
- Intensitas cahaya
- Status lampu
- Status AC
- Status sistem

Program dibuat dalam satu file (`mqtt_smart_room.py`) yang berisi lima skenario pengujian MQTT.

---

## Arsitektur Sistem

```
Publisher
    |
    v
Mosquitto Broker
    |
    v
Subscriber
```

Komunikasi menggunakan:

- Protocol : MQTT
- Broker : Eclipse Mosquitto
- Port : 1883
- Payload : JSON
- Transport : TCP/IP

---

## Struktur Topik MQTT

```text
smartroom/
├── sensor/
│   ├── temperature
│   ├── humidity
│   └── light
│
├── actuator/
│   ├── lamp
│   └── ac
│
└── system/
    ├── status
    └── alert
```

---

## Requirement

### Software

- Python 3.x
- Eclipse Mosquitto Broker
- Windows 10 / 11 (atau OS lain yang mendukung Mosquitto)

### Library Python

```bash
pip install paho-mqtt
```

---

## Menjalankan Mosquitto Broker

Pastikan Mosquitto sudah berjalan sebelum menjalankan program.

Cek service Mosquitto:

```bash
net start mosquitto
```

Atau jalankan manual:

```bash
mosquitto
```

Verifikasi broker:

```bash
netstat -an | find "1883"
```

---

## Menjalankan Program

```bash
python mqtt_smart_room.py
```

Menu utama akan muncul:

```text
SMART ROOM MONITORING MQTT

1. Skenario 1 – Komunikasi Dasar
2. Skenario 2 – QoS 0 / 1 / 2
3. Skenario 3 – Multi Topik
4. Skenario 4 – Wildcard +
5. Skenario 5 – Wildcard #
0. Keluar
```

Pilih:

- `p` untuk Publisher
- `s` untuk Subscriber

---

## Skenario Pengujian

### Skenario 1 — Komunikasi Dasar

Publisher mengirim data suhu ke:

```text
smartroom/sensor/temperature
```

Subscriber menerima dan menampilkan data secara real-time.

QoS:

```text
QoS 0
```

---

### Skenario 2 — Pengiriman Data dengan QoS

Menguji perbedaan perilaku MQTT:

- QoS 0 (At Most Once)
- QoS 1 (At Least Once)
- QoS 2 (Exactly Once)

Topik:

```text
smartroom/sensor/temperature
```

---

### Skenario 3 — Multi Topik

Publisher mengirim data ke lima topik sekaligus:

```text
smartroom/sensor/temperature
smartroom/sensor/humidity
smartroom/sensor/light
smartroom/actuator/lamp
smartroom/actuator/ac
```

Subscriber melakukan subscribe ke seluruh topik dalam satu koneksi.

---

### Skenario 4 — Wildcard +

Publisher mengirim suhu dari beberapa ruangan:

```text
smartroom/living_room/temperature
smartroom/bedroom/temperature
smartroom/kitchen/temperature
smartroom/bathroom/temperature
```

Subscriber menggunakan wildcard:

```text
smartroom/+/temperature
```

Wildcard `+` menggantikan tepat satu level hierarki.

---

### Skenario 5 — Wildcard #

Subscriber menggunakan:

```text
smartroom/#
```

Wildcard `#` menangkap seluruh topik di bawah prefix:

```text
smartroom/
```

Termasuk:

```text
sensor/*
actuator/*
system/*
```

---

## Format Payload

Contoh payload sensor suhu:

```json
{
  "device": "temperature_sensor",
  "room": "living_room",
  "value": 28,
  "unit": "C",
  "timestamp": "2026-06-12 12:00:00"
}
```

Contoh payload aktuator:

```json
{
  "device": "relay_ac",
  "state": "ON",
  "setpoint": 24,
  "timestamp": "2026-06-12 12:00:00"
}
```

---

## Level QoS MQTT

| QoS | Mekanisme | Jaminan |
|------|------------|----------|
| 0 | Tanpa ACK | Tidak terjamin |
| 1 | PUBACK | Minimal satu kali |
| 2 | PUBREC → PUBREL → PUBCOMP | Tepat satu kali |

---

## Struktur Program

```text
mqtt_smart_room.py
│
├── Helper Function
│   ├── buat_client()
│   ├── koneksi()
│   └── cetak_header()
│
├── Skenario 1
├── Skenario 2
├── Skenario 3
├── Skenario 4
├── Skenario 5
│
└── Menu Interaktif
```

---

## Fitur

- MQTT Publisher & Subscriber
- JSON Payload
- QoS 0, 1, dan 2
- Multi Topic Subscription
- Wildcard +
- Wildcard #
- Monitoring Real-Time
- Single File Interactive Menu
- Kompatibel dengan Paho MQTT v2.x

---

## Author

**Muhammad Radhi Rasyidi Rafli**  
NIM: 235150307111041

Fakultas Ilmu Komputer  
Universitas Brawijaya

---

## License

Project ini dibuat untuk keperluan matakuliah Cyber Physical System.
