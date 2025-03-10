# Tutorial Membuat Database dan Tabel Raw Data Sensor IoT di MySQL

## Bagian 1: Instalasi MySQL di Raspberry Pi

### Langkah 1: Update Sistem Raspberry Pi
```bash
sudo apt update
sudo apt upgrade -y
```

### Langkah 2: Instal MariaDB (fork MySQL yang populer di Raspberry Pi)
```bash
sudo apt install mariadb-server -y
```


### Langkah 3: Verifikasi Instalasi
```bash
sudo systemctl status mariadb
```
Output harus menunjukkan status "active (running)".

## Bagian 2: Konfigurasi MySQL untuk Akses Remote

### Langkah 1: Edit File Konfigurasi MySQL
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

### Langkah 2: Ubah Bind-Address
Cari baris yang berisi `bind-address = 127.0.0.1` dan ubah menjadi:
```
bind-address = 0.0.0.0
```

### Langkah 3: Simpan dan Restart MySQL
Simpan file (Ctrl+O, Enter, Ctrl+X) lalu restart MySQL:
```bash
sudo systemctl restart mariadb
```

### Langkah 4: Buat User untuk Akses Remote
```bash
sudo mysql -u root -p
```

Di prompt MySQL, jalankan:
```sql
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'password_aman';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%';
FLUSH PRIVILEGES;
EXIT;
```

### Langkah 5: Konfigurasi Firewall (jika aktif)
```bash
sudo ufw allow 3306/tcp
```

## Bagian 3: Membuat Database dan Tabel Raw Data

### Langkah 1: Login ke MySQL
```bash
sudo mysql -u root -p
```

### Langkah 2: Buat Database
```sql
CREATE DATABASE iot;
USE iot;
```

### Langkah 3: Buat Tabel Raw Data dan stations
```sql
CREATE TABLE IF NOT EXISTS raw_data (
    id INT(11) NOT NULL AUTO_INCREMENT,
    log_time DATETIME,
    sensor_id VARCHAR(255),
    gps_lat DOUBLE,
    gps_lon DOUBLE,
    gps_alt DOUBLE,
    gps_speed DOUBLE,
    accelero_ax DOUBLE,
    accelero_ay DOUBLE,
    accelero_az DOUBLE,
    accelero_gx DOUBLE,
    accelero_gy DOUBLE,
    accelero_gz DOUBLE,
    accelero_speed DOUBLE,
    magnetometer_heading DOUBLE,
    raw_message TEXT,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE IF NOT EXISTS stations (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    longitude DOUBLE,
    latitude DOUBLE,
    active BOOLEAN,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Langkah 5: Verifikasi Struktur Tabel
```sql
DESCRIBE raw_data;
DESCRIBE stations;
```


## Bagian 4: Contoh Insert Data

### Insert Data Manual
```sql
INSERT INTO raw_data (
    log_time, 
    sensor_id, 
    gps_lat, 
    gps_lon, 
    gps_alt, 
    gps_speed,
    accelero_ax, 
    accelero_ay, 
    accelero_az, 
    accelero_gx, 
    accelero_gy, 
    accelero_gz,
    accelero_speed, 
    magnetometer_heading, 
    raw_message
) VALUES (
    NOW(), 
    'GPS001', 
    -6.1745, 
    106.8442, 
    45.5, 
    0.0,
    0.01, 
    0.02, 
    9.8, 
    0.001, 
    0.002, 
    0.003,
    0.0, 
    275.5, 
    '{\"device_id\":\"GPS001\",\"timestamp\":\"2025-03-10T12:34:56Z\",\"readings\":{\"gps\":{\"lat\":-6.1745,\"lon\":106.8442}}}'
);

INSERT INTO stations (name, longitude, latitude, active) VALUES ('PASAR SENEN', 106.8442, -6.1745, 1);
INSERT INTO stations (name, longitude, latitude, active) VALUES ('GAMBIR', 106.8302, -6.1767, 1);
-- Tambahkan data lainnya
```

## Bagian 5: Menguji Koneksi Remote

### Dari Komputer Lain
Gunakan salah satu metode berikut:

1. **MySQL Workbench (GUI):**
   - Install MySQL Workbench
   - Buat koneksi baru:
     - Hostname: IP Raspberry Pi (contoh: 192.168.1.100)
     - Port: 3306
     - Username: remote_user
     - Password: password_aman

2. **Command Line:**
   ```bash
   mysql -h 192.168.1.100 -u remote_user -p sensor_database
   ```

## Bagian 6: Troubleshooting Koneksi Remote

### Periksa Status MySQL dan Binding
```bash
sudo netstat -tuln | grep 3306
```
Harusnya menampilkan: `tcp 0 0 0.0.0.0:3306 0.0.0.0:* LISTEN`

### Periksa Firewall
```bash
sudo ufw status
```
Pastikan port 3306 diizinkan.

### Restart MySQL jika Diperlukan
```bash
sudo systemctl restart mariadb
```

### Periksa Logs untuk Error
```bash
sudo tail -f /var/log/mysql/error.log
```

## Bagian 7: Tips Keamanan untuk Database Terpencil

1. **Gunakan Password yang Kuat**
2. **Batasi Akses Berdasarkan IP**
   ```sql
   CREATE USER 'remote_user'@'192.168.1.%' IDENTIFIED BY 'password_aman';
   GRANT ALL PRIVILEGES ON sensor_database.* TO 'remote_user'@'192.168.1.%';
   ```

3. **Pertimbangkan Menggunakan SSH Tunnel**
   ```bash
   ssh -L 3306:localhost:3306 pi@raspberry_pi_ip
   ```
   Lalu hubungkan ke `localhost:3306` pada komputer Anda.

4. **Aktifkan SSL untuk Koneksi MySQL**
   ```bash
   # Di file konfigurasi MySQL
   ssl-ca=/etc/mysql/ca-cert.pem
   ssl-cert=/etc/mysql/server-cert.pem
   ssl-key=/etc/mysql/server-key.pem
   ```

5. **Backup Berkala**
   ```bash
   mysqldump -u root -p sensor_database > backup_$(date +%Y%m%d).sql
   ```

Dengan mengikuti tutorial ini, Anda telah berhasil menginstal dan mengkonfigurasi MySQL di Raspberry Pi, membuat tabel raw_data, menambahkan data, dan mengaktifkan akses remote dengan aman.