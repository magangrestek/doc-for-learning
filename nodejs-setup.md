# Panduan Instalasi Node.js 20 dan PM2 di Ubuntu

## Daftar Isi
- [Persiapan Awal](#persiapan-awal)
- [Instalasi Node.js dengan NVM](#instalasi-nodejs-dengan-nvm)
- [Instalasi PM2](#instalasi-pm2)
- [Penggunaan Dasar PM2](#penggunaan-dasar-pm2)
- [Troubleshooting](#troubleshooting)

## Persiapan Awal

Sebelum memulai instalasi, pastikan sistem Ubuntu Anda sudah diperbarui:

```bash
sudo apt update
sudo apt upgrade -y
```

## Instalasi Node.js dengan NVM

1. Install curl jika belum tersedia:
```bash
sudo apt install curl
```

2. Install NVM:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

3. Load NVM ke environment:
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

4. Verifikasi instalasi NVM:
```bash
nvm --version
```

5. Install Node.js versi 20:
```bash
nvm install 20
```

6. Set Node.js versi 20 sebagai default:
```bash
nvm alias default 20
nvm use 20
```

7. Verifikasi instalasi:
```bash
node --version  # Harus menampilkan v20.x.x
npm --version   # Verifikasi versi npm
```

## Instalasi PM2

1. Install PM2 secara global menggunakan NPM:
```bash
npm install pm2@latest -g
```

2. Verifikasi instalasi PM2:
```bash
pm2 --version
```

3. (Opsional) Konfigurasi PM2 untuk berjalan saat startup:
```bash
pm2 startup ubuntu
```

## Penggunaan Dasar PM2

### Mengelola Aplikasi

1. Memulai aplikasi:
```bash
pm2 start app.js              # Aplikasi Node.js basic
pm2 start app.js --name myapp # Dengan nama kustom
pm2 start npm -- start        # Menjalankan npm start
```

2. Melihat daftar aplikasi:
```bash
pm2 list
```

3. Monitoring aplikasi:
```bash
pm2 monit
pm2 status
```

4. Mengelola aplikasi yang sedang berjalan:
```bash
pm2 stop myapp     # Menghentikan aplikasi
pm2 restart myapp  # Restart aplikasi
pm2 reload myapp   # Reload aplikasi dengan 0 downtime
pm2 delete myapp   # Menghapus aplikasi dari daftar PM2
```

### Log Management

```bash
pm2 logs            # Menampilkan log semua aplikasi
pm2 logs myapp      # Menampilkan log aplikasi tertentu
pm2 flush           # Membersihkan log
```

### Cluster Mode

```bash
pm2 start app.js -i max  # Cluster mode dengan jumlah CPU maksimum
pm2 scale myapp 2        # Scale ke 2 instance
```

## Troubleshooting

### Masalah dengan NVM

1. Jika perintah nvm tidak ditemukan setelah instalasi:
```bash
source ~/.bashrc
```

2. Jika masih tidak berfungsi, tambahkan ini ke ~/.bashrc:
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### Masalah Permission NPM Global

Jika mengalami masalah permission saat menginstall package global:

```bash
# Tidak perlu sudo saat menggunakan NVM
npm install -g [package_name]
```

### Error Umum

1. Jika muncul error EACCES:
```bash
sudo chown -R $USER:$USER ~/.npm
sudo chown -R $USER:$USER ~/.config
```

## Perintah Berguna Lainnya

### Manajemen Versi Node.js dengan NVM

```bash
nvm ls                    # Lihat versi Node.js yang terinstall
nvm ls-remote            # Lihat semua versi Node.js yang tersedia
nvm install node         # Install versi Node.js terbaru
nvm install --lts        # Install versi LTS terbaru
nvm use 20              # Gunakan Node.js versi 20
```

### Backup dan Restore PM2

```bash
pm2 save              # Menyimpan daftar proses yang berjalan
pm2 resurrect         # Mengembalikan proses yang tersimpan
```

### Update PM2

```bash
npm install pm2@latest -g   # Update ke versi terbaru
pm2 update                  # Update in-memory PM2
```