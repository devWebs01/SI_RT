# DOKUMENTASI ANALISIS SISTEM RTku

## 1. PENJELASAN UTAMA SISTEM

### 1.1 Tujuan Sistem
**RTku** adalah sistem informasi manajemen komunitas berbasis web yang dirancang untuk mengelola dan memudahkan administrasi pertukiman/permukiman (khususnya dalam konteks RT/RW di Indonesia). Sistem ini menyediakan platform terpusat untuk mengelola data penduduk, pengajuan surat-surat resmi, dan komunikasi antaranggota komunitas.

### 1.2 Scope dan Konteks
- **Nama Aplikasi**: RTku
- **Tipe Aplikasi**: Web Application (Laravel Framework)
- **Lokasi Deployment**: Localhost:8000 (konfigurasi default)
- **Database**: MySQL (rtku_db)
- **Struktur**: Multi-user dengan role-based access control (RBAC)

### 1.3 Arsitektur Teknis
| Komponen | Spesifikasi |
|----------|-------------|
| **Backend** | Laravel 12.x |
| **Database** | SQLite |
| **Frontend** | Bootstrap (dengan DataTables, CKEditor) |
| **Authentication** | Laravel Sanctum / Session-based |
| **Deployment** | Docker (Nginx + PHP-FPM + MySQL) |
| **Dokumen** | PhpWord (generasi surat), Dompdf (laporan PDF) |

---

## 2. FITUR-FITUR SISTEM

### 2.1 Fitur Utama (Core Features)

#### A. Manajemen Kartu Keluarga (KK)
- **Deskripsi**: Pengelolaan data Kartu Keluarga (KK) seluruh warga
- **Komponen**:
  - Pembuatan KK baru
  - Edit data KK (alamat, kepala keluarga, dll)
  - Hapus KK
  - View detail KK beserta anggota
- **Model**: [`FamilyCard`](app/Models/FamilyCard.php)
- **Controller**: [`FamilyCardController`](app/Http/Controllers/Admin/FamilyCardController.php)

#### B. Manajemen Penduduk
- **Deskripsi**: Pengelolaan data seluruh penduduk/warga
- **Data yang dikelola**:
  - Identitas (NIK, Nama Lengkap, Jenis Kelamin)
  - Biodata (Tempat Lahir, Tanggal Lahir, Agama)
  - Pendidikan dan Pekerjaan
  - Status Perkawinan
  - Orang Tua/Wali
  - Status Kependudukan (Warga Tetap/Tidak Tetap)
- **Model**: [`Resident`](app/Models/Resident.php)
- **Controller**: [`ResidentController`](app/Http/Controllers/Admin/ResidentController.php)

#### C. Manajemen Pengguna (User Management)
- **Deskripsi**: Pengelolaan akun pengguna sistem
- **Tipe Pengguna**:
  - **Admin**: Akses penuh ke semua fitur
  - **Warga**: Akses terbatas (pengajuan permohonan, lihat profil)
- **Data Pengguna**:
  - Username/Hp
  - Password (encrypted)
  - Role/Peran
  - Berkas (KTP, KK)
  - Tanggal verifikasi
- **Model**: [`User`](app/Models/User.php)
- **Controller**: [`UserController`](app/Http/Controllers/Admin/UserController.php)

#### D. Sistem Permohonan Surat
- **Deskripsi**: Pengajuan dan pengelolaan surat-surat resmi
- **Jenis Permohonan Tersedia** (14 jenis):
  1. KETERANGAN IZIN MENIKAH
  2. KETERANGAN KELAHIRAN
  3. KETERANGAN BELUM MEMILIKI RUMAH
  4. KETERANGAN BELUM MENIKAH
  5. KETERANGAN BERKELAKUAN BAIK
  6. KETERANGAN DOMISILI
  7. KETERANGAN IMB
  8. KETERANGAN KTP SEMENTARA
  9. KETERANGAN PBB
  10. KETERANGAN PINDAH
  11. KETERANGAN TIDAK MAMPU
  12. PENGANTAR NIKAH
  13. PERMOHONAN BLANKO KK DAN KTP
  14. KETERANGAN IZIN USAHA
- **Status Permohonan**:
  - **Pending**: Menunggu konfirmasi admin
  - **Dikonfirmasi**: Sudah disetujui admin
  - **Valid**: Surat sudah sah/valid
- **Model**: [`Request`](app/Models/Request.php), [`RequestType`](app/Models/RequestType.php), [`RequestForm`](app/Models/RequestForm.php)
- **Controller**: [`RequestController`](app/Http/Controllers/Admin/RequestController.php), [`CitizenRequestController`](app/Http/Controllers/Admin/CitizenRequestController.php)

#### E. Manajemen Pengumuman
- **Deskripsi**: Pengelolaan pengumuman/pemberitahuan kepada warga
- **Fitur**:
  - Membuat pengumuman baru
  - Edit pengumuman
  - Hapus pengumuman
  - Tanggal pengumuman
- **Model**: [`Announcement`](app/Models/Announcement.php)
- **Controller**: [`AnnouncementController`](app/Http/Controllers/Admin/AnnouncementController.php)

#### F. Daftar Tamu/Visitasi
- **Deskripsi**: Pencatatan tamu yang datang/mengunjungi wilayah
- **Controller**: [`GuestController`](app/Http/Controllers/Admin/GuestController.php)

#### G. Manajemen Perubahan Keterangan
- **Deskripsi**: Pencatatan perubahan status/informasi penduduk
- **Model**: [`InformationChange`](app/Models/InformationChange.php)
- **Controller**: [`ChangeController`](app/Http/Controllers/Admin/ChangeController.php)

### 2.2 Fitur Sekunder

#### A. Laporan dan Statistik
- **Laporan Warga Tetap**: Daftar warga dengan status tetap
- **Laporan Warga Tidak Tetap**: Daftar warga dengan status tidak tetap
- **Laporan Warga Lahir**: Daftar kelahiran
- **Fitur Print**: Export laporan ke PDF
- **Controller**: [`ResidentController`](app/Http/Controllers/Admin/ResidentController.php) (method laporan*)

#### B. Upload Berkas
- **Deskripsi**: Pengelolaan upload dokumen (KTP, KK)
- **Controller**: [`UploadController`](app/Http/Controllers/UploadController.php)
- **Fitur**:
  - Upload KTP
  - Upload KK
  - Revert/Hapus upload

#### C. Registrasi Warga Baru
- **Deskripsi**: Pendaftaran warga baru ke sistem
- **Alur**: Input KK → Input Penduduk → Upload Berkas → Verifikasi
- **Controller**: [`DaftarController`](app/Http/Controllers/DaftarController.php)

---

## 3. WORKFLOW PER FITUR

### 3.1 Workflow Manajemen KK dan Penduduk

```
┌─────────────────┐
│  Admin Login    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Dashboard      │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│  KK   │ │Penduduk│
└───┬───┘ └───┬───┘
    │         │
    ▼         ▼
┌───────────┐ ┌───────────┐
│ Create/   │ │ Create/   │
│ Edit/     │ │ Edit/     │
│ Delete    │ │ Delete    │
└───────────┘ └───────────┘
    │         │
    └────┬────┘
         ▼
┌─────────────────┐
│  Data Tersimpan │
│  di Database    │
└─────────────────┘
```

**Langkah Kerja**:
1. Admin login ke sistem
2. Akses menu KK atau Penduduk
3. Pilih aksi (Create/Read/Update/Delete)
4. Isi/formulir data sesuai kebutuhan
5. Simpan data ke database
6. Sistem menampilkan notifikasi berhasil/gagal

### 3.2 Workflow Pengajuan Surat

```
┌─────────────────┐
│  Warga Login    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Pilih Jenis    │
│  Permohonan     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Isi Form      │
│  Permohonan     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Upload Berkas  │
│  Pendukung      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Submit         │
│  Permohonan     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│  Status:        │────►│  Admin Review   │
│  Pending       │     │  & Konfirmasi   │
└─────────────────┘     └────────┬────────┘
                                 │
                    ┌────────────┴────────────┐
                    ▼                         ▼
          ┌─────────────────┐      ┌─────────────────┐
          │  Ditolak        │      │  Disetujui      │
          │  (Keterangan)   │      │  (Generate     │
          └─────────────────┘      │   Surat)       │
                                  └────────┬────────┘
                                           │
                                           ▼
                                  ┌─────────────────┐
                                  │  Status:        │
                                  │  Valid/Sah      │
                                  └─────────────────┘
```

**Langkah Kerja Warga**:
1. Login sebagai warga
2. Pilih menu "Permohonan"
3. Klik "Buat Permohonan Baru"
4. Pilih jenis permohonan dari dropdown
5. Isi formulir sesuai jenis permohonan
6. Tambahkan keterangan jika diperlukan
7. Submit permohonan
8. Tunggu konfirmasi admin
9. Jika disetujui, download/link surat

**Langkah Kerja Admin**:
1. Login sebagai admin
2. Akses menu "Permohonan Warga"
3. Lihat daftar permohonan masuk
4. Klik tombol konfirmasi pada permohonan
5. Generate surat secara otomatis (PhpWord)
6. Tanda tangani dan upload surat
7. Set status menjadi "Valid"

### 3.3 Workflow Pendaftaran Warga Baru

```
┌─────────────────┐
│  Buka Halaman   │
│  Pendaftaran    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Input Data KK  │
│  (No.KK, Alamat,│
│   Kepala Keluarga)│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Input Data     │
│  Penduduk       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Upload Berkas  │
│  (KTP, KK)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Sinkron ke     │
│  Aplikasi       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Verifikasi     │
│  Admin          │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Akun Dibuat    │
│  Warga Baru     │
└─────────────────┘
```

### 3.4 Workflow Pengumuman

```
┌─────────────────┐
│  Admin Login    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Menu Pengumuman│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Buat/Edit      │
│  Pengumuman     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Isi Konten     │
│  (CKEditor)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Set Tanggal    │
│  Publish        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Simpan &       │
│  Publish        │
└─────────────────┘
```

---

## 4. WORKFLOW PER AKTOR

### 4.1 Aktor: Admin

| No | Aktivitas | Modul |
|----|-----------|-------|
| 1 | Login ke sistem | Authentication |
| 2 | Lihat dashboard | Home |
| 3 | Kelola data KK | Family Card |
| 4 | Kelola data Resident | Resident |
| 5 | Kelola pengguna | User |
| 6 | Buat/Terbitkan pengumuman | Announcement |
| 7 | Kelola jenis permohonan | Request Type |
| 8 | Review & konfirmasi permohonan warga | Citizen Request |
| 9 | Generate surat | Request |
| 10 | Catat perubahan keterangan warga | Change |
| 11 | Kelola tamu/visitasi | Guest |
| 12 | Cetak laporan warga | Laporan |
| 13 | Logout | Authentication |

### 4.2 Aktor: Warga

| No | Aktivitas | Modul |
|----|-----------|-------|
| 1 | Registrasi akun baru | Daftar |
| 2 | Verifikasi akun | Email/SMS |
| 3 | Login ke sistem | Authentication |
| 4 | Lihat pengumuman | Pengumuman |
| 5 | Ajukan permohonan surat | Request |
| 6 | Lihat status permohonan | Request |
| 7 | Download surat yang disetujui | Request |
| 8 | Lihat profil sendiri | Profil |
| 9 | Logout | Authentication |

### 4.3 Aktor: Pengguna Baru (Calon Warga)

| No | Aktivitas | Modul |
|----|-----------|-------|
| 1 | Akses halaman daftar | Daftar |
| 2 | Input data KK | Daftar |
| 3 | Input data penduduk | Daftar |
| 4 | Upload berkas KTP | Upload |
| 5 | Upload berkas KK | Upload |
| 6 | Submit pendaftaran | Daftar |
| 7 | Tunggu verifikasi admin | - |

---

## 5. PENJABARAN LAINNYA

### 5.1 Struktur Database

#### Tabel Utama:
| Tabel | Deskripsi |
|-------|-----------|
| `users` | Data pengguna sistem (admin & warga) |
| `family_cards` | Data Kartu Keluarga |
| `residents` | Data warga/penduduk |
| `requests` | Data pengajuan surat |
| `request_types` | Katalog jenis surat |
| `request_forms` | Detail formulir per jenis permohonan |
| `announcements` | Data pengumuman |
| `information_changes` | Riwayat perubahan status warga |
| `roles` | Peran pengguna |
| `permissions` | Hak akses |

#### Relasi Tabel:
```
User (1) ────── (N) Request
User (1) ────── (1) Resident
Resident (N) ────── (1) FamilyCard
Request (N) ──── (1) RequestType
Request (N) ──── (1) Resident
```

### 5.2 Keamanan dan Akses

#### Mekanisme Autentikasi:
- **Login**: Session-based (Laravel Auth)
- **Password**: Hash bcrypt ($2y$10$)
- **Middleware**: `auth` untuk proteksi route admin

#### Role-Based Access Control (RBAC):
| Peran | Akses |
|-------|-------|
| `admin` | Full akses semua fitur |
| `warga` | Terbatas (ajuan permohonan, lihat profil) |

### 5.3 Teknologi dan Library

| Komponen | Teknologi |
|----------|-----------|
| Framework | Laravel 8.x |
| Database | MySQL 5.7+ |
| UI Framework | Bootstrap 4/5 |
| Data Table | Yajra DataTables |
| Rich Text Editor | CKEditor |
| PDF Generation | Dompdf |
| Word Generation | PhpWord |
| QR Code | SimpleQRCode |
| Image Upload | Intervention Image |

### 5.4 Endpoint/API Penting

#### Route Publik:
- `GET /` → Redirect ke login
- `GET /home` → Dashboard (redirect based on role)
- `POST /upload` → Upload berkas
- `GET /validasi/{kode_verifikasi}` → Validasi permohonan
- `GET /daftar` → Halaman registrasi
- `POST /daftar/kk` → Simpan KK baru
- `POST /daftar/penduduk` → Simpan penduduk

#### Route Admin (prefix: /admin):
- `GET /` → Dashboard
- `Resource: family-cards` → CRUD KK
- `Resource: residents` → CRUD Resident
- `Resource: users` → CRUD User
- `Resource: announcements` → CRUD Announcement
- `Resource: request-types` → CRUD Jenis Surat
- `Resource: requests` → Manajemen request user
- `Resource: citizen-requests` → Review permohonan warga
- `Resource: guests` → Manajemen tamu
- `GET /reports/resident-*` → Laporan berbagai jenis

### 5.5 Aspek Operasional

#### Backup dan Recovery:
- Database: mysqldump (lihat [`rtku_db.sql`](rtku_db.sql))
- File: Docker volume persistence

#### Logs:
- Laravel log di `storage/logs/`
- Audit trail menggunakan trait [`Auditable`](app/Traits/Auditable.php)

#### Deployment:
- Docker Compose (Nginx + PHP + MySQL)
- Konfigurasi: [`docker-compose.yml`](docker-compose.yml)

### 5.6 Batasan dan Asumsi

1. **Skala**: Dirancang untuk skala RT/RW (puluhan hingga ratusan warga)
2. **Single Instance**: Satu database untuk satu komunitas
3. **Dokumen Fisik**: Surat di-generate dalam format Word (.docx)
4. **Validasi**: Kode verifikasi untuk validasi permohonan
5. **File Storage**: Lokal (public/berkas/)
6. **Session**: 180 menit (lihat [.env:21](.env))

### 5.7 Fitur-Fitur Tersembunyi

1. **Sinkronisasi**: Endpoint `/daftar/sinkron-app` untuk sinkronisasi data eksternal
2. **Revert Upload**: Kemampuan membatalkan upload sebelum final
3. **Dynamic Form**: Form permohonan berbeda-beda berdasarkan jenis (field `add_form`)
4. **QR Code**: Generate QR code untuk validasi surat
5. **Multi-format Export**: Dukungan PDF dan Word untuk dokumen

---

*Dokumen ini dibuat berdasarkan analisis kode sumber sistem RTku pada tanggal 1 Maret 2026.*
