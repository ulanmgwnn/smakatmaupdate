# Project Requirements Document (PRD)
## Portal Web Resmi & Sistem PPDB Online SMAS Katolik Makale

---

## 1. Ringkasan Proyek (Project Overview)

**SMAS Katolik Makale** adalah lembaga pendidikan menengah atas terkemuka dan terakreditasi Unggul (Akreditasi A BAN-S/M) yang berkedudukan di Makale, Kabupaten Tana Toraja, Sulawesi Selatan. Sejak berdiri pada tahun 1958 di Bumi Lakipadada, sekolah ini mengemban misi pendidikan holistik yang memadukan integritas iman Kristiani (*Fides*), keunggulan ilmu pengetahuan modern (*Scientia*), kepedulian kemanusiaan serta pelestarian budaya lokal Toraja (*Humaniora*), dengan semboyan kehormatan *Veritas et Sapientia* (Kebenaran dan Kebijaksanaan).

Proyek ini bertujuan membangun **Portal Web Resmi Terintegrasi dan Sistem Penerimaan Peserta Didik Baru (PPDB) Online** bagi SMAS Katolik Makale. Platform ini dirancang untuk:
1. Menampilkan citra institusi yang berwibawa, berprestasi, dan elegan dengan mengadopsi standar sistem desain *International Academy Editorial*.
2. Memfasilitasi calon peserta didik dan orang tua dari Tana Toraja, Toraja Utara, maupun luar daerah untuk mengakses informasi akademik, fasilitas, asrama putra-putri, dan melakukan pendaftaran online terstruktur.
3. Menyediakan kanal warta resmi, pengumuman, dokumentasi prestasi, dan agenda kegiatan sekolah yang dinamis dan terverifikasi.
4. Memberikan gerbang akses awal bagi siswa, alumni, guru, serta panitia PPDB untuk mempermudah koordinasi dan administrasi sekolah.

Keberhasilan proyek diukur dari kecepatan akses (< 1.5 detik), tampilan responsif lintas perangkat (mobile, tablet, desktop), pengalaman pendaftaran PPDB yang mudah tanpa kendala teknis, serta kemudahan pengelolaan konten oleh pengelola sekolah.

---

## 2. Cakupan Proyek (In-Scope vs. Out-of-Scope)

### Fitur Dalam Cakupan (In-Scope - Fase 1)

1. **Halaman Beranda Institusional (`/`)**:
   - Topbar informatif: Lokasi kampus (Makale, Tana Toraja), nomor kontak/telepon resmi, serta tautan cepat Portal Siswa & Portal Alumni.
   - Navigasi utama: Beranda, Profil, Akademik, Kesiswaan, Berita & Acara, PPDB, serta tombol aksi cepat *DAFTAR PPDB*.
   - *Hero Section* terpadu dengan pesan utama, lencana Akreditasi Unggul, pemutaran video profil sekolah melalui modal dialog, dan indikator tradisi 65+ tahun.
   - Pita Metrik Struktural (4 Pilar Utama): *Fides* (Iman), *Scientia* (Ilmu), *Humaniora* (Kemanusiaan & Budaya), dan *Lingkungan Aman* (Bebas Perundungan & Asri).
   - Pratinjau Profil & Fasilitas Unggulan.
   - Peminatan Akademik (Kurikulum Merdeka: MIPA/Sains, IPS/Humaniora, Bahasa & Budaya, Kelas Riset & Olimpiade).
   - Warta Terkini & Agenda Kampus Mendatang.
   - Sambutan Resmi Kepala Sekolah.
   - Testimoni Siswa dan Alumni Berprestasi.
   - Banner Ajakan Aksi PPDB Online Terbuka.
   - Footer komprehensif berisi peta kontak, tautan navigasi, media sosial resmi, dan informasi legal akreditasi.

2. **Halaman Profil & Sejarah Sekolah (`/profil`)**:
   - Linimasa sejarah berdirinya sekolah sejak 1958 di Makale.
   - Visi, Misi, Tujuan Institusi, dan Nilai-Nilai Dasar (*Fides, Scientia, Humaniora, Caritas*).
   - Profil Pimpinan Sekolah, Dewan Guru, dan Yayasan Pengelola.
   - Galeri Fasilitas Kampus (Laboratorium Sains, Lab Komputer, Perpustakaan, Lapangan Olahraga, Kapel/Ruang Doa, dan Asrama Putra/Putri).
   - Rekam jejak akreditasi dan capaian alumni di perguruan tinggi kedinasan & PTN ternama.

3. **Halaman Akademik & Kesiswaan (`/akademik` & `/kesiswaan`)**:
   - Penerapan Kurikulum Merdeka Berbagi dan peminatan fase lanjut.
   - Statistik pendidikan (rasio pengajar-siswa, laboratorium aktif, tingkat kelulusan).
   - Program pembinaan karakter, rekoleksi, retret, dan kegiatan rohani Katolik.
   - Direktori Ekstrakurikuler: OSIS, Pramuka, Paduan Suara / Koor Gerejawi, Seni Musik Bambu & Tari Tradisional Toraja, PMR, Klub Robotika/IT, serta cabang olahraga.
   - Dokumentasi prestasi akademik dan non-akademik siswa.
   - Kalender akademik tahun ajaran berjalan.

4. **Kanal Berita & Acara / Warta Kampus (`/berita` & `/berita/[slug]`)**:
   - Berita Utama (*Featured News*) dengan visual besar berwibawa.
   - Pencarian warta dan filter kategori (Prestasi, Akademik, Kesiswaan, Kerohanian, Pengumuman Resmi).
   - Halaman detail berita dengan tipografi editorial yang nyaman dibaca, tanggal rilis, penulis, dan galeri foto.
   - Agenda & Kalender Kegiatan Mendatang (Jadwal ujian, perayaan misa sekolah, pameran karya, linimasa PPDB).

5. **Modul Sistem PPDB Online (`/ppdb`)**:
   - Pusat informasi PPDB: Jalur Prestasi, Jalur Beasiswa Afirmasi/Yayasan, dan Jalur Reguler/Tes.
   - Persyaratan berkas (Rapor SMP, Ijazah/SKL, Kartu Keluarga, Akta Kelahiran, Pas Foto, Surat Baptis jika ada).
   - Linimasa penerimaan gelombang 1 dan gelombang 2.
   - Informasi transparansi pembiayaan sekolah dan program beasiswa yayasan.
   - Informasi fasilitas asrama putra dan putri bagi siswa dari luar kota/daerah.
   - Formulir Pendaftaran Online bertahap (*multi-step registration*) dengan validasi input komprehensif.
   - Fitur upload berkas pendaftaran digital.
   - Pengecekan status pendaftaran dan cetak bukti nomor pendaftaran.
   - Layanan bantuan / Helpdesk konsultasi PPDB melalui WhatsApp dan formulir pesan cepat.

6. **Portal Akses Pengguna & Administrasi (`/portal` & `/admin`)**:
   - Autentikasi berbasis *Better-Auth* dengan kontrol peran (*Role-Based Access Control* - RBAC):
     - **Admin Utama / Sekretariat**: Pengelolaan warta, agenda kegiatan, pengaturan halaman profil.
     - **Panitia PPDB**: Peninjauan berkas pendaftar, verifikasi keabsahan data, pengubahan status seleksi (*Lolos Berkas*, *Tahap Wawancara*, *Diterima*, *Tidak Diterima*).
     - **Calon Siswa / Wali**: Akses dashboard pelacakan progres pendaftaran.

---

### Di Luar Cakupan (Out-of-Scope - Fase Lanjutan)

- Integrasi *payment gateway* perbankan otomatis (Fase 1 menggunakan verifikasi transfer manual/rekening resmi yayasan).
- Sistem Informasi Akademik (SIAKAD) lengkap untuk pengelolaan e-Rapor dan nilai harian.
- *Learning Management System* (LMS) untuk penugasan daring kelas dan ujian CBT internal.
- Forum diskusi alumni berjejaring sosial mandiri.

---

## 3. Alur Perjalanan Pengguna (User Flows)

1. **Calon Siswa / Orang Tua (Pendaftar PPDB)**:
   - Mendarat di Beranda -> Menyimak pilar nilai dan keunggulan sekolah -> Mengklik tombol **Daftar PPDB**.
   - Masuk ke halaman PPDB -> Memilih jalur pendaftaran (Prestasi / Beasiswa / Reguler).
   - Mengisi formulir identitas, asal sekolah, data orang tua, dan mengunggah dokumen digital.
   - Mengirimkan formulir -> Menerima Kode Registrasi & Kartu Bukti Pendaftaran digital.
   - Memantau halaman status pendaftaran menggunakan nomor registrasi / nomor telepon.

2. **Masyarakat Umum / Pemerhati Pendidikan**:
   - Mengunjungi Beranda -> Membaca visi-misi sekolah, profil sejarah sejak 1958, dan fasilitas asrama.
   - Mengakses menu Berita & Acara -> Memfilter berita prestasi siswa atau mencari agenda perayaan sekolah mendatang.
   - Menonton video profil sekolah melalui tautan pemutar video di beranda.

3. **Panitia PPDB & Pengelola Konten (Admin)**:
   - Masuk melalui halaman login portal staf.
   - Mengakses panel verifikasi berkas pendaftar PPDB: menyaring pendaftar berdasarkan jalur, memverifikasi lampiran berkas, dan memperbarui status pendaftar.
   - Mempublikasikan warta prestasi baru, mengunggah foto kegiatan, atau menambahkan agenda kegiatan baru ke kalender sekolah.

---

## 4. Struktur Halaman & Rute Aplikasi

| Path Rute | Nama Modul | Deskripsi Fungsi |
|---|---|---|
| `/` | Beranda Sekolah | Halaman beranda dengan hero, 4 pilar, program unggulan, warta, sambutan, testimoni, dan CTA |
| `/profil` | Profil & Sejarah | Sejarah pendirian 1958, visi-misi, pimpinan guru & yayasan, galeri fasilitas kampus & asrama |
| `/akademik` | Kurikulum & Peminatan | Kurikulum Merdeka, peminatan MIPA/IPS/Bahasa, kalender pendidikan, capaian kelulusan |
| `/kesiswaan` | Pembinaan & Ekskul | OSIS, Pramuka, Koor, Seni Budaya Toraja, olahraga, pembinaan rohani & retret Katolik |
| `/berita` | Warta & Acara | Direktori berita kampus, filter kategori, kotak pencarian, agenda kegiatan mendatang |
| `/berita/[slug]` | Detail Warta | Tampilan artikel berita lengkap, galeri dokumentasi foto, dan artikel terkait |
| `/ppdb` | Pusat Informasi PPDB | Panduan jalur masuk, jadwal pendaftaran, transparansi biaya/beasiswa, info asrama |
| `/ppdb/daftar` | Formulir PPDB Online | Formulir pendaftaran calon siswa baru bertahap dengan upload dokumen |
| `/ppdb/status` | Pelacakan Status | Pencarian progres pendaftaran dengan Nomor Registrasi / NISN |
| `/portal` | Gerbang Portal | Halaman masuk bagi siswa, alumni, dan staf sekolah |
| `/admin/*` | Panel Manajemen Admin | Dashboard verifikasi PPDB, manajemen warta, kalender acara, dan pesan masuk |

---

## 5. Standar Kualitas & Persyaratan Non-Fungsional

- **Performa**: Waktu muat awal (*First Contentful Paint*) di bawah 1.5 detik; optimasi aset gambar webp/avif dengan lazy loading.
- **Responsivitas**: Tampilan adaptif sempurna pada layar mobile (320px - 767px), tablet (768px - 1023px), dan desktop (1024px+).
- **Aksesibilitas & Tipografi**: Memenuhi standar kontras WCAG AA; tipografi elegan menggunakan kombinasi font serif berwibawa (*Playfair Display*) dan sans-serif (*Inter*) untuk keterbacaan tinggi.
- **Keamanan Data**:
  - Enkripsi data sensitif (NIK calon siswa/orang tua, dokumen identitas).
  - Validasi ketat terhadap jenis file unggahan dokumen (PDF, JPEG, PNG, maks. 2MB).
  - Proteksi anti-spam (Rate Limiting pada API registrasi dan form pesan).
  - Autentikasi sesi aman menggunakan cookie `HttpOnly` dan `SameSite=Lax`.

---

## 6. Rencana Verifikasi & Uji Terima

1. **Uji Validasi Form PPDB**: Memastikan formulir menolak input yang tidak valid dan berhasil memproses data pendaftar beserta berkas unggahan.
2. **Uji Tampilan & Visual**: Menguji kecocokan seluruh komponen visual dengan sistem desain `international_academy/DESIGN.md` pada perangkat ponsel dan layar monitor desktop.
3. **Uji Performa**: Evaluasi skor Lighthouse (Performance, Accessibility, Best Practices, SEO) dengan target minimal 90.
