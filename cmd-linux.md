# Percakapan Tentang Mencari File Terbaru di Linux

## Pertanyaan

**User:**
cmd linux yang digunakan untuk mencari file terbaru

## Jawaban

**Asisten:**
Untuk mencari file terbaru di Linux, Anda bisa menggunakan perintah berikut di terminal:

### 1. **Menampilkan File Terbaru dalam Direktori Tertentu**
```bash
ls -lt | head -n 10
```
- **`ls -lt`**: Menampilkan daftar file berdasarkan waktu modifikasi terbaru (descending).  
- **`head -n 10`**: Menampilkan 10 file terbaru.  

### 2. **Mencari File Terbaru di Seluruh Direktori Secara Rekursif**
```bash
find /path/to/directory -type f -printf '%T@ %p\n' | sort -n | tail -1
```
- **`find /path/to/directory -type f`**: Mencari semua file dalam direktori tertentu.  
- **`-printf '%T@ %p\n'`**: Menampilkan timestamp dan nama file.  
- **`sort -n`**: Mengurutkan berdasarkan timestamp (ascending).  
- **`tail -1`**: Mengambil file dengan timestamp terbaru.  

### 3. **Menggunakan `stat` untuk Detail File Terbaru**
```bash
find /path/to/directory -type f -exec stat --format '%Y :%y %n' {} + | sort -n | tail -1
```
- Menampilkan informasi waktu modifikasi dalam format yang lebih rinci.  

### 4. **Hanya Menampilkan Nama File Terbaru**
```bash
find /path/to/directory -type f -printf '%T@ %p\n' | sort -n | awk '{print $2}' | tail -1
```

Gantilah `/path/to/directory` dengan direktori yang ingin Anda telusuri. Jika ingin mencari dari direktori saat ini, gunakan titik (`.`) sebagai path.  

### Contoh:
```bash
find . -type f -printf '%T@ %p\n' | sort -n | tail -1
```
Output:  
```
1679876543.123456789 ./example/latest_file.txt
```

# Backup and Rollback Configuration for ProFTPD

## Creating a Backup of `proftpd.conf`
To ensure you can rollback changes to the `proftpd.conf` file, create a backup first.

### **1. Copy File with `.bak` Extension**
```bash
sudo cp /etc/proftpd/proftpd.conf /etc/proftpd/proftpd.conf.bak
```
This command creates a copy of the configuration file with the name `proftpd.conf.bak` in the same directory.

---

### **2. Verify the Backup**
Check if the backup file is created:
```bash
ls -l /etc/proftpd/proftpd.conf*
```
Expected output:
```
-rw-r--r-- 1 root root 2048 Jan 6 10:30 proftpd.conf
-rw-r--r-- 1 root root 2048 Jan 6 10:30 proftpd.conf.bak
```

---

### **3. Rollback Configuration**
If you need to restore the backup, use the following command:
```bash
sudo cp /etc/proftpd/proftpd.conf.bak /etc/proftpd/proftpd.conf
```

---

### **4. Restart ProFTPD Service**
After making changes or rolling back, restart the ProFTPD service to apply the configuration:
```bash
sudo systemctl restart proftpd
```

---

## Additional Tips

### **Backup with Timestamp**
For better tracking, include a timestamp in the backup file name:
```bash
sudo cp /etc/proftpd/proftpd.conf /etc/proftpd/proftpd.conf.$(date +%F_%H-%M-%S).bak
```
Example backup file:
```
proftpd.conf.2025-01-06_10-30-15.bak
```

### **Check ProFTPD Service Status**
Ensure the ProFTPD service is running after changes:
```bash
sudo systemctl status proftpd
```
