# Cara Mengubah Timezone di Linux ke UTC+7 (Asia/Jakarta)

Berikut langkah-langkah untuk mengubah timezone di Linux menjadi Waktu Indonesia Barat (WIB) yang memiliki offset +7:

---

## 1. **Cek Timezone Saat Ini**
Gunakan perintah berikut untuk memeriksa timezone yang sedang digunakan:

```bash
timedatectl
```

Perintah ini akan menampilkan informasi waktu dan timezone saat ini.

---

## 2. **Set Timezone ke Asia/Jakarta (+7)**
Jalankan perintah berikut untuk mengatur timezone ke `Asia/Jakarta`:

```bash
sudo timedatectl set-timezone Asia/Jakarta
```

---

## 3. **Verifikasi Timezone Baru**
Setelah mengubah timezone, verifikasi perubahan dengan menjalankan kembali perintah:

```bash
timedatectl
```

Pastikan timezone sudah berubah menjadi `Asia/Jakarta`.

---

## 4. **Sinkronisasi dengan NTP (Opsional)**
Untuk memastikan waktu tetap akurat, aktifkan sinkronisasi dengan NTP menggunakan perintah:

```bash
sudo timedatectl set-ntp true
```

Perintah ini memastikan waktu server disinkronkan dengan waktu dari internet.

---

## Troubleshooting (Jika Ada Masalah)
1. **Periksa File Timezone**:
   Lihat file timezone di `/etc/localtime`:

   ```bash
   ls -l /etc/localtime
   ```

   Pastikan file tersebut merupakan symlink ke `Asia/Jakarta`.

2. **Atur Manual dengan Symlink**:
   Jika metode di atas tidak berhasil, Anda bisa langsung membuat symlink secara manual:

   ```bash
   sudo ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
   ```

Timezone sekarang seharusnya berubah ke UTC+7.

---

