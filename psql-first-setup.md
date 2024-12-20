# Tutorial Instalasi PostgreSQL 14 di Ubuntu 22.04

## Prasyarat
- Ubuntu 22.04 LTS
- Akses root atau sudo
- Koneksi internet

## Langkah-langkah Instalasi

### 1. Update Sistem
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Tambahkan Repository PostgreSQL
```bash
# Tambahkan PostgreSQL repository GPG key
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

### 3. Update Package List
```bash
sudo apt update
```

### 4. Install PostgreSQL 14
```bash
sudo apt install postgresql-14 postgresql-contrib postgresql-contrib-14 -y
```

### 5. Verifikasi Instalasi
```bash
sudo systemctl status postgresql
```

### 6. Konfigurasi PostgreSQL

#### Mengatur Password PostgreSQL
```bash
# Masuk ke PostgreSQL shell
sudo -u postgres psql

# Di dalam PostgreSQL shell, atur password
ALTER USER postgres WITH PASSWORD 'password_baru';
\q
```

#### Mengaktifkan Remote Access

1. Edit postgresql.conf:
```bash
sudo nano /etc/postgresql/14/main/postgresql.conf
```

Ubah konfigurasi berikut:
```
listen_addresses = '*'
```

2. Edit pg_hba.conf:
```bash
sudo nano /etc/postgresql/14/main/pg_hba.conf
```

Tambahkan baris berikut di bagian bawah file:
```
# IPv4 remote connections
host    all             all             0.0.0.0/0               md5
# IPv6 remote connections
host    all             all             ::/0                    md5
```

### 7. Restart PostgreSQL
```bash
sudo systemctl restart postgresql
```

### 8. Konfigurasi Firewall
```bash
sudo ufw allow 5432/tcp
```

## Perintah Dasar PostgreSQL

### Manajemen Service
```bash
# Start PostgreSQL
sudo systemctl start postgresql

# Stop PostgreSQL
sudo systemctl stop postgresql

# Restart PostgreSQL
sudo systemctl restart postgresql

# Enable PostgreSQL pada startup
sudo systemctl enable postgresql
```

### Akses Database
```bash
# Masuk ke PostgreSQL shell
sudo -u postgres psql

# Daftar database
\l

# Keluar dari PostgreSQL shell
\q
```

## Catatan Keamanan
- Ganti 'password_baru' dengan password yang kuat
- Pertimbangkan untuk membatasi akses remote ke IP atau subnet tertentu
- Secara berkala update PostgreSQL untuk mendapatkan patch keamanan terbaru

## Troubleshooting

### Jika gagal melakukan koneksi remote:
1. Pastikan postgresql.conf mengizinkan remote connections
2. Verifikasi konfigurasi pg_hba.conf
3. Periksa status firewall
4. Cek status service PostgreSQL

### Log PostgreSQL dapat ditemukan di:
```bash
/var/log/postgresql/postgresql-14-main.log
```