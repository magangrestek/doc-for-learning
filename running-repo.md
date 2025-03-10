### **1. Clone Repository**
Pastikan Raspberry Pi terhubung ke internet, lalu buka terminal dan jalankan:
```bash
git clone https://github.com/magangrestek/gps-middleware.git
cd gps-middleware
```

---

### **2. Install Dependencies**
Pastikan Node.js sudah terinstal. Jika belum, cek versinya:
```bash
node -v
npm -v
```
Jika sudah, install dependencies proyek:
```bash
npm install
```

---

### **3. Konfigurasi Environment**
Cek apakah ada file `.env.example` atau dokumentasi terkait konfigurasi. Jika ada, buat file `.env` dengan:
```bash
cp .env.example .env
```
Kemudian edit `.env` menggunakan `nano` atau VSCode:
```bash
nano .env
```
Atau jika VSCode terinstall:
```bash
code .env
```
Sesuaikan variabel dengan kebutuhan.

---

### **4. Jalankan Aplikasi Secara Manual**
Jalankan aplikasi untuk memastikan tidak ada error:
```bash
node app.js
```
Atau jika menggunakan `npm start`:
```bash
npm start
```
Jika berhasil, aplikasi akan berjalan dan menampilkan log di terminal.

---

### **5. Jalankan dengan PM2**
Agar aplikasi tetap berjalan meskipun terminal ditutup atau Raspberry Pi restart, gunakan **PM2**:
```bash
pm2 start app.js --name gps-middleware
pm2 save
pm2 startup
```
Perintah `pm2 startup` akan memberikan instruksi tambahan untuk memastikan aplikasi berjalan saat booting. Ikuti langkahnya.

---

### **6. Cek Status dan Log**
- Cek apakah aplikasi berjalan:
  ```bash
  pm2 list
  ```
- Lihat log aplikasi:
  ```bash
  pm2 logs gps-middleware
  ```
- Jika perlu restart aplikasi:
  ```bash
  pm2 restart gps-middleware
  ```
- Jika perlu menghentikan aplikasi:
  ```bash
  pm2 stop gps-middleware
  ```
- Jika ingin menghapus aplikasi dari PM2:
  ```bash
  pm2 delete gps-middleware
  ```

---

### **7. Tes API (Opsional)**
Jika aplikasi menyediakan API, cek dengan `curl` atau Postman:
```bash
curl http://localhost:3000/api/status
```
Sesuaikan port berdasarkan konfigurasi di `.env`.

---

Jika ada error saat running, beri tahu saya pesan errornya. 🚀