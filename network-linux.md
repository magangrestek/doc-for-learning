

# Panduan Mengatur IP Statis di Linux

Panduan ini menjelaskan langkah-langkah untuk mengatur **IP Address statis** pada Linux menggunakan berbagai metode dan mencakup solusi untuk beberapa skenario seperti rollback, troubleshooting, dan pengaturan khusus.

---

## 1. **Cara Mengatur IP Statis (Sementara)**
IP Address sementara akan hilang setelah server di-reboot. Cocok untuk pengujian cepat atau troubleshooting.

### Langkah-langkah:
1. **Cek Interface Network:**
   ```bash
   ip addr show
   ```
   Temukan nama interface, misalnya `eth0` atau `eno1np2`.

2. **Set IP Address Sementara:**
   ```bash
   sudo ip addr add 192.168.0.50/24 dev eno1np2
   ```

3. **Set Default Gateway:**
   ```bash
   sudo ip route add default via 192.168.0.1
   ```

4. **Verifikasi:**
   ```bash
   ip addr show eno1np2
   ping 192.168.0.1
   ```

---

## 2. **Cara Mengatur IP Statis (Permanen)**

### **2.1 Ubuntu/Debian (Netplan)**
1. Edit file konfigurasi Netplan:
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. Tambahkan konfigurasi berikut:
   ```yaml
   network:
     version: 2
     ethernets:
       eno1np2:
         dhcp4: no
         addresses:
           - 192.168.0.50/24
         gateway4: 192.168.0.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
   ```

3. Terapkan konfigurasi:
   ```bash
   sudo netplan apply
   ```

4. Verifikasi:
   ```bash
   ip addr show eno1np2
   ```

### **2.2 CentOS/RHEL (Network Scripts)**
1. Edit file konfigurasi:
   ```bash
   sudo nano /etc/sysconfig/network-scripts/ifcfg-eno1np2
   ```

2. Tambahkan konfigurasi:
   ```
   DEVICE=eno1np2
   BOOTPROTO=none
   ONBOOT=yes
   IPADDR=192.168.0.50
   NETMASK=255.255.255.0
   GATEWAY=192.168.0.1
   DNS1=8.8.8.8
   DNS2=8.8.4.4
   ```

3. Restart jaringan:
   ```bash
   sudo systemctl restart network
   ```

4. Verifikasi:
   ```bash
   ip addr show eno1np2
   ```

---

## 3. **Rollback Konfigurasi Netplan**
### **3.1 Backup File Konfigurasi**
Sebelum mengedit, buat salinan file konfigurasi:
```bash
sudo cp /etc/netplan/01-netcfg.yaml /etc/netplan/01-netcfg.yaml.bak
```

### **3.2 Gunakan `netplan try`**
Sebelum menerapkan konfigurasi secara permanen:
```bash
sudo netplan try
```
- Jika berhasil, tekan `Enter` untuk mengonfirmasi.
- Jika gagal atau tidak ada respons dalam 120 detik, sistem akan otomatis rollback ke konfigurasi sebelumnya.

---

## 4. **Troubleshooting IP Statis**

### **4.1 Tidak Bisa Ping Internet (Google)**
#### **Cek Default Gateway**
1. Lihat rute aktif:
   ```bash
   ip route show
   ```
2. Tambahkan gateway jika hilang:
   ```bash
   sudo ip route add default via 192.168.1.1
   ```

#### **Cek DNS Resolver**
1. Periksa file `/etc/resolv.conf`:
   ```bash
   cat /etc/resolv.conf
   ```
2. Tambahkan nameserver jika kosong:
   ```bash
   sudo nano /etc/resolv.conf
   ```
   Tambahkan:
   ```
   nameserver 8.8.8.8
   nameserver 8.8.4.4
   ```

#### **Tes Koneksi Langsung ke IP**
Jika DNS gagal, tes ping langsung ke IP Google:
```bash
ping 8.8.8.8
```

---

## 5. **Pengaturan Khusus (Tanpa Gateway atau DNS)**
Jika tidak ingin mengatur gateway dan DNS (untuk menghindari konflik dengan internet):
1. Edit file Netplan:
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```
2. Tambahkan hanya IP Address:
   ```yaml
   network:
     version: 2
     ethernets:
       eno1np2:
         dhcp4: no
         addresses:
           - 192.168.0.50/24
   ```

3. Terapkan konfigurasi:
   ```bash
   sudo netplan apply
   ```

---

## 6. **Menyimpan Log Konfigurasi**
Setiap kali melakukan perubahan, simpan salinan konfigurasi:
```bash
sudo cp /etc/netplan/01-netcfg.yaml /etc/netplan/01-netcfg-$(date +%Y%m%d%H%M%S).yaml
```

---

## 7. **Perintah Penting**
| Perintah                    | Fungsi                                 |
|-----------------------------|----------------------------------------|
| `ip addr show`              | Menampilkan informasi interface       |
| `sudo ip addr add ...`      | Menambahkan IP Address sementara      |
| `sudo netplan apply`        | Menerapkan konfigurasi Netplan        |
| `sudo netplan try`          | Mencoba konfigurasi Netplan sementara |
| `ip route show`             | Menampilkan rute jaringan             |
| `ping <IP/Domain>`          | Tes konektivitas jaringan             |
| `nslookup <domain>`         | Cek resolusi DNS                      |

---

Dengan panduan ini, Anda dapat mengatur dan mengelola IP Address statis di Linux dengan mudah. Jika ada kendala, lakukan troubleshooting dengan langkah-langkah yang disebutkan di atas.

--- 

Berikut adalah rangkuman langkah-langkah untuk rollback konfigurasi Netplan dalam format `.md`:

---

# Rollback Konfigurasi Netplan

Netplan adalah alat untuk mengatur jaringan di sistem berbasis Ubuntu/Debian. Jika terjadi masalah setelah mengedit konfigurasi Netplan, Anda dapat dengan mudah rollback ke kondisi sebelumnya dengan langkah berikut.

---

## 1. Backup File Konfigurasi Sebelum Mengedit
Sebelum mengedit file Netplan, buat salinan file konfigurasi sebagai backup:
```bash
sudo cp /etc/netplan/01-netcfg.yaml /etc/netplan/01-netcfg.yaml.bak
```

Jika terjadi masalah, Anda dapat memulihkan file asli:
```bash
sudo cp /etc/netplan/01-netcfg.yaml.bak /etc/netplan/01-netcfg.yaml
sudo netplan apply
```

---

## 2. Gunakan `netplan try`
Sebelum menerapkan konfigurasi secara permanen, gunakan perintah `netplan try` untuk mencobanya secara sementara:
```bash
sudo netplan try
```

- Sistem akan mencoba konfigurasi selama 120 detik.
- Jika konfigurasi berhasil, tekan `Enter` untuk mengonfirmasi.
- Jika konfigurasi gagal atau Anda tidak menekan `Enter` dalam 120 detik, sistem akan otomatis rollback ke konfigurasi sebelumnya.

---

## 3. Rollback Manual
Jika konfigurasi sudah diterapkan dan ada masalah:
1. Edit ulang file Netplan untuk mengembalikan konfigurasi sebelumnya:
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. Terapkan kembali konfigurasi lama:
   ```bash
   sudo netplan apply
   ```

---

## 4. Recovery Mode (Jika Tidak Bisa Akses Server)
Jika server tidak dapat diakses:
1. Reboot server dan masuk ke **Recovery Mode** dari menu GRUB.
2. Pilih **Root Shell** untuk akses terminal.
3. Edit file Netplan:
   ```bash
   nano /etc/netplan/01-netcfg.yaml
   ```
4. Simpan perubahan dan terapkan konfigurasi:
   ```bash
   netplan apply
   ```
5. Reboot server:
   ```bash
   reboot
   ```

---

## 5. Simpan Log Konfigurasi
Untuk menghindari kehilangan konfigurasi sebelumnya, simpan setiap perubahan dengan membuat salinan file:
```bash
sudo cp /etc/netplan/01-netcfg.yaml /etc/netplan/01-netcfg-$(date +%Y%m%d%H%M%S).yaml
```

---

Dengan langkah-langkah ini, Anda dapat memastikan konfigurasi Netplan dapat di-rollback jika terjadi masalah. Jika ada kesalahan, selalu periksa kembali konfigurasi dan pastikan format YAML sesuai.

--- 

**Catatan:** Pastikan konfigurasi Netplan divalidasi sebelum diterapkan menggunakan `netplan apply`.

---




# Mengubah Alamat IP di Linux

## 1. Menggunakan Perintah `ifconfig` (Sementara)
Perubahan ini hanya bersifat sementara dan akan hilang setelah sistem di-reboot.

```bash
sudo ifconfig eno1np2 192.168.0.41 netmask 255.255.255.0
```

### Catatan:
- Ganti `eno1np2` dengan nama antarmuka jaringan Anda (dalam contoh ini `eno1np2`).
- `192.168.0.41` adalah alamat IP baru yang diinginkan.
- Pastikan alamat IP baru tidak konflik dengan perangkat lain di jaringan.

---

## 2. Mengonfigurasi IP Statis Secara Permanen
Untuk perubahan permanen, Anda perlu mengedit file konfigurasi jaringan. Langkah ini bergantung pada distribusi Linux Anda.

### a. Ubuntu/Debian dengan Netplan
1. Buka file konfigurasi Netplan:
   ```bash
   sudo nano /etc/netplan/*.yaml
   ```

2. Edit konfigurasi antarmuka:
   ```yaml
   network:
     version: 2
     ethernets:
       eno1np2:
         dhcp4: no
         addresses:
           - 192.168.0.41/24
         gateway4: 192.168.0.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
   ```

3. Terapkan konfigurasi:
   ```bash
   sudo netplan apply
   ```

---

### b. CentOS/RHEL dengan `ifcfg`
1. Edit file konfigurasi antarmuka:
   ```bash
   sudo nano /etc/sysconfig/network-scripts/ifcfg-eno1np2
   ```

2. Tambahkan atau perbarui konfigurasi berikut:
   ```
   BOOTPROTO=none
   IPADDR=192.168.0.41
   NETMASK=255.255.255.0
   GATEWAY=192.168.0.1
   DNS1=8.8.8.8
   DNS2=8.8.4.4
   ```

3. Restart layanan jaringan:
   ```bash
   sudo systemctl restart network
   ```

---

### c. Menggunakan `nmcli` (NetworkManager)
1. Set alamat IP baru:
   ```bash
   sudo nmcli con mod eno1np2 ipv4.addresses 192.168.0.41/24 ipv4.gateway 192.168.0.1 ipv4.dns "8.8.8.8,8.8.4.4" ipv4.method manual
   ```

2. Terapkan perubahan:
   ```bash
   sudo nmcli con up eno1np2
   ```

---

Dengan salah satu metode di atas, IP Anda akan berubah menjadi `192.168.0.41`.

# Network Troubleshooting Guide

This guide provides step-by-step instructions to diagnose and resolve network issues when DNS or connectivity is not functioning correctly. It focuses on environments using `systemd-networkd`.

## Steps to Troubleshoot Network Issues

### 1. Check Network Interface Status
Run the following command to verify that network interfaces are up and have IP addresses:
```bash
ip addr
```
- Look for interfaces like `eth0`, `ens33`, or `wlan0`.
- Ensure the interface has a valid IP address assigned.

### 2. Verify Default Gateway
Check the routing table to confirm there is a valid default gateway:
```bash
ip route
```
Expected output should include a line like:
```
default via <gateway-ip> dev <interface>
```
If missing, add it manually:
```bash
sudo ip route add default via <gateway-ip>
```

### 3. Test Connectivity
#### Ping an External IP
Test if the system can reach an external IP (e.g., Google DNS):
```bash
ping 8.8.8.8
```
- Success indicates internet connectivity is functional.
- Failure suggests an issue with the network or gateway.

#### Test DNS Resolution
Check if DNS resolution works:
```bash
nslookup google.com
```
- Success confirms DNS is functioning.
- Failure points to a DNS configuration issue.

### 4. Verify and Update DNS Configuration
Check the contents of `/etc/resolv.conf`:
```bash
cat /etc/resolv.conf
```
Ensure it contains:
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```
If it’s missing or incorrect, update it:
```bash
sudo nano /etc/resolv.conf
```
Add the following lines:
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

#### Prevent Overwrite by Systemd
Prevent `systemd-resolved` from overwriting:
```bash
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

### 5. Restart Networking Services
Restart the `systemd-networkd` service to apply changes:
```bash
sudo systemctl restart systemd-networkd
```
Verify the status:
```bash
sudo systemctl status systemd-networkd
```

### 6. Disable Firewall (Temporary Test)
Check if the firewall is blocking traffic:
```bash
sudo ufw disable
sudo iptables -F
```
If connectivity works after this, reconfigure the firewall to allow DNS (port 53):
```bash
sudo ufw allow 53
sudo ufw enable
```

### 7. Check Logs for Errors
Inspect logs for networking-related errors:
```bash
sudo journalctl -u systemd-networkd
```
Look for warnings or errors related to network configuration.

### 8. Verify After Changes
- Ping external IP:
  ```bash
  ping 8.8.8.8
  ```
- Ping a domain:
  ```bash
  ping google.com
  ```

## Common Fixes
### No Default Gateway
If no default gateway is set, use:
```bash
sudo ip route add default via <gateway-ip>
```

### Missing or Incorrect DNS
Manually edit `/etc/resolv.conf` and restart the networking service:
```bash
sudo nano /etc/resolv.conf
sudo systemctl restart systemd-networkd
```

### Persistent DNS Overwrite
Link `systemd-resolved` configuration:
```bash
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## Commands Reference
| Command                                  | Description                            |
|------------------------------------------|----------------------------------------|
| `ip addr`                                | Show interface IP addresses           |
| `ip route`                               | Show routing table                    |
| `ping <IP or domain>`                    | Test connectivity                     |
| `nslookup <domain>`                      | Test DNS resolution                   |
| `sudo systemctl restart systemd-networkd`| Restart networking service            |
| `sudo journalctl -u systemd-networkd`    | Check networking logs                 |

## Additional Notes
- Ensure that network cables are connected if using Ethernet.
- Check Wi-Fi configuration if using a wireless connection.
- Consult your network administrator if the issue persists.

---
