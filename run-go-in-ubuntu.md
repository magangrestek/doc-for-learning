# Setup Guide untuk Project Golang di Ubuntu 22.04

## Prasyarat

Sebelum memulai, pastikan sistem Anda sudah memiliki:
- Go (Golang) terinstall
- Git terinstall
- pkg-config terinstall

## Struktur Project
Project ini memiliki struktur folder sebagai berikut:
```
.
├── .docker/
├── .jetclient/
├── .run/
├── .warp/
├── cmd/
├── embeds/
├── internal/
├── mocks/
├── params/
├── queries/
├── scripts/
├── variable/
├── .editorconfig
├── .gitignore
├── .golangci.yml
├── .goreleaser.yaml
├── docker-compose.yml
├── go.mod
├── go.sum
├── godepgraph.png
├── godepgraph.svg
├── main.go
├── Makefile
└── README.md
```

## Langkah-langkah Setup

### 1. Install Dependencies Sistem
Jalankan perintah berikut untuk menginstall library yang dibutuhkan:

```bash
# Update package list
sudo apt-get update

# Install FFmpeg dependencies
sudo apt-get install -y ffmpeg libavcodec-dev libavformat-dev libavdevice-dev \
  libavutil-dev libswscale-dev libswresample-dev libavfilter-dev

# Install libvips untuk image processing
sudo apt-get install -y libvips-dev
```

### 2. Setup Project

#### Inisialisasi Go Module
```bash
# Download dependencies
go mod tidy
```

### 3. Menjalankan Aplikasi

#### Cara 1: Menggunakan Go secara langsung
```bash
go run main.go
```

#### Cara 2: Menggunakan Makefile (jika tersedia)
```bash
make run
```

#### Cara 3: Menggunakan Docker
```bash
docker-compose up --build
```

## Troubleshooting

### Error: Package Not Found in pkg-config
Jika mendapatkan error seperti:
```
Package libavutil was not found in the pkg-config search path.
Package libvips was not found in the pkg-config search path.
```
Solusinya adalah menginstall dependencies yang hilang dengan perintah di bagian "Install Dependencies Sistem".

### Port Sudah Terpakai
Jika mendapatkan error port sudah digunakan:
1. Cek proses yang menggunakan port tersebut:
   ```bash
   sudo lsof -i :[nomor-port]
   ```
2. Hentikan proses tersebut atau ubah konfigurasi port di aplikasi

## Bantuan
Jika mengalami masalah atau butuh bantuan lebih lanjut:
1. Cek dokumentasi di `README.md` project
2. Jalankan `make help` (jika tersedia)
3. Buka issue di repository project