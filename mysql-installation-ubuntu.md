# Remote MySQL Server di Ubuntu

Dokumentasi ini menjelaskan cara menginstal dan mengkonfigurasi MySQL di Ubuntu agar dapat diakses secara remote dari komputer lain.

## 1. Instalasi MySQL di Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install mysql-server -y
```

Setelah instalasi selesai, pastikan MySQL berjalan:

```bash
sudo systemctl status mysql
```
Jika MySQL belum berjalan, jalankan:
```bash
sudo systemctl start mysql
```

## 2. Konfigurasi MySQL untuk Akses Remote

### 2.1 Ubah Konfigurasi MySQL agar Mendengarkan di Semua IP

Edit file konfigurasi MySQL:
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Cari baris berikut:
```
bind-address = 127.0.0.1
```
Ubah menjadi:
```
bind-address = 0.0.0.0
```
Simpan dengan **Ctrl + X → Y → Enter**.
Restart MySQL agar perubahan berlaku:
```bash
sudo systemctl restart mysql
```

### 2.2 Buat User dengan Hak Akses Remote

Masuk ke MySQL:
```bash
sudo mysql -u root -p
```
Lalu buat user baru dengan akses remote:
```sql
CREATE USER 'remoteuser'@'%' IDENTIFIED BY 'passwordku';
GRANT ALL PRIVILEGES ON *.* TO 'remoteuser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
> **Catatan:**
> - Ganti `'remoteuser'` dengan nama pengguna yang diinginkan.
> - Ganti `'passwordku'` dengan kata sandi yang aman.
> - `'%'` berarti user bisa login dari mana saja. Untuk membatasi akses ke IP tertentu, ganti `%` dengan IP spesifik.

### 2.3 Izinkan Firewall untuk Koneksi MySQL

Jika menggunakan firewall **UFW**, buka port 3306 agar bisa diakses dari luar:
```bash
sudo ufw allow 3306/tcp
sudo ufw reload
```
Cek status firewall:
```bash
sudo ufw status
```

Jika firewall masih memblokir, nonaktifkan sementara untuk pengujian:
```bash
sudo ufw disable
```
> **Penting:** Setelah pengujian, aktifkan kembali dengan:
> ```bash
> sudo ufw enable
> ```

## 3. Cek MySQL Mendengarkan di IP Publik

Gunakan perintah berikut untuk memastikan MySQL berjalan di semua antarmuka jaringan:
```bash
sudo netstat -tulnp | grep mysql
```
Atau:
```bash
ss -tulnp | grep mysql
```
Seharusnya ada output yang menunjukkan MySQL berjalan di `0.0.0.0:3306` atau IP spesifik.

## 4. Menghubungkan ke MySQL dari Komputer Lain

Dari komputer client, coba koneksi dengan:
```bash
mysql -h IP_SERVER -u remoteuser -p
```
Gantilah `IP_SERVER` dengan alamat IP server Ubuntu.

### Menggunakan MySQL Workbench atau DBeaver:
- **Host**: `IP_SERVER`
- **User**: `remoteuser`
- **Password**: `passwordku`
- **Port**: `3306`
- **Koneksi**: `Standard TCP/IP`

## 5. Troubleshooting Jika Tidak Bisa Remote

- Pastikan MySQL berjalan:
  ```bash
  sudo systemctl status mysql
  ```
  Jika tidak berjalan, restart dengan:
  ```bash
  sudo systemctl restart mysql
  ```
- Cek apakah firewall masih memblokir:
  ```bash
  sudo iptables -L -n
  ```
- Jika menggunakan AWS atau cloud provider lain, pastikan **Security Group** mengizinkan koneksi ke **port 3306**.

## 6. Membuat Database Monitoring Perjalanan Kereta

```sql
-- 1. Buat database monitoring perjalanan kereta
CREATE DATABASE train_tracking;

-- 2. Gunakan database yang baru dibuat
USE train_tracking;

-- 3. Buat tabel devices (menyimpan informasi perangkat/kereta)
CREATE TABLE devices (
    id INT AUTO_INCREMENT PRIMARY KEY,
    device_id VARCHAR(50) UNIQUE NOT NULL,
    train_name VARCHAR(100) NOT NULL
);

ALTER TABLE devices ADD COLUMN status CHAR(1) NOT NULL DEFAULT '1';

-- 4. Buat tabel train_logs (menyimpan log perjalanan kereta)
CREATE TABLE train_logs (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    device_id VARCHAR(50) NOT NULL,
    timestamp DATETIME NOT NULL,
    latitude DECIMAL(9,6) NOT NULL,
    longitude DECIMAL(9,6) NOT NULL,
    speed FLOAT NOT NULL,
    heading FLOAT NOT NULL,
    station_id INT,
    station_name VARCHAR(100),
    distance_to_station INT,
    eta_minutes INT,
    FOREIGN KEY (device_id) REFERENCES devices(device_id) ON DELETE CASCADE
);

-- 5. Buat index untuk optimasi pencarian data berdasarkan device_id dan timestamp
CREATE INDEX idx_device_time ON train_logs (device_id, timestamp);

-- 6. Buat tabel user_management untuk menyimpan data pengguna
CREATE TABLE user_management (
    username VARCHAR(30) PRIMARY KEY,
    password TEXT NOT NULL,
    nama_lengkap VARCHAR(50) NOT NULL,
    aktif CHAR(1) NOT NULL DEFAULT '1' -- '1' untuk aktif, '0' untuk nonaktif
);

-- 7. Tambahkan indeks unik pada username untuk memastikan tidak ada duplikat
CREATE UNIQUE INDEX idx_username ON user_management (username);

-- 8. Buat tabel raw_data untuk menyimpan data sensor dan GPS
CREATE TABLE train_tracking.raw_data (
    id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
    log_time TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    sensor_id VARCHAR(255) NOT NULL,
    gps_lat DOUBLE NOT NULL DEFAULT 0,
    gps_lon DOUBLE NOT NULL DEFAULT 0,
    gps_alt DOUBLE NOT NULL DEFAULT 0,
    gps_speed DOUBLE NOT NULL DEFAULT 0,
    accelero_ax DOUBLE NOT NULL DEFAULT 0,
    accelero_ay DOUBLE NOT NULL DEFAULT 0,
    accelero_az DOUBLE NOT NULL DEFAULT 0,
    accelero_gx DOUBLE NOT NULL DEFAULT 0,
    accelero_gy DOUBLE NOT NULL DEFAULT 0,
    accelero_gz DOUBLE NOT NULL DEFAULT 0,
    accelero_speed DOUBLE NOT NULL DEFAULT 0,
    magnetometer_heading DOUBLE NOT NULL DEFAULT 0
);
```

## 7. Kesimpulan

- Konfigurasi MySQL agar menerima koneksi dari semua IP (`bind-address = 0.0.0.0`).
- Buat user dengan akses remote (`CREATE USER 'remoteuser'@'%'`).
- Izinkan firewall membuka port **3306**.
- Buat database **train_tracking** untuk monitoring perjalanan kereta.
- Coba koneksi dari komputer lain dengan **MySQL CLI** atau **MySQL Workbench**.

Sekarang MySQL sudah bisa diakses secara remote dan memiliki database pemantauan perjalanan kereta! 🚀

