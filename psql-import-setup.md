# Tutorial PostgreSQL: Manajemen Database dan Import Data

## Daftar Isi
1. [Membuat Database Baru](#membuat-database-baru)
2. [Mengimport Database](#mengimport-database)
3. [Mengelola Permission](#mengelola-permission)
4. [Troubleshooting Umum](#troubleshooting-umum)

## Membuat Database Baru
1. Login ke PostgreSQL:
```bash
psql -U postgres
```

2. Membuat database baru:
```sql
CREATE DATABASE nama_database;
```

3. Verifikasi database telah dibuat:
```sql
\l
```

## Mengimport Database
1. Import dari file SQL yang ada di /home:
```bash
psql -U postgres -d nama_database -f /home/path/ke/file.sql
```

Atau jika sudah di dalam psql:
```sql
\i /home/path/ke/file.sql
```

2. Verifikasi import berhasil:
```sql
\dt
```

## Mengelola Permission
1. Memberikan akses penuh ke user:
```sql
GRANT ALL PRIVILEGES ON DATABASE nama_database TO nama_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO nama_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO nama_user;
```

2. Memberikan permission untuk schema:
```sql
GRANT USAGE ON SCHEMA nama_schema TO nama_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA nama_schema TO nama_user;
```

3. Setting default privileges:
```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
GRANT ALL ON TABLES TO nama_user;
```

## Troubleshooting Umum

### Error Permission Denied
Jika mengalami error permission denied:
1. Verifikasi user privileges:
```sql
SELECT usename, usesuper FROM pg_user WHERE usename = current_user;
```

2. Reset ownership jika perlu:
```sql
ALTER TABLE nama_tabel OWNER TO nama_user;
```

### Tips Penting
1. Selalu backup database sebelum melakukan import
2. Verifikasi permission setelah import
3. Gunakan superuser dengan hati-hati
4. Pastikan path file SQL benar sebelum import

## Perintah Berguna

### Database Management
- Melihat daftar database: `\l`
- Pindah database: `\c nama_database`
- Melihat daftar tabel: `\dt`
- Keluar dari psql: `\q`

### User Management
- Membuat user baru: `CREATE USER nama_user WITH PASSWORD 'password';`
- Memberikan superuser: `ALTER USER nama_user WITH SUPERUSER;`
- Melihat daftar user: `\du`

## Kesimpulan
Tutorial ini mencakup langkah-langkah dasar untuk:
1. Membuat database baru di PostgreSQL
2. Mengimport data dari file SQL
3. Mengelola permission dan user
4. Menangani masalah umum yang mungkin terjadi

Pastikan untuk selalu mengikuti praktik keamanan yang baik dan melakukan backup sebelum melakukan operasi penting pada database.