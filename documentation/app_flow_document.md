# App Flow Document
## Alur Perjalanan Pengguna & Navigasi Sistem SMAS Katolik Makale

Dokumen ini mendeskripsikan secara menyeluruh setiap skenario perjalanan pengguna (*user journey*), transisi halaman, interaksi antarmuka, dan penanganan kondisi galat (*error states*) pada aplikasi web SMAS Katolik Makale.

---

## 1. Alur Pengunjung Publik & Calon Siswa (Public Exploration Flow)

Pengunjung yang membuka URL utama aplikasi (`/`) disambut dengan landing page yang merefleksikan identitas akademik bergengsi:

1. **Mendarat di Beranda (`/`)**:
   - Pengunjung melihat bilah atas (*topbar*) yang memuat kontak resmi di Makale, Tana Toraja, serta tautan menuju *Portal Siswa* dan *Portal Alumni*.
   - Pada bagian *Hero*, pengunjung disajikan moto kehormatan *"Fides, Scientia, Humaniora"*, lencana akreditasi Unggul BAN-S/M, serta tombol aksi cepat **Jelajahi Sekolah** dan **Lihat Profil Video**.
   - Menekan tombol video akan membuka dialog modal (*video modal*) secara instan tanpa mengalihkan halaman, memutar tayangan profil sinematik kehidupan kampus SMAS Katolik Makale.
2. **Menyimak 4 Pilar Pendidikan**:
   - Melewati pita metrik struktural yang memaparkan 4 pilar utama: *Fides* (Iman Kristiani), *Scientia* (Ilmu Modern), *Humaniora* (Kearifan Budaya Toraja), dan *Lingkungan Aman* (Kampus Asri & Anti Perundungan).
3. **Eksplorasi Profil & Sejarah Sekolah (`/profil`)**:
   - Pengunjung mengklik menu navigasi **Profil**.
   - Halaman menampilkan linimasa berdirinya sekolah sejak 1958 di Makale, rekam jejak lebih dari 65 tahun pengabdian, visi-misi, nilai luhur *Veritas et Sapientia*, profil pimpinan sekolah, serta fasilitas laboratorium dan asrama putra/putri.
4. **Eksplorasi Kurikulum & Kesiswaan (`/akademik` & `/kesiswaan`)**:
   - Membaca program Kurikulum Merdeka, peminatan belajar (MIPA, IPS, Bahasa), dan kelas pembinaan olimpiade sains.
   - Meninjau kegiatan ekstrakurikuler (Pramuka, Paduan Suara, Seni Tari/Musik Bambu Toraja, PMR, Olahraga) dan program retret/rekoleksi kerohanian Katolik.

---

## 2. Alur Pendaftaran PPDB Online (Admissions Journey)

Alur ini dirancang sangat mulus (*frictionless*) guna memandu calon siswa baru dan orang tua dari berbagai daerah:

```
[Beranda / Navigasi] 
        ↓ Klik "DAFTAR PPDB"
[Pusat Informasi PPDB (/ppdb)]
        ↓ Pelajari Jalur, Jadwal, & Syarat
[Formulir PPDB Multi-Step (/ppdb/daftar)]
        ├─ Langkah 1: Pemilihan Jalur (Prestasi/Beasiswa/Reguler) & Peminatan
        ├─ Langkah 2: Data Diri Calon Siswa (NIK, NISN, TTL, Agama, Kontak)
        ├─ Langkah 3: Asal Sekolah (Nama SMP, Kota, Nilai Rapor)
        ├─ Langkah 4: Data Orang Tua / Wali (Nama, Pekerjaan, No. WhatsApp)
        ├─ Langkah 5: Unggah Berkas Digital (KK, Rapor, Akta, Surat Baptis)
        └─ Langkah 6: Pratinjau & Konfirmasi Pengiriman Data
        ↓ Klik "Kirim Pendaftaran"
[Halaman Sukses & Bukti Registrasi]
        ├─ Menerima Nomor Registrasi Unik (Contoh: PPDB-2025-0012)
        ├─ Unduh Bukti Tanda Terima Pendaftaran (PDF / Gambar)
        └─ Petunjuk Jadwal Tahap Selanjutnya (Verifikasi & Wawancara)
```

### Pengecekan Status Seleksi Mandiri (`/ppdb/status`)
1. Pendaftar membuka halaman pelacakan status seleksi.
2. Memasukkan Nomor Registrasi (atau NISN dan tanggal lahir).
3. Sistem menampilkan status terkini secara transparan:
   - `submitted`: *Pendaftaran Diterima, Menunggu Verifikasi Berkas*.
   - `under_review`: *Berkas Sedang Diperiksa Panitia PPDB*.
   - `interview_scheduled`: *Jadwal Wawancara Telah Ditentukan*.
   - `accepted`: *Selamat! Anda Diterima di SMAS Katolik Makale*.
   - `rejected`: *Mohon Maaf, Berkas Tidak Memenuhi Persyaratan*.

---

## 3. Alur Akses Warta, Pengumuman, & Kalender Kampus (`/berita`)

1. Pengunjung masuk ke kanal **Berita & Acara**.
2. **Berita Utama (*Featured News*)**: Menampilkan berita terpenting di posisi teratas dengan visual besar.
3. **Filter Kategori**: Pengunjung dapat menyaring arsip berdasarkan tab: *Semua*, *Prestasi*, *Akademik*, *Kesiswaan*, *Kerohanian*, atau *Pengumuman Resmi*.
4. **Pencarian Cepat**: Menggunakan kolom pencarian (*search bar*) untuk mencari berita atau agenda tertentu.
5. **Membaca Artikel Lengkap (`/berita/[slug]`)**: Memuat artikel terformat rapi dengan foto dokumentasi resolusi tinggi dan rekomendasi artikel terkait.
6. **Kalender Acara**: Memantau jadwal kegiatan mendatang (misalnya misa awal tahun ajaran, ujian semester, atau batas akhir pendaftaran PPDB).

---

## 4. Alur Portal Siswa & Alumni (`/portal`)

1. Pengguna mengklik tautan **Portal Siswa** atau **Portal Alumni** pada bilah informasi atas (*topbar*).
2. Sistem mengarahkan ke antarmuka login terpadu berbasis *Better-Auth*.
3. Pengguna memasukkan kredensial (email/nomor identitas dan kata sandi).
4. Setelah berhasil masuk:
   - Siswa aktif dapat melihat pengumuman internal dan tautan akademik.
   - Alumni dapat memperbarui data ikatan alumni dan direktori angkatan.

---

## 5. Alur Pengelolaan Panitia PPDB & Administrator Sekolah (`/admin`)

1. Staf panitia atau administrator masuk ke rute `/admin` dengan kredensial berhak akses (`role: admin` atau `role: staff_ppdb`).
2. **Dashboard Verifikasi PPDB**:
   - Menampilkan ringkasan metrik statistik pendaftar (total pendaftar, rasio jalur prestasi/reguler, kuota asrama).
   - Daftar tabel pendaftar dilengkapi filter status dan pencarian nama.
   - Panitia membuka berkas pendaftar, meninjau lampiran dokumen (pratinjau PDF kartu keluarga dan rapor).
   - Panitia memperbarui status pendaftar, mencatat catatan verifikasi (*admin notes*), atau mengundang wawancara langsung/daring.
3. **Manajemen Konten Warta & Kalender**:
   - Administrator menambahkan warta baru, mengunggah foto kegiatan, serta menetapkan apakah artikel menjadi *Featured News*.
   - Memperbarui tanggal agenda kegiatan pada kalender sekolah.

---

## 6. Penanganan Kondisi Khusus & Galat (Edge Cases & Error Handling)

1. **Galat Validasi Formulir PPDB**:
   - Jika calon siswa mengosongkan kolom wajib (misalnya nomor WhatsApp orang tua atau NIK), sistem menampilkan pesan kesalahan inline berwarna merah (*error token*) tepat di bawah kolom tersebut sebelum data dikirim.
2. **Galat Unggah Dokumen**:
   - File melebihi batas 2MB atau berekstensi yang tidak didukung (selain PDF/JPG/PNG) akan ditolak langsung oleh antarmuka dengan notifikasi peringatan (*toast notification* via Sonner).
3. **Pencegahan Pendaftaran Ganda**:
   - Jika NISN atau NIK sudah pernah didaftarkan pada tahun ajaran yang sama, sistem memberikan pesan peringatan informatif dan mengarahkan pengguna ke halaman cek status pendaftaran.
4. **Koneksi Terputus saat Submit**:
   - Tombol pengiriman menampilkan indikator pemuatan (*loading spinner*) dan dinonaktifkan sementara (*disabled*) guna mencegah pengiriman data berulang (*double submission*). Jika jaringan gagal, notifikasi ramah pengguna menyarankan untuk mencoba kembali tanpa menghapus data input formulir yang telah terisi.