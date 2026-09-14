# 🏛️ ATHENA - Sistem PPDB & Verifikasi Siswa SMK Tunas Harapan

**Athena** adalah platform Penerimaan Peserta Didik Baru (PPDB) terintegrasi untuk **SMK Tunas Harapan Pati**. Sistem ini mencakup pendaftaran online multi-step, pengiriman bukti pendaftaran berupa QR Code via Email otomatis, dashboard admin, verifikasi daftar ulang berbasis live camera QR scanner, dan integrasi webhook WhatsApp bot.

---

## 🏗️ Arsitektur Proyek

Proyek ini terdiri dari 3 modul utama:

| Direktori | Teknologi | Deskripsi |
| :--- | :--- | :--- |
| **`api/`** | Laravel (PHP), MySQL, Sanctum, SMTP Mailer | RESTful Backend API untuk manajemen data pendaftar, autentikasi admin, dan pengiriman email QR Code. |
| **`client/`** | React.js, Vite, Tailwind CSS, Html5Qrcode | Frontend SPA untuk form pendaftaran calon siswa dan Dashboard Admin dengan Scanner Kamera. |
| **`webhook/`** | Node.js, Express, Axios | Microservice webhook untuk integrasi bot pesan WhatsApp (Wazapbro API). |

---

## ✨ Fitur Utama

- **Pendaftaran Calon Siswa (Multi-step Form):** Form pendaftaran interaktif 5 langkah (Data Pribadi, Tempat/Tanggal Lahir, Kontak & Agama, Data Orang Tua, Asal Sekolah SMP/MTs & Pilihan Jurusan).
- **Pengiriman QR Code via Email:** Setelah mendaftar, calon siswa menerima email otomatis berisi bukti pendaftaran dan QR Code unik untuk verifikasi fisik di sekolah.
- **Dashboard Admin:**
  - Ringkasan pendaftar terbaru & data statistik.
  - Tabel kelola seluruh data calon siswa beserta filter status verifikasi & pencarian nama.
- **Scanner Kamera QR Code (Real-Time):** Fitur pemindaian QR Code menggunakan kamera perangkat untuk verifikasi daftar ulang siswa secara instan di sekolah.
- **WhatsApp Webhook Bot:** Layanan webhook perantara untuk tanya-jawab informasi seputar pendaftaran via WhatsApp.

---

## 🚀 Panduan Instalasi & Menjalankan Aplikasi

### 📋 Prasyarat Sistem
- **PHP** >= 8.2 & **Composer**
- **Node.js** >= 18.x & **npm**
- **MySQL / MariaDB** (misal via XAMPP atau Laragon)

---

### 1. Setup Backend API (`api/`)

1. Masuk ke direktori `api`:
   ```bash
   cd api
   ```
2. Install dependensi PHP:
   ```bash
   composer install
   ```
3. Salin file environment:
   ```bash
   cp .env.example .env
   ```
4. Generate APP_KEY Laravel:
   ```bash
   php artisan key:generate
   ```
5. Sesuaikan konfigurasi database dan email pada file `.env`:
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=db_athena
   DB_USERNAME=root
   DB_PASSWORD=

   # Konfigurasi Gmail SMTP untuk pengiriman QR Code
   MAIL_MAILER=smtp
   MAIL_HOST=smtp.gmail.com
   MAIL_PORT=587
   MAIL_USERNAME=email_anda@gmail.com
   MAIL_PASSWORD=16_digit_app_password
   MAIL_ENCRYPTION=tls
   MAIL_FROM_ADDRESS="email_anda@gmail.com"
   MAIL_FROM_NAME="${APP_NAME}"
   ```
6. Jalankan migrasi dan seeder database:
   ```bash
   php artisan migrate --seed
   ```
7. Jalankan server Laravel:
   ```bash
   php artisan serve
   ```
   > Backend API berjalan di: `http://localhost:8000`

---

### 2. Setup Frontend Client (`client/`)

1. Buka terminal baru dan masuk ke direktori `client`:
   ```bash
   cd client
   ```
2. Install dependensi Node.js:
   ```bash
   npm install
   ```
3. Jalankan server development Vite:
   ```bash
   npm run dev
   ```
   > Frontend berjalan di: `http://localhost:5173`

---

### 3. Setup Webhook WhatsApp (`webhook/` - Opsional)

1. Masuk ke direktori `webhook`:
   ```bash
   cd webhook
   ```
2. Install dependensi:
   ```bash
   npm install
   ```
3. Buat file `.env` di dalam folder `webhook`:
   ```env
   PORT=3000
   WAZAPBRO_API_URL=https://api.wazapbro.com/send
   WAZAPBRO_TOKEN=token_wazapbro_anda
   ATHENA_API_URL=http://localhost:8000/api/messages
   ```
4. Jalankan webhook server:
   ```bash
   npm start
   ```

---

## 🔑 Akun Default Admin

Setelah menjalankan `php artisan migrate --seed`, akun admin default siap digunakan:

- **Halaman Login Admin:** `http://localhost:5173/admin/login`
- **Username:** `admin`
- **Password:** `admin123`

---

## 📂 Struktur Endpoint Utama Backend

| Method | Endpoint | Auth | Keterangan |
| :--- | :--- | :---: | :--- |
| `GET` | `/api/jurusans` | Publik | Mengambil daftar jurusan keahlian |
| `POST` | `/api/siswa` | Publik | Mendaftarkan calon siswa baru & kirim email QR |
| `POST` | `/api/auth/login` | Publik | Login admin & mendapatkan Sanctum Bearer Token |
| `GET` | `/api/siswa` | Admin | Menampilkan daftar seluruh calon siswa |
| `GET` | `/api/siswa/{id}` | Admin | Mengambil detail calon siswa berdasarkan ID/QR Code |
| `POST` | `/api/siswa/verify` | Admin | Memverifikasi status daftar ulang siswa |

---

## 👥 Pengembang

- **Riki Maulana** & Tim PPDB SMK Tunas Harapan Pati
- © 2024 - 2026 SMK Tunas Harapan Pati
