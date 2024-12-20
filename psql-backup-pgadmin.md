# Cara Backup Database PostgreSQL Menggunakan pgAdmin4

## Persiapan
- Pastikan pgAdmin4 sudah terinstall
- Pastikan database PostgreSQL yang ingin di-backup dalam keadaan aktif
- Siapkan ruang penyimpanan yang cukup untuk hasil backup

## Langkah-langkah Backup

### 1. Akses pgAdmin
- Buka browser Anda
- Akses alamat pgAdmin (biasanya http://localhost/pgadmin4 atau http://127.0.0.1/pgadmin4)
- Masukkan email dan password pgAdmin Anda

### 2. Navigasi ke Database
- Pada panel sebelah kiri, klik tanda panah (▶) pada "Servers"
- Expand server PostgreSQL Anda 
- Expand folder "Databases"
- Temukan database yang ingin Anda backup

### 3. Proses Backup
- Klik kanan pada nama database yang akan di-backup
- Pilih menu "Backup..." dari daftar opsi yang muncul

### 4. Konfigurasi Backup
**Tab General**
- Filename: Pilih lokasi dan nama file backup (akan otomatis berakhiran .sql)
- Format: Pilih "Plain" 
- Encoding: Gunakan "UTF8" (default)
- Role name: Pilih "postgres" atau user database Anda

**Tab Dump Options**
- Sections: Biarkan default untuk backup lengkap
- Type of objects: Biarkan default
- Do not save: Biarkan default
- Queries: Biarkan default
- Disable: Biarkan default

### 5. Eksekusi Backup
- Periksa kembali semua pengaturan
- Klik tombol "Backup"
- Tunggu proses backup selesai
- File .sql akan tersimpan di lokasi yang telah ditentukan

## Catatan Penting
- Pastikan koneksi database stabil selama proses backup
- Backup pada jam non-peak untuk database yang besar
- Pastikan ada ruang penyimpanan yang cukup untuk file backup
- File backup akan berformat .sql yang bisa digunakan untuk restore di kemudian hari