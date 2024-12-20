# Tutorial Mengatasi Error Autentikasi GitHub dan Setup SSH Key

## Pendahuluan
Sejak 13 Agustus 2021, GitHub telah menghentikan autentikasi menggunakan password untuk operasi Git. Ada dua metode utama yang direkomendasikan:
1. Personal Access Token (PAT)
2. SSH Key Authentication

## Bagian 1: Personal Access Token (PAT)

### Membuat Personal Access Token (PAT)
1. Buka browser dan kunjungi [GitHub.com](https://github.com)
2. Login ke akun GitHub Anda
3. Klik foto profil Anda di pojok kanan atas
4. Pilih "Settings" dari menu dropdown
5. Scroll ke bawah dan klik "Developer settings" (ada di sidebar paling bawah)
6. Klik "Personal access tokens" di sidebar kiri
7. Pilih "Tokens (classic)"
8. Klik tombol "Generate new token (classic)"
9. Di halaman pembuatan token:
   - Isi "Note" dengan nama yang menjelaskan penggunaan token (misal: "Access for local development")
   - Pilih "Expiration" sesuai kebutuhan
   - Centang scope yang diperlukan (minimal centang 'repo' untuk akses repository)
   - Scroll ke bawah dan klik "Generate token"
10. **PENTING**: Copy token yang muncul dan simpan di tempat yang aman. Token ini hanya akan ditampilkan sekali!

### Menggunakan Personal Access Token

#### Metode 1: Menggunakan Token Langsung dalam URL
```bash
git clone https://USERNAME:YOUR_PAT@github.com/username/repository.git
```
Ganti:
- USERNAME dengan username GitHub Anda
- YOUR_PAT dengan token yang baru dibuat
- username/repository.git dengan repository yang ingin diakses

#### Metode 2: Menyimpan Kredensial
1. Buka terminal atau command prompt
2. Jalankan perintah:
```bash
git config --global credential.helper store
```
3. Lakukan operasi Git (clone/push/pull)
4. Saat diminta kredensial:
   - Username: masukkan username GitHub Anda
   - Password: masukkan Personal Access Token (bukan password GitHub)

## Bagian 2: SSH Key Authentication

### Penjelasan RSA
RSA adalah algoritma kriptografi yang digunakan untuk mengamankan komunikasi. Nama RSA diambil dari nama penemunya (Rivest–Shamir–Adleman).
Dalam konteks SSH key:
- RSA menghasilkan sepasang kunci: public key dan private key
- Private key disimpan di komputer/server kita
- Public key diberikan ke GitHub
- Keduanya bekerja berpasangan untuk autentikasi yang aman

### Standar RSA Key
1. Panjang Bit Key:
   - Minimum: 2048 bit
   - Rekomendasi: 4096 bit (lebih aman)
   - Command: `ssh-keygen -t rsa -b 4096`
   - rsa example mg23404
2. Format yang valid:
   ```
   ssh-rsa AAAAB3NzaC1....[base64].... email@example.com
   ```

### Standar Email
1. Email harus:
   - Terdaftar di GitHub
   - Valid (bisa diakses)
   - Format: username@domain.com
2. Best Practice:
   - Gunakan email yang sama dengan GitHub
   - Jangan gunakan email temporary
   - Hindari email dengan karakter khusus

### Langkah-Langkah Setup SSH Key
```bash
# 1. Buat direktori .ssh
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# 2. Generate SSH key baru
ssh-keygen -t rsa -b 4096 -C "email@gmail.com"
# Tekan Enter untuk lokasi default
# Tekan Enter 2x untuk skip passphrase

# 3. Lihat public key
cat ~/.ssh/id_rsa.pub

# 4. Copy ke GitHub
# Settings -> SSH Keys -> New SSH Key

# 5. Test koneksi
ssh -T git@github.com

# 6. Set git config
git config --global user.name "username_github"
git config --global user.email "email@gmail.com"
```

### Notes Penting untuk SSH Key
1. JANGAN PERNAH:
   - Share private key (~/.ssh/id_rsa)
   - Upload private key ke internet
   - Gunakan key yang sama di banyak server

2. SELALU:
   - Backup private key
   - Gunakan passphrase di environment public
   - Ganti key jika dicurigai terekspos

3. Tips Keamanan:
   - Generate key baru untuk setiap server
   - Hapus key yang tidak digunakan
   - Review key aktif secara berkala

## Troubleshooting

### Error yang Mungkin Muncul

1. "Authentication failed":
   - Pastikan token belum expired (jika menggunakan PAT)
   - Verifikasi scope token sudah sesuai
   - Cek username dan token tidak ada typo
   - Pastikan SSH key sudah terdaftar di GitHub (jika menggunakan SSH)

2. "Permission denied":
   - Pastikan Anda punya akses ke repository
   - Verifikasi scope token mencakup akses yang diperlukan
   - Cek permission SSH key di GitHub

3. Token atau SSH key tidak bekerja:
   - Buat token atau SSH key baru
   - Hapus kredensial yang tersimpan:
     ```bash
     git config --global --unset credential.helper
     ```

## Kesimpulan
Dengan mengikuti langkah-langkah di atas, Anda seharusnya sudah bisa mengatasi masalah autentikasi GitHub dan melanjutkan development dengan lancar. Pastikan untuk memilih metode autentikasi yang sesuai dengan kebutuhan Anda:
- PAT lebih cocok untuk penggunaan temporary atau di environment yang berbeda-beda
- SSH Key lebih cocok untuk penggunaan jangka panjang dan lebih aman untuk development sehari-hari

Jika masih mengalami masalah, pastikan untuk memeriksa dokumentasi resmi GitHub atau menghubungi support GitHub.