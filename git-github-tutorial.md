# Tutorial Lengkap Penggunaan Git dan GitHub

## Pendahuluan
Tutorial ini akan memandu Anda dari awal pembuatan repository hingga proses push ke GitHub, mencakup berbagai skenario yang mungkin Anda hadapi.

## Prasyarat
- Git sudah terinstal di komputer Anda
- Akun GitHub sudah dibuat
- Git sudah dikonfigurasi dengan credential GitHub

Berikut adalah tutorial instalasi Git yang dapat Anda tambahkan ke dokumen:

---

## Instalasi Git

Sebelum menggunakan Git, Anda perlu menginstalnya di komputer Anda. Berikut langkah-langkah instalasi berdasarkan sistem operasi yang Anda gunakan.

### 1. Instalasi Git di Windows
1. **Download Git**  
   - Kunjungi [halaman resmi Git](https://git-scm.com/downloads).
   - Pilih versi untuk Windows dan unduh instalernya.

2. **Jalankan Installer**  
   - Buka file `.exe` yang telah diunduh.
   - Ikuti langkah-langkah instalasi dengan pengaturan default atau sesuaikan sesuai kebutuhan:
     - Pilih editor default (bisa menggunakan Nano, Vim, atau lainnya).
     - Pilih opsi "Git from the command line and also from 3rd-party software".
     - Pastikan opsi "Use Git Credential Manager" diaktifkan untuk menyimpan kredensial.

3. **Verifikasi Instalasi**  
   Setelah instalasi selesai, buka **Command Prompt** atau **Git Bash**, lalu jalankan:
   ```bash
   git --version
   ```
   Jika instalasi berhasil, versi Git akan ditampilkan.

---

### 2. Instalasi Git di macOS
1. **Menggunakan Homebrew (Disarankan)**
   - Jika Homebrew belum terinstal, instal terlebih dahulu dengan menjalankan perintah berikut di Terminal:
     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Setelah Homebrew terinstal, jalankan perintah berikut untuk menginstal Git:
     ```bash
     brew install git
     ```

2. **Menggunakan Git Installer**  
   - Kunjungi [Git untuk macOS](https://git-scm.com/download/mac).
   - Unduh dan jalankan instalernya.

3. **Verifikasi Instalasi**  
   Buka **Terminal** dan jalankan:
   ```bash
   git --version
   ```
   Jika instalasi berhasil, versi Git akan muncul.

---

### 3. Instalasi Git di Linux
#### **Ubuntu/Debian**
```bash
sudo apt update
sudo apt install git -y
```

#### **Fedora**
```bash
sudo dnf install git -y
```

#### **Arch Linux**
```bash
sudo pacman -S git
```

#### **Verifikasi Instalasi**
Setelah proses instalasi selesai, jalankan:
```bash
git --version
```
Jika berhasil, versi Git akan ditampilkan.

---

### 4. Konfigurasi Git Setelah Instalasi
Setelah Git terinstal, Anda perlu mengatur nama pengguna dan email yang akan digunakan dalam setiap commit.

```bash
git config --global user.name "Nama Anda"
git config --global user.email "email@anda.com"
```

Anda dapat memeriksa konfigurasi yang telah diset dengan:
```bash
git config --list
```

Sekarang Git siap digunakan! 🎉

## Skenario 1: Memulai Proyek Baru

### 1. Membuat Repository Baru di GitHub
1. Buka GitHub dan login
2. Klik tombol "+" di pojok kanan atas
3. Pilih "New repository"
4. Isi nama repository
5. Pilih visibility (Public/Private)
6. Klik "Create repository"

### 2. Membuat Proyek dari Awal (Local First)
```bash
# Buat direktori baru
mkdir nama-proyek
cd nama-proyek

# Inisialisasi Git
git init

# Tambahkan remote repository
git remote add origin https://github.com/username/nama-repository.git

# Buat file README
echo "# Nama Proyek" > README.md

# Add dan commit file pertama
git add .
git commit -m "initial commit"

# Push ke GitHub
git branch -M main
git push -u origin main
```

## Skenario 2: Clone Repository yang Sudah Ada

### 1. Clone via HTTPS
```bash
# Clone ke folder dengan nama sama dengan repository
git clone https://github.com/username/nama-repository.git

# Clone ke folder dengan nama tertentu
git clone https://github.com/username/nama-repository.git nama-folder-baru
```

### 2. Clone via SSH (Jika sudah setup SSH key)
```bash
git clone git@github.com:username/nama-repository.git
```

### 3. Clone Branch Tertentu
```bash
# Clone branch spesifik
git clone -b nama-branch https://github.com/username/nama-repository.git
```

## Skenario 3: Mengelola Remote Repository

### 1. Melihat Remote yang Terdaftar
```bash
# Lihat daftar remote
git remote -v
```

### 2. Menambah Remote
```bash
# Format umum
git remote add nama-remote url-repository

# Contoh
git remote add origin https://github.com/username/nama-repository.git
```

### 3. Mengubah URL Remote
```bash
# Mengubah URL remote origin
git remote set-url origin https://github.com/username/repository-baru.git
```

### 4. Menghapus Remote
```bash
git remote remove nama-remote
```

## Langkah-langkah Setelah Clone/Init

### 1. Cek Status Perubahan
```bash
git status
```

### 2. Menambahkan File ke Staging Area
```bash
# Menambahkan semua perubahan
git add .

# Menambahkan file spesifik
git add nama-file.txt
```

### 3. Membuat Commit
```bash
git commit -m "pesan commit anda"
```

### 4. Pull Perubahan Terbaru
```bash
git pull origin main
```

### 5. Push ke GitHub
```bash
git push origin main
```

## Konfigurasi Git

### 1. Konfigurasi Awal
```bash
# Set username
git config --global user.name "Nama Anda"

# Set email
git config --global user.email "email@anda.com"
```

### 2. Menyimpan Kredensial
```bash
# Menyimpan kredensial di cache
git config --global credential.helper cache

# Menyimpan kredensial permanent (Windows)
git config --global credential.helper wincred

# Menyimpan kredensial permanent (macOS)
git config --global credential.helper osxkeychain
```

## Tips Keamanan
- Gunakan HTTPS untuk clone public repository
- Gunakan SSH untuk repository private
- Jangan simpan password di URL remote
- Gunakan Personal Access Token untuk autentikasi HTTPS
- Aktifkan 2FA di akun GitHub

## Struktur Direktori Git
```
nama-proyek/
├── .git/               # Direktori Git (jangan dimodifikasi manual)
├── .gitignore         # File untuk mengatur file yang diabaikan
├── README.md          # Dokumentasi proyek
└── src/               # Source code proyek
```

## Troubleshooting Umum

### Remote Error
```bash
# Jika remote sudah ada
fatal: remote origin already exists
# Solusi:
git remote remove origin
git remote add origin URL-baru
```

### Authentication Failed
```bash
# Pastikan kredensial benar
# Gunakan personal access token sebagai password
# Atau setup SSH key
```

### SSL Certificate Problem
```bash
# Jika ada masalah SSL
git config --global http.sslVerify false  # Gunakan dengan hati-hati
```

## Penutup
Mulailah dengan repository kecil untuk latihan sebelum bekerja dengan proyek besar. Pastikan selalu membuat backup sebelum melakukan operasi Git yang berisiko.