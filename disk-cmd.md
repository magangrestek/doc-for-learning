
# Cara Mengatur Jenis File System (FS) pada SD Card Menggunakan Command Prompt (Windows)

Dokumen ini berisi panduan lengkap tentang cara memformat dan mengatur tipe file system (FS) pada kartu SD di Windows menggunakan **Command Prompt (cmd)**. **PERHATIAN**: Proses ini akan menghapus semua data di SD card. Pastikan Anda telah mencadangkan (backup) semua data penting sebelum melanjutkan.

---

## Metode 1: Menggunakan DiskPart

### 1. Buka Command Prompt sebagai Administrator
1. Klik tombol **Start** pada Windows.
2. Ketik `cmd` pada kolom pencarian.
3. Klik kanan pada **Command Prompt** dan pilih **Run as administrator**.

### 2. Masuk ke DiskPart
```bash
diskpart
```

### 3. Tampilkan Semua Disk
```bash
list disk
```
Perintah ini akan menampilkan semua disk yang terhubung. Identifikasi SD card Anda berdasarkan ukurannya.

### 4. Pilih Disk SD Card
Gantilah `X` dengan nomor disk SD card Anda (hasil dari langkah sebelumnya).
```bash
select disk X
```
> **Peringatan:** Pastikan Anda memilih disk yang benar untuk menghindari kehilangan data pada disk lain.

### 5. Bersihkan Disk (Opsional, namun Disarankan)
Perintah ini akan menghapus seluruh partisi dan data pada disk yang dipilih.
```bash
clean
```

### 6. Buat Partisi Primary
```bash
create partition primary
```

### 7. Format Partisi
Gantilah `FAT32` dengan file system yang Anda inginkan. Pilihan umumnya adalah `FAT32`, `exFAT`, atau `NTFS`.
```bash
format fs=FAT32 quick
```
- Untuk **exFAT**:
  ```bash
  format fs=exFAT quick
  ```
- Untuk **NTFS**:
  ```bash
  format fs=NTFS quick
  ```

### 8. Beri Huruf Drive
```bash
assign
```

### 9. Keluar dari DiskPart
```bash
exit
```

---

## Metode 2: Menggunakan Perintah Format

### 1. Identifikasi Huruf Drive SD Card
Buka **File Explorer** dan perhatikan huruf drive yang ditugaskan untuk SD card Anda, misalnya `E:`.

### 2. Buka Command Prompt sebagai Administrator
Sama seperti pada Metode 1.

### 3. Format SD Card
Gantilah `E:` dengan huruf drive yang sesuai pada komputer Anda, serta `FAT32` dengan tipe file system yang diinginkan.
```bash
format E: /FS:FAT32 /Q
```
- Untuk **exFAT**:
  ```bash
  format E: /FS:exFAT /Q
  ```
- Untuk **NTFS**:
  ```bash
  format E: /FS:NTFS /Q
  ```
  
> `/Q` berarti **quick format** (format cepat).

**Catatan:** Windows membatasi format FAT32 maksimal hingga 32GB. Apabila ukuran SD card lebih besar, Anda mungkin harus menggunakan **DiskPart** (Metode 1) atau software pihak ketiga.

---

## Tips Penting
1. **Backup Data**: Pastikan Anda telah mem-backup semua data penting sebelum memformat.
2. **Pilih Disk dengan Benar**: Cermati baik-baik disk yang dipilih agar tidak salah format.
3. **Administrator Privileges**: Command Prompt perlu dijalankan sebagai administrator agar perintah dapat dieksekusi dengan benar.

---

Dengan mengikuti langkah-langkah di atas, Anda dapat mengatur tipe file system (FS) pada SD card menggunakan Command Prompt di Windows.
