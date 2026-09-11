# Tech Stack Document
## Portal Web Resmi & Sistem PPDB SMAS Katolik Makale

Dokumen ini menguraikan seluruh arsitektur teknologi, pustaka, dan infrastruktur yang digunakan dalam pembangunan platform web SMAS Katolik Makale. Setiap komponen dipilih untuk memastikan keandalan, kecepatan tinggi, keamanan data pendaftar, kemudahan pemeliharaan jangka panjang, serta kepatuhan pada sistem desain *International Academy*.

---

## 1. Frontend Architecture & UI Framework

### Core Framework & Library
- **Next.js 15 (App Router)**
  - Menggunakan arsitektur *React Server Components* (RSC) untuk merender halaman publik institusi di sisi server, menghasilkan *First Contentful Paint* (FCP) yang instan dan indeks SEO maksimal pada mesin pencari.
  - Mendukung *Turbopack* untuk kompilasi lokal yang cepat (`next dev --turbopack`).
  - Menyediakan *Server Actions* untuk pengiriman formulir PPDB dan mutasi data tanpa memerlukan penulisan boilerplate API berlebih.
- **React 19 & React DOM 19**
  - Fondasi antarmuka berbasis komponen modern dengan performa render tinggi dan pengelolaan state asinkron bawaan (*Actions*, `useActionState`, `useOptimistic`).
- **TypeScript 5 (Strict Mode)**
  - Menjamin keamanan tipe (*type safety*) pada seluruh alur pendaftaran PPDB, skema database, dan props komponen, mencegah kesalahan runtime di lingkungan produksi.

### Styling & Design System Integration
- **Tailwind CSS v4**
  - Mesin utilitas CSS performa tinggi dengan integrasi PostCSS generasi baru.
  - Pemetaan token desain resmi dari `documentation/international_academy/DESIGN.md`:
    - **Warna Utama**: Deep Navy (`#0A1D37` / `#00030A`), Academic Amber Gold (`#F4A900` / `#7F5600`), Surface (`#F8F9FF`), Neutral Slate (`#121C2A`).
    - **Tipografi**: *Playfair Display* (Editorial Serif untuk judul, pilar, angka metrik statistik) dan *Inter* (Clean Sans-serif untuk teks badan, form label, dan navigasi).
    - **Border Radius & Spacing**: Sudut melengkung elegan (`rounded-xl` / `rounded-2xl`) dengan spasi grid 12 kolom konsisten.
- **Tailwind Merge & CVA (Class Variance Authority)**
  - `tailwind-merge` dan `clsx` digunakan untuk penggabungan kelas utilitas dinamis tanpa konflik CSS specificity.

### UI Primitives & Interactive Components
- **Radix UI Primitives**
  - Komponen antarmuka yang aksesibel (WAI-ARIA compliant): Dialog/Modal (pemutar video profil sekolah), Accordion (FAQ PPDB & Kurikulum), Dropdown Menu, Select (pilihan jalur PPDB & peminatan), Tabs, Popover, dan Tooltip.
- **Iconography**
  - **Lucide React** & **Tabler Icons**: Pustaka ikon garis halus untuk navigasi, indikator pilar sekolah, dan status berkas.
  - **Google Material Symbols Outlined**: Digunakan untuk ikon khusus akademik dan badge akreditasi sesuai mockup desain.
- **Visual & Rich Media Elements**
  - **Embla Carousel**: Slider interaktif untuk galeri fasilitas kampus, dokumentasi prestasi, dan banner warta utama.
  - **Recharts**: Visualisasi data statistik kelulusan PTN/Kedinasan dan perkembangan pendaftaran PPDB pada dashboard pengelola.
  - **Sonner**: Notifikasi toast elegan dan responsif untuk konfirmasi aksi formulir.

---

## 2. Form Handling & Data Validation

- **React Hook Form**
  - Pengelolaan state formulir pendaftaran PPDB yang ringan, cepat, dan tidak memicu re-render berlebih pada halaman.
- **Zod v4**
  - Skema validasi deklaratif menyeluruh:
    - Memvalidasi format NIK, NISN, nomor telepon orang tua/wali, serta nilai rapor sekolah menengah pertama.
    - Validasi berkas unggahan: memastikan tipe MIME yang diizinkan (`application/pdf`, `image/jpeg`, `image/png`) dan batas ukuran maksimal (2MB per berkas).

---

## 3. Backend, Database, & Authentication

### Database Layer
- **PostgreSQL**
  - Sistem manajemen basis data relasional yang handal dan stabil untuk menyimpan data pendaftar PPDB, riwayat status seleksi, arsip warta, agenda kegiatan, serta data pengguna sekolah.
- **Drizzle ORM & Drizzle Kit**
  - ORM TypeScript modern yang sangat cepat dan ramah performa (zero-overhead).
  - Skema terdefinisi secara deklaratif di folder `db/schema/` dengan migrasi otomatis melalui `drizzle-kit generate` dan `drizzle-kit migrate`.
  - Dilengkapi antarmuka inspeksi data visual melalui `drizzle-kit studio`.

### Authentication & Session Management
- **Better-Auth**
  - Kerangka autentikasi modern berbasis sesi aman (*HttpOnly*, *Secure*, *SameSite* cookies) yang terintegrasi langsung dengan Drizzle ORM.
  - Pengelolaan peran (*Role-Based Access Control* - RBAC):
    - `admin`: Akses penuh konfigurasi situs, manajemen warta, dan verifikasi PPDB.
    - `panitia_ppdb`: Akses peninjauan berkas pendaftar, pencatatan wawancara, dan pembaruan status seleksi.
    - `user` (siswa/alumni/wali): Akses ke dashboard pelacakan progres berkas.

---

## 4. Containerization, DevOps, & Lingkungan Kerja

- **Docker & Docker Compose**
  - Konfigurasi `docker-compose.yaml` yang siap menjalankan PostgreSQL lokal secara terisolasi:
    - Profil `postgres`: Server PostgreSQL utama untuk lingkungan pengembangan lokal.
    - Profil `postgres-dev`: Lingkungan pengujian terpisah untuk seeding data demo.
- **Linting & Code Formatting**
  - **ESLint 9** dengan konfigurasi `eslint-config-next` untuk kepatuhan standar kode Next.js.
  - **Prettier** untuk penataan gaya penulisan kode seragam.
- **Package Management**
  - Dijalankan menggunakan Node.js (v20+) dan NPM.

---

## 5. Ringkasan Matriks Perangkat Lunak

| Lapisan (Layer) | Pustaka / Alat | Versi | Tujuan Penggunaan |
|---|---|---|---|
| **Framework** | Next.js | 15.5.0 | Routing halaman, SSR/SSG, Server Actions |
| **UI Core** | React / React DOM | 19.1.0 | Library komponen antarmuka deklaratif |
| **Language** | TypeScript | ^5.0 | Type safety dan dokumentasi kode otomatis |
| **CSS Engine** | Tailwind CSS | ^4.0 | Utilitas desain sistem *International Academy* |
| **Komponen UI** | Radix UI | Latest | Primitif modal, dropdown, tabs, accordion |
| **Form & Validasi** | React Hook Form + Zod | ^7.62 / ^4.1 | Formulir PPDB online bertahap & validasi berkas |
| **ORM** | Drizzle ORM / Kit | ^0.44 / ^0.31 | Abstraksi query database relasional tipe-aman |
| **Database** | PostgreSQL | 16 (Docker) | Penyimpanan data relasional aman |
| **Autentikasi** | Better-Auth | ^1.3.7 | Sesi terenkripsi & RBAC staf/admin |
| **Notifikasi** | Sonner | ^2.0.7 | Toast notifikasi status pengiriman pendaftaran |