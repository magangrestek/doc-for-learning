# Tutorial PgAgent: Membuat dan Mengelola Jobs

## Persiapan PgAgent
1. Pastikan pgAgent sudah terinstall:
```bash
sudo apt-get install postgresql-*-pgagent
```

2. Buat extension di database:
```sql
CREATE EXTENSION IF NOT EXISTS pgagent;
```

3. Setup service pgAgent:
```bash
sudo nano /etc/systemd/system/pgagent.service
```

Isi dengan konfigurasi:
```ini
[Unit]
Description=PgAgent for PostgreSQL
After=postgresql.service

[Service]
Type=simple
User=postgres
ExecStart=/usr/bin/pgagent -f -l 1 host=localhost port=5432 dbname=postgres user=postgres
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```

4. Aktifkan service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable pgagent
sudo systemctl start pgagent
```

## Membuat Job Melalui SQL

1. Membuat Job Baru:
```sql
INSERT INTO pgagent.pga_job(
    jobjclid,
    jobname,
    jobdesc,
    jobhostagent,
    jobenabled
) VALUES (
    1::integer,
    'NamaJob',
    'Deskripsi job Anda',
    '',
    true
) RETURNING jobid;
```

2. Menambahkan Step ke Job:
```sql
INSERT INTO pgagent.pga_jobstep(
    jstjobid,
    jstname,
    jstenabled,
    jstkind,
    jstcode,
    jstdbname
) VALUES (
    (SELECT jobid FROM pgagent.pga_job WHERE jobname = 'NamaJob'),
    'Step1',
    true,
    'sql'::character varying(5),
    'SELECT now();', -- Ganti dengan SQL yang diinginkan
    'nama_database'
);
```

3. Menambahkan Schedule:
```sql
INSERT INTO pgagent.pga_schedule(
    jscjobid,
    jscname,
    jscenabled,
    jscstart,    -- Waktu mulai
    jscminutes,  -- Array menit (NULL = setiap menit)
    jschours,    -- Array jam (NULL = setiap jam)
    jscweekdays, -- Array hari (NULL = setiap hari)
    jscmonthdays -- Array tanggal (NULL = setiap tanggal)
) VALUES (
    (SELECT jobid FROM pgagent.pga_job WHERE jobname = 'NamaJob'),
    'Schedule1',
    true,
    '2024-12-19 00:00:00'::timestamp,
    '{0,15,30,45}'::integer[], -- Contoh: jalankan setiap 15 menit
    '{9,12,15}'::integer[],    -- Contoh: jalankan pada jam 9, 12, dan 15
    '{1,2,3,4,5}'::integer[],  -- Contoh: jalankan Senin-Jumat
    NULL                       -- Jalankan setiap tanggal
);
```

## Membuat Job Melalui pgAdmin

1. Di pgAdmin, expand server dan database
2. Klik kanan pada "pgAgent Jobs"
3. Pilih "Create" > "pgAgent Job"
4. Isi form dengan:
   - Name: Nama job
   - Enabled: Centang untuk mengaktifkan
   - Job Class: Pilih class yang sesuai
   - Host Agent: Kosongkan untuk semua host

5. Di tab Steps:
   - Klik + untuk menambah step
   - Isi nama step
   - Pilih tipe (SQL atau batch)
   - Masukkan kode SQL
   - Atur "On Error" action

6. Di tab Schedule:
   - Klik + untuk menambah schedule
   - Atur waktu mulai
   - Pilih frekuensi (menit, jam, hari, bulan)
   - Atur Exception (jika perlu)

## Monitoring Jobs

1. Melihat status job:
```sql
SELECT jobid, jobname, jobenabled, jobstatus 
FROM pgagent.pga_job;
```

2. Melihat log job:
```sql
SELECT jlgid, jlgjobid, jlgstatus, jlgstart, jlgduration 
FROM pgagent.pga_joblog 
ORDER BY jlgid DESC 
LIMIT 10;
```

3. Melihat detail step:
```sql
SELECT jsljlgid, jslstatus, jslresult, jslstart, jslduration 
FROM pgagent.pga_jobsteplog 
WHERE jsljlgid IN (
    SELECT jlgid 
    FROM pgagent.pga_joblog 
    ORDER BY jlgid DESC 
    LIMIT 1
);
```

## Tips dan Troubleshooting

1. Selalu test job dalam lingkungan development terlebih dahulu
2. Mulai dengan schedule sederhana sebelum menggunakan yang kompleks
3. Gunakan exception untuk mengatur jadwal khusus (libur, maintenance)
4. Monitor log secara regular
5. Atur notifikasi untuk job yang gagal
6. Backup konfigurasi job secara regular

## Perintah Berguna

1. Enable/Disable Job:
```sql
UPDATE pgagent.pga_job 
SET jobenabled = true/false 
WHERE jobname = 'NamaJob';
```

2. Hapus Job:
```sql
DELETE FROM pgagent.pga_job 
WHERE jobname = 'NamaJob';
```

3. Update Schedule:
```sql
UPDATE pgagent.pga_schedule 
SET jscenabled = true/false 
WHERE jscjobid = (
    SELECT jobid 
    FROM pgagent.pga_job 
    WHERE jobname = 'NamaJob'
);
```