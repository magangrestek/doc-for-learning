# **Aplikasi PHP + MySQL**
Panduan ini menjelaskan langkah-langkah untuk menginstal dan menjalankan aplikasi berbasis **PHP** dan **MySQL** di Ubuntu.

---

## **1. Instalasi Apache, PHP, dan MySQL**
```bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php php-mysql mysql-server -y
```
### **1.1. Aktifkan dan Jalankan Apache & MySQL**
```bash
sudo systemctl enable apache2
sudo systemctl enable mysql
sudo systemctl start apache2
sudo systemctl start mysql
```

---

## **2. Konfigurasi MySQL**
Jalankan buat database dan user:
```sql
CREATE DATABASE myapp_db;
CREATE USER 'myapp_user'@'localhost' IDENTIFIED BY 'passwordku';
GRANT ALL PRIVILEGES ON myapp_db.* TO 'myapp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## **3. Setup Proyek PHP**
```bash
cd /var/www/html/
sudo mkdir myapp
sudo chown -R $USER:$USER myapp
cd myapp
```

Buat file **index.php**:
```bash
nano index.php
```
Tambahkan:
```php
<?php
echo "Aplikasi PHP & MySQL berhasil berjalan!";
?>
```
Akses di browser:
```
http://localhost/myapp/index.php
```

---

## **4. Koneksi PHP ke MySQL**
Buat **config.php**:
```bash
nano config.php
```
Isi:
```php
<?php
$host = "localhost";
$user = "myapp_user";
$password = "passwordku";
$database = "myapp_db";

$conn = new mysqli($host, $user, $password, $database);

if ($conn->connect_error) {
    die("Koneksi gagal: " . $conn->connect_error);
}
?>
```

**Uji Koneksi** dengan **test_db.php**:
```bash
nano test_db.php
```
Isi:
```php
<?php
include 'config.php';

$sql = "SHOW TABLES";
$result = $conn->query($sql);

if ($result) {
    echo "Koneksi ke database berhasil!";
} else {
    echo "Koneksi gagal: " . $conn->error;
}
?>
```
Akses di browser:
```
http://localhost/myapp/test_db.php
```

---

## **5. CRUD (Create, Read, Update, Delete)**

### **5.1. Buat Tabel**
Masuk ke MySQL:
```bash
sudo mysql -u myapp_user -p myapp_db
```
Jalankan SQL:
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **5.2. Insert Data**
Buat **insert.php**:
```php
<?php
include 'config.php';

$name = "John Doe";
$email = "john@example.com";

$sql = "INSERT INTO users (name, email) VALUES ('$name', '$email')";

if ($conn->query($sql) === TRUE) {
    echo "Data berhasil ditambahkan!";
} else {
    echo "Error: " . $conn->error;
}

$conn->close();
?>
```
Akses di browser:
```
http://localhost/myapp/insert.php
```

### **5.3. Read Data**
Buat **read.php**:
```php
<?php
include 'config.php';

$sql = "SELECT * FROM users";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        echo "ID: " . $row["id"] . " - Nama: " . $row["name"] . " - Email: " . $row["email"] . "<br>";
    }
} else {
    echo "Tidak ada data.";
}

$conn->close();
?>
```
Akses di browser:
```
http://localhost/myapp/read.php
```

### **5.4. Update Data**
Buat **update.php**:
```php
<?php
include 'config.php';

$id = 1;
$new_name = "Jane Doe";

$sql = "UPDATE users SET name='$new_name' WHERE id=$id";

if ($conn->query($sql) === TRUE) {
    echo "Data berhasil diperbarui!";
} else {
    echo "Error: " . $conn->error;
}

$conn->close();
?>
```
Akses di browser:
```
http://localhost/myapp/update.php
```

### **5.5. Delete Data**
Buat **delete.php**:
```php
<?php
include 'config.php';

$id = 1;

$sql = "DELETE FROM users WHERE id=$id";

if ($conn->query($sql) === TRUE) {
    echo "Data berhasil dihapus!";
} else {
    echo "Error: " . $conn->error;
}

$conn->close();
?>
```
Akses di browser:
```
http://localhost/myapp/delete.php
```

---

## **6. Konfigurasi Apache untuk Akses Eksternal**
Aktifkan agar Apache berjalan otomatis:
```bash
sudo systemctl enable apache2
```
Buka akses firewall:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```
Akses aplikasi dari luar jaringan:
```
http://<IP-Server>/myapp/
```

---

## **7. Keamanan dan Best Practices**
- Gunakan **Prepared Statements** untuk mencegah **SQL Injection**.
- Simpan kredensial database di **.env** atau konfigurasi aman.
- Update **Apache, PHP, dan MySQL** secara berkala.

---

Dengan langkah-langkah ini, aplikasi PHP + MySQL Anda siap digunakan! 🚀  
Jika ada pertanyaan atau kendala, silakan beri tahu. 😊