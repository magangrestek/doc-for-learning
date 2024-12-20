# Tutorial Lengkap Penggunaan Git dan GitHub

## Pendahuluan
Tutorial ini akan memandu Anda dari awal pembuatan repository hingga proses push ke GitHub, mencakup berbagai skenario yang mungkin Anda hadapi.

## Prasyarat
- Git sudah terinstal di komputer Anda
- Akun GitHub sudah dibuat
- Git sudah dikonfigurasi dengan credential GitHub

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