# Tutorial: Manajemen Folder di Linux: Memindahkan dan Menghapus

## Daftar Isi
- [Prasyarat](#prasyarat)
- [Langkah-langkah](#langkah-langkah)
- [Pengaturan Izin](#pengaturan-izin)
- [Menghapus Folder](#menghapus-folder)
- [Troubleshooting](#troubleshooting)
- [Tips Tambahan](#tips-tambahan)

## Prasyarat
Sebelum memulai, pastikan Anda memiliki:
- Akses root atau sudo privileges
- Folder yang ingin dipindahkan di direktori home
- Izin yang sesuai untuk direktori tujuan

## Langkah-langkah

### 1. Memindahkan Folder
Gunakan perintah berikut untuk memindahkan folder:
```bash
sudo mv ~/nama_folder /var/www/html/
```

Contoh dengan folder bernama "website":
```bash
sudo mv ~/website /var/www/html/
```

### 2. Verifikasi Pemindahan
Periksa apakah folder telah dipindahkan dengan benar:
```bash
ls -l /var/www/html/
```

## Pengaturan Izin

### 1. Mengatur Kepemilikan
Ubah kepemilikan folder ke www-data (webserver user):
```bash
sudo chown -R www-data:www-data /var/www/html/nama_folder
```

### 2. Mengatur Permission
Atur izin yang sesuai untuk folder web:
```bash
sudo chmod -R 755 /var/www/html/nama_folder
```

Penjelasan permission 755:
- 7 (owner): read, write, execute
- 5 (group): read, execute
- 5 (others): read, execute

## Troubleshooting

### Masalah Umum dan Solusi

1. **Permission Denied**
   ```bash
   sudo chmod -R 755 /var/www/html/nama_folder
   sudo chown -R $USER:$USER /var/www/html/nama_folder
   ```

2. **Folder Tidak Ditemukan**
   - Periksa path yang benar
   - Pastikan nama folder sesuai (case sensitive)
   ```bash
   ls -la ~/
   ```

3. **Cannot Move Directory**
   - Periksa ruang disk
   - Periksa izin di direktori tujuan
   ```bash
   df -h
   ls -la /var/www/html/
   ```

## Menghapus Folder

### 1. Menghapus Folder Kosong
Untuk menghapus folder yang kosong, gunakan perintah `rmdir`:
```bash
rmdir nama_folder
```

### 2. Menghapus Folder Beserta Isinya
Untuk menghapus folder yang berisi file atau subfolder, gunakan perintah `rm` dengan opsi `-r` (recursive):
```bash
rm -r nama_folder
```

### 3. Opsi Tambahan untuk Penghapusan
- Menghapus secara paksa tanpa konfirmasi:
```bash
rm -rf nama_folder
```

- Menghapus dengan konfirmasi dan tampilan proses:
```bash
rm -riv nama_folder
```

Keterangan opsi:
- `-f` (force): memaksa penghapusan tanpa konfirmasi
- `-v` (verbose): menampilkan proses penghapusan
- `-i` (interactive): meminta konfirmasi sebelum menghapus

### 4. Praktik Keamanan saat Menghapus
1. Selalu verifikasi folder yang akan dihapus
2. Hindari penggunaan `rm -rf /` karena dapat menghapus seluruh sistem
3. Backup data penting sebelum menghapus
4. Gunakan opsi `-i` untuk konfirmasi jika tidak yakin

## Tips Tambahan

### Backup Sebelum Memindahkan
1. Membuat backup folder:
```bash
sudo cp -r ~/nama_folder ~/nama_folder_backup
```

### Memindahkan dengan Opsi Tambahan
1. Memindahkan dengan verbose (menampilkan proses):
```bash
sudo mv -v ~/nama_folder /var/www/html/
```

2. Memindahkan dan menyalin atribut:
```bash
sudo cp -a ~/nama_folder /var/www/html/
sudo rm -r ~/nama_folder
```

### Monitoring Log
Jika terjadi masalah, cek log sistem:
```bash
sudo tail -f /var/log/syslog
```

## Catatan Keamanan
1. Selalu verifikasi path sebelum menjalankan perintah mv
2. Backup data penting sebelum memindahkan
3. Pastikan permission yang diberikan sesuai dengan kebutuhan keamanan
4. Gunakan sudo dengan hati-hati

## Penutup
Setelah mengikuti langkah-langkah di atas, folder Anda seharusnya sudah dipindahkan dengan aman ke direktori /var/www/html/ dan siap digunakan oleh web server.

---
*Pastikan untuk selalu memverifikasi setiap langkah dan backup data penting sebelum melakukan operasi pemindahan.*