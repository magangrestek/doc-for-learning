# 🔧 Mengatasi "fatal: Authentication failed" Saat Git Pull

Jika Anda mengalami error **"fatal: Authentication failed"** saat menjalankan `git pull`, ikuti langkah-langkah berikut untuk memperbaikinya.

---

## **1. Periksa URL Remote**
Sebelum melakukan perbaikan, pastikan repository dikonfigurasi dengan URL yang benar.

```sh
git remote -v
```

Jika URL masih dalam format HTTPS:
```
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```
Anda perlu memperbarui kredensialnya.

---

## **2. Gunakan Personal Access Token (PAT)**
GitHub tidak lagi mendukung autentikasi menggunakan username dan password melalui HTTPS. Anda harus menggunakan **Personal Access Token (PAT)** sebagai pengganti password.

### **(a) Buat Personal Access Token (PAT)**
1. Buka **[GitHub Token Settings](https://github.com/settings/tokens)**.
2. Klik **Generate new token (classic)**.
3. Pilih cakupan akses dengan mencentang `repo`.
4. Klik **Generate token** dan salin token yang dihasilkan.

### **(b) Ubah URL Remote dengan Token**
Ubah URL `origin` menggunakan token yang sudah dibuat:

```sh
git remote set-url origin https://<TOKEN>@github.com/username/repository.git
```

Misalnya, jika token Anda adalah `ghp_xxx123`:
```sh
git remote set-url origin https://ghp_xxx123@github.com/username/repository.git
```

### **(c) Coba Git Pull Lagi**
Setelah mengubah URL, coba tarik perubahan dengan:

```sh
git pull origin main
```

Jika branch utama Anda adalah `master`, gunakan:
```sh
git pull origin master
```

---

## **3. Menghapus Kredensial Lama (Opsional)**
Jika Anda pernah menyimpan kredensial lama, hapus dengan perintah berikut:

- **Windows:**
  ```sh
  git credential reject https://github.com
  ```
- **Linux/Mac:**
  ```sh
  git credential reject https://github.com
  ```

Setelah itu, coba ulangi `git pull` dengan memasukkan token saat diminta.

---

## **4. Alternatif: Gunakan SSH (Rekomendasi)**
Agar tidak perlu menggunakan token setiap kali, gunakan autentikasi **SSH**.

### **(a) Buat SSH Key (jika belum ada)**
```sh
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### **(b) Tambahkan ke GitHub**
- Jalankan:
  ```sh
  cat ~/.ssh/id_ed25519.pub
  ```
- Salin hasilnya, lalu tambahkan ke **GitHub > Settings > SSH and GPG keys**.

### **(c) Ubah URL Remote ke SSH**
```sh
git remote set-url origin git@github.com:username/repository.git
```

### **(d) Tes Koneksi**
```sh
ssh -T git@github.com
```
Jika sukses, coba tarik perubahan:
```sh
git pull origin main
```

---

## **5. Kesimpulan**
Jika mengalami error **"fatal: Authentication failed"** saat `git pull`, Anda bisa:
1. Menggunakan **Personal Access Token (PAT)** untuk autentikasi HTTPS.
2. Menghapus kredensial lama jika perlu.
3. Beralih ke **SSH** untuk autentikasi yang lebih mudah dan aman.

Semoga bermanfaat! 🚀
