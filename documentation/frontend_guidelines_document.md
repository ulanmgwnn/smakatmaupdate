# Frontend Guidelines Document
## Panduan Pengembangan Antarmuka & Sistem Desain SMAS Katolik Makale

Dokumen ini menjadi acuan utama bagi pengembang antarmuka (frontend developer) dalam mengimplementasikan seluruh halaman situs SMAS Katolik Makale. Panduan ini sepenuhnya mengadopsi spesifikasi desain dari **International Academy Design System** (`documentation/international_academy/DESIGN.md`) serta 5 berkas mockup HTML (`beranda`, `profil_sejarah`, `akademik_kesiswaan`, `berita_acara`, dan `ppdb_online`).

---

## 1. Filosofi & Estetika Visual (Brand & Style)

Antarmuka web SMAS Katolik Makale mencerminkan wibawa, tradisi panjang sejak 1958, dan keunggulan akademis institusi pendidikan Katolik di tanah Toraja. Konsep visual menggabungkan **Presisi Modern / Korporat** dengan **Keanggunan Editorial**:
- **Authoritative Anchors**: Latar belakang *Deep Navy* (`#0A1D37`) pada header, pita metrik, dan tombol utama memberikan kesan kokoh, stabil, dan berwibawa.
- **Warm Prestige**: Aksen *Warm Academic Gold* (`#F4A900`) memberikan kehangatan, optimisme, dan menonjolkan titik fokus konversi (misalnya tombol *Daftar PPDB* dan angka prestasi).
- **Editorial Legibility**: Judul-judul utama menggunakan font *serif* dengan kontras tinggi (*Playfair Display*), diimbangi antarmuka *sans-serif* modern (*Inter*) untuk keterbacaan yang jernih pada teks panjang dan formulir.
- **Balanced Structure**: Tata letak grid simetris 12 kolom dengan batas kontainer maksimal 1280px (`max-w-container-max`), dipercantik pita metrik struktural melayang (*overlapping ribbons*).

---

## 2. Sistem Warna (Color Tokens)

Berikut adalah pemetaan token warna resmi yang dikonfigurasikan pada Tailwind CSS:

| Token Warna | Nilai Hex | Peruntukan Penggunaan |
|---|---|---|
| `primary-container` / `primary` | `#0A1D37` / `#00030A` | Header, pita pilar sekolah, tombol navigasi utama, footer |
| `secondary` | `#F4A900` / `#7F5600` | Aksen emas, teks penekanan pada headline, lencana unggulan |
| `secondary-container` | `#FFB215` | Latar belakang tombol konversi utama ("DAFTAR PPDB") |
| `on-secondary-container` | `#6B4800` | Warna teks tombol pada kontainer sekunder |
| `surface` | `#F8F9FF` | Latar belakang kanvas umum halaman (lembut, sejuk) |
| `surface-container-lowest` | `#FFFFFF` | Latar belakang kartu (*card*), modal, dan kontainer formulir |
| `surface-container-low` | `#EFF4FF` | Latar belakang panel fitur dan kartu sekunder |
| `surface-container` | `#E6EEFF` | Pembatas dan area latar belakang dengan kontras halus |
| `on-surface` | `#121C2A` | Warna teks utama (*Slate Charcoal*) dengan kontras ramah mata |
| `on-surface-variant` | `#44474D` | Teks pendukung, deskripsi paragraf, dan metadata |
| `outline` / `outline-variant` | `#75777E` / `#C5C6CE` | Garis batas input, pembatas kartu, dan garis divider |
| `error` / `error-container` | `#BA1A1A` / `#FFDAD6` | Indikator kesalahan validasi formulir dan peringatan sistem |

---

## 3. Sistem Tipografi (Typography System)

Kombinasi dua jenis huruf wajib digunakan secara konsisten:

### 1. *Playfair Display* (Editorial Serif)
Digunakan khusus untuk judul, subjudul editorial, dan angka statistik:
- `display-hero`: Ukuran 56px / Line-height 64px / Bold 700 / Spacing -0.02em (Desktop); 36px / 44px pada Mobile.
- `headline-lg`: Ukuran 40px / Line-height 48px / Bold 700 / Spacing -0.015em (Desktop); 28px / 36px pada Mobile.
- `headline-md`: Ukuran 28px / Line-height 36px / Semi-bold 600.
- `headline-sm`: Ukuran 22px / Line-height 30px / Semi-bold 600.
- `stat-number`: Ukuran 32px / Line-height 36px / Bold 700.

### 2. *Inter* (Systematic Sans-Serif)
Digunakan untuk navigasi, isi paragraf, label formulir, tombol, dan microcopy:
- `body-lg`: Ukuran 18px / Line-height 28px / Regular 400.
- `body-md`: Ukuran 15px / Line-height 24px / Regular 400.
- `body-sm`: Ukuran 13px / Line-height 20px / Regular 400.
- `label-eyebrow`: Ukuran 12px / Line-height 16px / Bold 700 / Spacing 0.12em (Kapital penuh).
- `label-md`: Ukuran 14px / Line-height 20px / Semi-bold 600.

---

## 4. Spasi & Sistem Grid (Grid & Spacing Scale)

- **Batas Kontainer Maksimal**: `max-w-[1280px]` (`max-w-container-max`) berada di tengah layar (`mx-auto`).
- **Jarak Tepi Layar (Gutters)**:
  - Layar Ponsel (< 768px): `px-4` (`1rem`)
  - Layar Tablet (768px - 1023px): `px-6` (`1.5rem`)
  - Layar Komputer (1024px+): `px-8` (`2rem`)
- **Skala Jarak Antar Seksi (Vertical Rhythm)**:
  - Antar seksi utama halaman: `py-16 lg:py-24` (`space-3xl` s.d. `space-4xl`).
  - Antar elemen judul, deskripsi, dan tombol: `gap-3` s.d. `gap-6` (`space-xs` s.d. `space-md`).

---

## 5. Pola Komponen Inti (Core Component Patterns)

### 1. Header Global & Bilah Informasi Atas (Topbar)
- **Topbar**: Bilah atas berwarna *Deep Navy* (`bg-primary-container`) dengan tinggi kompak (`py-1`). Menampilkan ikon lokasi (*Makale, Tana Toraja*), nomor kontak telepon resmi, serta tautan cepat menuju *Portal Siswa* dan *Portal Alumni*.
- **Main Navbar**: Latar putih transparan dengan efek buram (`bg-surface-container-lowest/95 backdrop-blur-md`). Berisi logo resmi sekolah, nama SMAS KATOLIK MAKALE beserta semboyan *Fides, Scientia, Humaniora*, tautan navigasi dengan garis bawah aktif (*active underline indicator*), serta tombol konversi menonjol `DAFTAR PPDB`.

### 2. Pita Metrik Struktural (4 Pilar Sekolah)
- Komponen pita horizontal berwarna *Deep Navy* (`bg-primary-container`) dengan efek tumpang-tindih melayang (-mt-6 lg:-mt-10) di bawah hero section.
- Menampilkan 4 pilar: **Fides** (Iman), **Scientia** (Ilmu), **Humaniora** (Kemanusiaan), dan **Lingkungan Aman** dengan pembatas halus dan ikon garis emas.

### 3. Kartu Program & Fasilitas (Program & Campus Cards)
- Rasio gambar `16:10` atau `4:3` dengan sudut lengkung `rounded-xl`.
- Latar kartu putih murni (`bg-surface-container-lowest`), bayangan halus (`shadow-sm`), dan efek transisi hover naik 4px (`hover:-translate-y-1 transition-all`).

### 4. Dialog Pemutar Video Profil
- Modal dialog berbasis *Radix UI Dialog* dengan latar gelap semi-transparan (`bg-black/80`).
- Menyematkan pemutar video profil institusional SMAS Katolik Makale dengan kontrol responsif 16:9.

### 5. Formulir Pendaftaran PPDB Multi-Step
- Pembagian tahapan pendaftaran secara bertahap:
  1. **Jalur & Peminatan**: Pemilihan Jalur Prestasi / Beasiswa / Reguler dan pilihan peminatan (MIPA/IPS/Bahasa).
  2. **Data Diri Calon Siswa**: Nama lengkap, NIK, NISN, tempat tanggal lahir, jenis kelamin, agama, alamat domisili.
  3. **Data Asal Sekolah & Nilai**: Nama SMP/MTs asal, kabupaten, nilai rata-rata rapor semester 1-5.
  4. **Data Orang Tua / Wali**: Nama ayah/ibu/wali, pekerjaan, nomor WhatsApp aktif.
  5. **Upload Dokumen**: Komponen dropzone untuk unggah Kartu Keluarga, Rapor, Akta Kelahiran, dan Surat Baptis (opsional).
  6. **Konfirmasi & Unduh Bukti**: Halaman ringkasan data sebelum kirim dan pembuatan kartu pendaftaran ber-QR code.

---

## 6. Struktur Direktori Komponen Frontend

```
components/
├── common/
│   ├── header.tsx              # Topbar + Main Navbar + Mobile Drawer
│   ├── footer.tsx              # Footer komprehensif institusi
│   ├── video-modal.tsx         # Dialog pop-up video profil sekolah
│   └── section-heading.tsx     # Eyebrow + Playfair Headline + Deskripsi
├── home/
│   ├── hero-section.tsx        # Hero banner dengan lencana akreditasi
│   ├── four-pillars-strip.tsx  # Pita 4 pilar (Fides, Scientia, Humaniora)
│   ├── about-showcase.tsx      # Ringkasan keunggulan & fasilitas
│   ├── academic-programs.tsx   # Kartu peminatan kurikulum merdeka
│   ├── headmaster-speech.tsx   # Sambutan kepala sekolah
│   ├── news-preview.tsx        # Cuplikan warta terkini
│   └── testimonials.tsx        # Kutipan testimoni alumni
├── ppdb/
│   ├── step-indicator.tsx      # Bar penunjuk langkah pendaftaran
│   ├── form-pathway.tsx        # Langkah 1: Jalur & Peminatan
│   ├── form-student-data.tsx   # Langkah 2: Identitas siswa
│   ├── form-school-origin.tsx  # Langkah 3: Asal sekolah SMP
│   ├── form-parent-data.tsx    # Langkah 4: Orang tua & kontak
│   ├── form-document-upload.tsx# Langkah 5: Unggah berkas
│   └── registration-success.tsx# Langkah 6: Bukti pendaftaran & QR
├── news/
│   ├── featured-article.tsx    # Berita utama tampilan besar
│   ├── news-grid.tsx           # Grid daftar warta dengan filter
│   ├── category-tabs.tsx       # Tab penyaring kategori warta
│   └── event-calendar-card.tsx # Kartu kalender agenda acara
└── ui/                         # Komponen primitif Shadcn / Radix UI
    ├── button.tsx
    ├── dialog.tsx
    ├── input.tsx
    ├── select.tsx
    ├── badge.tsx
    └── tabs.tsx
```