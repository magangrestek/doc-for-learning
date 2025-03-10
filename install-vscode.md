## Visual Studio Code (VS Code) di **Raspberry Pi**, 
ikuti langkah-langkah berikut:

### **1. Perbarui Sistem**
Sebelum menginstal, pastikan sistem operasi **Raspberry Pi OS** sudah diperbarui:
```bash
sudo apt update && sudo apt upgrade -y
```

### **2. Tambahkan Repository Microsoft**
Tambahkan kunci GPG dan repository Microsoft:
```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/packages.microsoft.gpg > /dev/null
echo "deb [arch=arm64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
```

**Catatan:** Jika menggunakan Raspberry Pi dengan arsitektur **ARMHF (32-bit)**, ganti `[arch=arm64]` dengan `[arch=armhf]`.

### **3. Perbarui Repository dan Instal VS Code**
Setelah repository ditambahkan, jalankan:
```bash
sudo apt update
sudo apt install code -y
```

### **4. Jalankan VS Code**
Setelah instalasi selesai, jalankan **VS Code** dengan:
```bash
code
```

### **Alternatif: Instal dengan .deb Package**
Jika instalasi melalui repository tidak berhasil, gunakan metode manual:
1. **Download VS Code untuk Raspberry Pi**:
   ```bash
   wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-arm64 -O code_arm64.deb
   ```
   **Untuk 32-bit (ARMHF), gunakan**:
   ```bash
   wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-armhf -O code_armhf.deb
   ```
2. **Instal paket .deb**:
   ```bash
   sudo dpkg -i code_arm64.deb  # Untuk ARM64
   sudo dpkg -i code_armhf.deb  # Untuk ARMHF
   ```
3. **Perbaiki dependensi jika ada error**:
   ```bash
   sudo apt --fix-broken install
   ```

Setelah selesai, **VS Code** bisa dijalankan dengan perintah `code`.

Semoga berhasil! 🚀