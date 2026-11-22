<!-- prettier-ignore -->
<div align="center">

<img src="./public/android-chrome-512x512.png" alt="Logo e-Surat Desa" height="96" />

# e-Surat Desa

**Sistem tata kelola persuratan dan administrasi digital terpadu untuk Pemerintah Desa, Kelurahan, Gampong, dan Nagari.**

[![Tests](https://github.com/diskonnekted/e-surat-desa/actions/workflows/tests.yml/badge.svg)](https://github.com/diskonnekted/e-surat-desa/actions/workflows/tests.yml)
![PHP](https://img.shields.io/badge/PHP-%5E8.3-777BB4?style=flat-square&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-13-FF2D20?style=flat-square&logo=laravel&logoColor=white)
![Vue](https://img.shields.io/badge/Vue-3.5-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![Node](https://img.shields.io/badge/Node-26-339933?style=flat-square&logo=nodedotjs&logoColor=white)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](https://opensource.org/licenses/MIT)

[Ikhtisar](#ikhtisar) • [Struktur & Tata Kelola Desa](#struktur--tata-kelola-desa) • [Fitur](#fitur) • [Memulai](#memulai) • [Arsitektur](#arsitektur) • [Konfigurasi](#konfigurasi) • [Testing](#testing) • [Deployment](#deployment) • [Security](#security)

</div>

**e-Surat Desa** dirancang khusus untuk memodernisasi tata kelola persuratan dinas di tingkat pemerintah desa. Aplikasi ini mencakup siklus lengkap administrasi persuratan: registrasi surat masuk dari kecamatan/kabupaten/warga, penomoran agenda otomatis, disposisi berjenjang dari Kepala Desa ke Sekretaris Desa hingga Kepala Seksi (Kasi) / Kepala Urusan (Kaur), pembuatan nota dinas, surat keluar, arsip digital sesuai kode klasifikasi kearsipan desa, serta notifikasi instan untuk seluruh perangkat desa.

Dibangun dengan fondasi performa tinggi menggunakan **Laravel 13 + Inertia 3 + Vue 3 (TypeScript) + Tailwind CSS**.

> [!TIP]
> Ingin mencoba cepat di lokal? Jalankan `composer setup` lalu `composer dev`. Aplikasi dapat diakses langsung di `http://127.0.0.1:8000`.

---

## Ikhtisar

Alur tata kelola persuratan pemerintah desa:

1. **Registrasi & Penerimaan Surat (Surat Masuk)** — Surat resmi dari instansi luar (Kecamatan, Kabupaten, Dinas terkait) atau undangan warga/lembaga dicatat dengan penomoran agenda otomatis (`RefAgendaSurat`), metadata TNDE, dan pengunggahan berkas lampiran privat.
2. **Distribusi & Disposisi Kepala Desa** — Surat masuk diteruskan langsung ke meja Kepala Desa / Lurah. Kepala Desa memberikan instruksi disposisi (tindakan/tindak lanjut, catatan khusus, dan batas waktu) kepada Sekretaris Desa (Carik) atau langsung ke Kasi/Kaur terkait.
3. **Tindak Lanjut Perangkat Desa** — Perangkat desa penerima disposisi menindaklanjuti instruksi, menambahkan riwayat koordinasi, membuat nota dinas, atau menyusun draf surat keluar balasan.
4. **Penyusunan Surat Keluar (Compose)** — Surat keluar desa (surat dinas, surat edaran, undangan musrenbang, surat tugas) disusun dengan klasifikasi baku, diverifikasi, dan diterbitkan nomor agendanya secara otomatis.
5. **Pengarsipan Digital** — Setelah seluruh proses selesai, surat ditutup (`close`) dan masuk ke modul Arsip Digital yang dilengkapi riwayat audit, rekam jejak aktivitas, dan pencarian dokumen cepat.

---

## Struktur & Tata Kelola Desa

e-Surat Desa disesuaikan dengan struktur organisasi tata kerja (SOTK) pemerintah desa:

- **Kepala Desa / Lurah**: Menerima surat masuk tingkat pimpinan, memberikan arahan disposisi, menyetujui nota dinas dan surat keluar.
- **Sekretaris Desa (Carik)**: Verifikator administrasi umum, pengendali agenda surat masuk/keluar, dan pengelola distribusi disposisi.
- **Kaur (Kepala Urusan)**:
  - *Kaur Tata Usaha & Umum*: Administrasi persuratan, kearsipan desa, dan ekspedisi surat.
  - *Kaur Keuangan*: Persuratan terkait penatausahaan APBDes dan administrasi keuangan desa.
  - *Kaur Perencanaan*: Persuratan perencanaan desa (RPJMDes, RKPDes, Musrenbangdes).
- **Kasi (Kepala Seksi)**:
  - *Kasi Pemerintahan*: Administrasi kependudukan, pertanahan, ketenteraman dan ketertiban.
  - *Kasi Kesejahteraan*: Pengelolaan program bantuan sosial, pemberdayaan masyarakat, dan kesehatan/posyandu.
  - *Kasi Pelayanan*: Pelayanan surat pengantar dan perizinan warga desa.
- **Kepala Dusun (Dukuh / Kamituwa)**: Menerima tembusan surat, undangan dinas, dan sosialisasi wilayah dusun.
- **Badan Permusyawaratan Desa (BPD) & Lembaga Desa**: Koordinasi surat masuk/keluar kelembagaan (BPD, RT, RW, Karang Taruna, PKK).

---

## Fitur Utama

- **Penyusunan Surat Keluar Desa** — Penomoran agenda dinas otomatis, pengelompokan berdasarkan kode klasifikasi kearsipan (Permendagri/Pemda), integrasi draf lampiran, riwayat aktivitas penulisan, dan fleksibilitas revisi per bagian (isi, penerima, berkas).
- **Kotak Masuk Digital (Inbox Desa)** — Pemisahan surat masuk kedinasan reguler dan surat undangan kegiatan/musyawarah desa, filter status baca, pencarian kata kunci, serta penandaan waktu baca (`read_at`).
- **Disposisi Elektronik** — Disposisi berjenjang maupun khusus antar-perangkat desa, instruksi tindak lanjut terstandar, catatan khusus, dan rekam jejak timeline transparan.
- **Nota Dinas Internal** — Jalur komunikasi kedinasan internal antar-bagian di lingkungan kantor desa, penutupan status surat (`close`), dan dokumentasi rapat koordinasi.
- **Arsip Digital Desa** — Arsip surat berstatus selesai dengan perlindungan akses (read-only), metadata tata naskah dinas elektronik (TNDE), dan penyimpanan dokumen privat yang aman.
- **Notifikasi Multi-Kanal** — Notifikasi kedinasan terintegrasi ke Database (web notification bell), Web Push browser, Email dinas, hingga bot **Telegram** untuk memudahkan perangkat desa yang bekerja di lapangan.
- **Master Data Pemerintahan Desa** — Pengelolaan data perangkat desa, jabatan SOTK desa, role & permission berbasis RBAC, daftar kode klasifikasi surat, serta jenis instruksi follow-up.
- **Dashboard Statistik Persuratan** — Ringkasan metrik volume surat masuk/keluar, kecepatan respon disposisi, dan status surat terbuka secara real-time.
- **Keamanan & Autentikasi** — Didukung Laravel Fortify dengan autentikasi kata sandi terenkripsi, Two-Factor Authentication (2FA - TOTP & recovery codes), serta dukungan Passkeys (biometrik/FIDO2).

---

## Tech Stack

| Lapisan       | Teknologi                                                                                                                |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Backend       | PHP ^8.3 (mendukung PHP 8.4), Laravel ^13, Inertia ^3, Laravel Fortify, spatie/laravel-permission, Ziggy                 |
| Frontend      | Vue 3.5 (Composition API + `<script setup>`), TypeScript, Inertia Vue, Tailwind CSS 4, reka-ui, Unovis, GSAP, vue-sonner |
| Build         | Vite+ (`vp`); `vite` di-override ke `@voidzero-dev/vite-plus-core`                                                        |
| Database      | Default SQLite (cocok untuk server mandiri desa); MySQL/MariaDB dan PostgreSQL didukung penuh lewat `.env`              |
| Queue         | Database queue untuk pemrosesan notifikasi latar belakang                                                                |
| Notifikasi    | Database, Web Push (VAPID), Email, dan Telegram Bot                                                                      |
| Pengujian     | Pest 4, Larastan (PHPStan), Laravel Pint, `vue-tsc`                                                                       |

---

## Struktur Proyek

```text
.
├── .github/              # Workflow CI, Dependabot, template issue/PR
├── app/
│   ├── Actions/          # Logika bisnis per domain: Letter, Memo, Disposition, Notification, MasterData
│   ├── Http/             # Controller, FormRequest, middleware otorisasi & role
│   ├── Models/           # Eloquent Models (tabel persuratan desa)
│   ├── Notifications/    # Notifikasi surat, disposisi, dan channel Telegram
│   └── Services/         # Manajemen storage privat, cache badge, dan notification center
├── routes/               # web.php dan modul per fitur persuratan
├── resources/js/         # Halaman Inertia Vue 3, komponen UI desa, composable, tipe TypeScript
├── database/             # Migrasi struktur database dan seeder data awal / demo
├── tests/                # Test suite Pest (Feature & Unit)
├── docs/                 # Panduan konfigurasi, deployment, dan runbook
└── public/               # Web root; service worker web push
```

---

## Memulai

### Prasyarat Sistem

- **PHP**: Versi **^8.3** atau **8.4** dengan ekstensi: `pdo_sqlite` / `pdo_mysql`, `curl`, `mbstring`, `openssl`, `fileinfo`, `gd`, `intl`, `zip`.
- **Composer**: Composer 2.x.
- **Node.js & npm**: Node.js versi 20+ (rekomendasi LTS / v24+).
- **Database**: SQLite (bawaan tanpa instalasi DBMS tambahan) atau MySQL/MariaDB.

### Instalasi Cepat

```bash
# 1. Clone repositori
git clone https://github.com/diskonnekted/e-surat-desa.git
cd e-surat-desa

# 2. Setup dependensi, database, dan build frontend
composer setup

# 3. (Opsional) Jalankan data contoh perangkat desa
php artisan db:seed --class=DemoDataSeeder

# 4. Jalankan aplikasi lokal
composer dev
```

Aplikasi siap dibuka di peramban web: **`http://127.0.0.1:8000`**.

> [!WARNING]
> `DatabaseSeeder` membuat akun awal `superadmin` dengan kata sandi bawaan `password`. Segera ubah kata sandi di menu Pengaturan Keamanan setelah login pertama.

> [!NOTE]
> Akun demo yang tersedia setelah menjalankan `DemoDataSeeder`:
> - `pengelola` (Password: `password`)
> - `pengguna1` (Password: `password`)
> - `pengguna2` (Password: `password`)

---

## Menjalankan Server Lokal

```powershell
# Mode lengkap (Web server + Worker Notifikasi + Vite Hot-Reload):
composer dev

# Atau hanya web server Laravel:
php artisan serve
```

---

## Konfigurasi Penting

Salin berkas `.env.example` menjadi `.env`. Pengaturan umum yang dapat disesuaikan:

| Variabel | Penjelasan |
| --- | --- |
| `APP_NAME` | Nama aplikasi (contoh: `e-Surat Desa Trimulyo`) |
| `APP_URL` | Alamat web aplikasi (contoh: `http://localhost:8000` atau domain desa) |
| `COMPANY_KEY` | Kode identitas desa / kalurahan |
| `COMPANY_FEATURES` | Fitur opsional aktif: `email`, `telegram`, `webpush` (contoh: `telegram,webpush`) |
| `DB_CONNECTION` | Driver database: `sqlite` atau `mysql` |
| `TELEGRAM_BOT_TOKEN` | Token Bot Telegram desa untuk pengiriman notifikasi surat masuk/disposisi |

---

## Pengujian Kualitas Kode

```bash
# Menjalankan seluruh test suite Pest
php artisan test

# Menjalankan pengecekan tipe statis & linting
composer lint:check
composer types:check
```

---

## Lisensi

Aplikasi ini dilisensikan di bawah lisensi terbuka [MIT License](LICENSE). Bebas digunakan, dikembangkan, dan disesuaikan untuk kebutuhan tata kelola desa di seluruh Indonesia.
