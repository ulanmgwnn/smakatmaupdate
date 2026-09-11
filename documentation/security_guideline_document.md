# Security Guidelines
## Panduan Keamanan Sistem Web & PPDB SMAS Katolik Makale

Dokumen ini menetapkan prinsip-prinsip keamanan wajib dan praktik terbaik implementasi yang disesuaikan dengan kebutuhan platform web dan sistem PPDB Online SMAS Katolik Makale. Sistem ini menangani data pribadi yang bersifat sensitif (NIK, NISN, data keluarga, berkas pendaftaran) sehingga keamanan harus menjadi prioritas utama sejak tahap desain (*Security by Design*).

---

## 1. Prinsip Keamanan Mendasar

- **Security by Design**: Tinjauan ancaman (*threat modeling*) dilakukan setiap kali fitur baru ditambahkan, terutama pada alur penerimaan formulir PPDB, endpoint API, dan fitur upload berkas.
- **Least Privilege**: Setiap peran pengguna (`admin`, `staff_ppdb`, `user`) hanya memiliki akses minimum yang diperlukan untuk menjalankan tugasnya.
- **Defense in Depth**: Perlindungan berlapis di tingkat middleware Next.js, validasi Zod pada input, query ORM Drizzle yang terparameterisasi, dan enkripsi data sensitif di lapisan database.
- **Data Minimization**: Sistem hanya mengumpulkan data pribadi pendaftar yang benar-benar dibutuhkan untuk proses seleksi PPDB.

---

## 2. Autentikasi & Kontrol Akses

### 2.1 Manajemen Sesi (*Better-Auth*)
- Sesi diimplementasikan menggunakan cookie **HttpOnly**, **Secure** (HTTPS saja), dan **SameSite=Lax** untuk mencegah serangan *Cross-Site Request Forgery* (CSRF) dan akses JavaScript tidak sah pada token sesi.
- Sesi kedaluwarsa otomatis: **30 menit idle** atau **8 jam absolut** untuk sesi staf PPDB/admin; **7 hari** untuk pengguna umum (siswa/alumni).
- ID sesi diperbarui (*regenerated*) setiap kali autentikasi berhasil untuk mencegah serangan *session fixation*.

### 2.2 Kontrol Akses Berbasis Peran (RBAC)
- Setiap route terlindungi (`/admin/*`, `/api/admin/*`) diverifikasi melalui middleware server-side yang memeriksa peran pengguna dari sesi aktif sebelum memproses permintaan apapun.
- Staf PPDB (`staff_ppdb`) hanya memiliki akses baca dan ubah status pendaftar. Mereka **tidak dapat** menghapus data atau menerbitkan warta publik.
- Akses ke berkas unggahan pendaftar (URL berkas dokumen) dibatasi hanya untuk panitia PPDB dan admin yang telah terautentikasi.

### 2.3 Perlindungan Brute-Force & Rate Limiting
- Endpoint login portal (`/api/auth/sign-in`) dibatasi dengan *rate limiting* menggunakan middleware Next.js: maksimal **5 percobaan gagal** dalam 15 menit per alamat IP sebelum akun dikunci sementara.
- Endpoint pengiriman formulir PPDB (`/ppdb/daftar`) dibatasi: maksimal **3 pendaftaran** per alamat IP per hari untuk mencegah pengiriman massal otomatis (*bot spam*).
- Endpoint formulir kontak publik dibatasi: maksimal **10 pesan** per IP per jam.

---

## 3. Penanganan & Validasi Input

### 3.1 Validasi Formulir PPDB (Berlapis Ganda)
- **Sisi Klien** (React Hook Form + Zod): Validasi format langsung saat pengguna mengisi formulir.
  - Format NIK: 16 digit numerik.
  - Format NISN: 10 digit numerik.
  - Nomor WhatsApp: Format `+62` atau awalan `08`, minimal 10 digit.
  - Nilai rapor: Rentang desimal 0.00–100.00.
- **Sisi Server** (Zod + Next.js Server Action): Seluruh input **divalidasi ulang** di sisi server sebelum menyentuh lapisan database, menganggap semua data dari klien sebagai tidak terpercaya.
- Semua kolom teks bebas (misalnya kolom alamat dan nama asal sekolah) dibersihkan dari karakter HTML khusus untuk mencegah serangan *Cross-Site Scripting* (XSS).

### 3.2 Keamanan Upload Berkas Dokumen
- **Validasi Tipe File**: Hanya menerima berkas dengan tipe MIME `application/pdf`, `image/jpeg`, dan `image/png`. Validasi dilakukan berdasarkan *magic bytes* konten berkas, bukan hanya ekstensi nama file.
- **Batas Ukuran**: Maksimal **2 MB per berkas** dan **10 MB total** per pendaftaran.
- **Penamaan Berkas Aman**: Nama berkas asli yang diunggah oleh pengguna **tidak digunakan** sebagai nama penyimpanan. Berkas disimpan menggunakan nama acak yang dihasilkan secara aman (`crypto.randomUUID()`).
- **Penyimpanan Terpisah**: Berkas pendaftar PPDB disimpan di direktori atau bucket penyimpanan yang tidak dapat diakses langsung melalui URL publik tanpa autentikasi yang valid.
- **Pemindaian Konten**: Berkas divalidasi apakah benar-benar berupa gambar atau dokumen PDF yang sah sebelum disimpan secara permanen.

### 3.3 Pencegahan Injeksi SQL
- Seluruh interaksi dengan database menggunakan **Drizzle ORM** yang secara otomatis menggunakan *parameterized queries* — tidak ada satu pun query SQL yang dibentuk melalui penggabungan string (*string concatenation*) langsung.

---

## 4. Perlindungan Data Pribadi (Privasi & Kepatuhan)

- **Enkripsi Data Sensitif**: Nomor Identitas Kependudukan (NIK) calon siswa dan orang tua dienkripsi sebelum disimpan ke kolom database menggunakan AES-256, menggunakan kunci enkripsi yang disimpan di variabel lingkungan (`process.env.ENCRYPTION_KEY`) dan **tidak** di-*commit* ke repositori kode.
- **Data Retention Policy**: Data pendaftar PPDB yang tidak diterima dihapus secara otomatis dari sistem setelah **1 tahun** berdasarkan kebijakan retensi yayasan.
- **Akses Log**: Setiap akses panitia ke berkas pendaftar dicatat (*audit log*) dengan informasi identitas staf, waktu akses, dan data yang diakses.

---

## 5. Keamanan Infrastruktur & Konfigurasi Deployment

### 5.1 Manajemen Variabel Lingkungan
- **Tidak ada** kredensial, kunci rahasia, kunci enkripsi, atau *connection string* database yang boleh di-*hardcode* dalam kode sumber.
- Seluruh rahasia dikonfigurasi di file `.env.local` (lokal) dan disuntikkan oleh sistem deployment di lingkungan produksi/staging.
- File `.env.example` hanya berisi nama variabel dengan nilai kosong atau nilai contoh, sebagai panduan bagi pengembang baru.

```env
# Contoh variabel wajib (lihat .env.example)
DATABASE_URL=
BETTER_AUTH_SECRET=
BETTER_AUTH_URL=
ENCRYPTION_KEY=
NEXT_PUBLIC_APP_URL=
```

### 5.2 Konfigurasi Next.js untuk Produksi
- Aktifkan mode ketat React (`reactStrictMode: true`) pada `next.config.ts`.
- Konfigurasi HTTP Security Headers via `next.config.ts`:
  - `Strict-Transport-Security` (HSTS): Memaksa HTTPS.
  - `X-Content-Type-Options: nosniff`: Mencegah *MIME sniffing*.
  - `X-Frame-Options: SAMEORIGIN`: Mencegah *clickjacking* via iframe.
  - `Content-Security-Policy`: Membatasi sumber skrip, gambar, dan font yang diizinkan.
  - `Referrer-Policy: strict-origin-when-cross-origin`.

### 5.3 Konfigurasi Database & Docker
- Database PostgreSQL dijalankan di jaringan Docker privat (`docker-compose.yaml`) yang **tidak** mengekspos port `5432` secara langsung ke publik di lingkungan produksi. Akses hanya diizinkan dari kontainer aplikasi Next.js.
- Pengguna database dikonfigurasi dengan hak akses minimum: hanya operasi `SELECT`, `INSERT`, `UPDATE`, dan `DELETE` pada tabel aplikasi — **bukan** hak akses superuser.

---

## 6. Penanganan Galat & Logging yang Aman

- Pesan kesalahan yang ditampilkan kepada pengguna bersifat umum dan informatif (contoh: *"Terjadi kesalahan, silakan coba lagi"*) — **tidak pernah** mengekspos detail teknis, nama tabel, atau *stack trace* ke antarmuka pengguna publik.
- Log kesalahan teknis lengkap (termasuk detail *stack trace*) hanya dikirimkan ke sistem pencatatan internal server dan tidak dapat diakses secara publik.
- Kredensial dan data sensitif **tidak boleh** pernah dicatat ke dalam log aplikasi dalam bentuk teks biasa (*plaintext*).