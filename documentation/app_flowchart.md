flowchart TD
  %% ─── PUBLIC PAGES ───
  Home["🏠 Beranda (/)"]
  Profil["📖 Profil & Sejarah (/profil)"]
  Akademik["🎓 Akademik & Kesiswaan (/akademik)"]
  Berita["📰 Berita & Acara (/berita)"]
  BeritaDetail["📄 Detail Warta (/berita/[slug])"]

  %% ─── PPDB FLOW ───
  PPDBInfo["📋 Pusat Informasi PPDB (/ppdb)"]
  PPDBForm["📝 Formulir Pendaftaran Online (/ppdb/daftar)"]
  Step1["Langkah 1: Pilih Jalur & Peminatan"]
  Step2["Langkah 2: Data Diri Calon Siswa"]
  Step3["Langkah 3: Asal Sekolah & Nilai"]
  Step4["Langkah 4: Data Orang Tua / Wali"]
  Step5["Langkah 5: Upload Berkas Dokumen"]
  Step6["Langkah 6: Konfirmasi & Submit"]
  PPDBSuccess["✅ Sukses - Bukti Registrasi & Nomor Pendaftaran"]
  PPDBStatus["🔍 Cek Status Seleksi (/ppdb/status)"]
  StatusResult{{"Status: submitted / under_review / accepted / rejected"}}

  %% ─── PORTAL & AUTH ───
  Portal["🔐 Gerbang Login Portal (/portal)"]
  AuthCheck{{"Valid Credentials?"}}
  AdminPanel["🖥️ Panel Admin & Panitia PPDB (/admin)"]
  UserDashboard["👤 Dashboard Siswa/Alumni"]

  %% ─── ADMIN WORKFLOWS ───
  PPDBAdmin["📊 Verifikasi Berkas Pendaftar"]
  UpdateStatus["✏️ Ubah Status Seleksi Pendaftar"]
  ManageContent["📢 Kelola Warta & Kalender Acara"]

  %% ─── MAIN NAVIGATION CONNECTIONS ───
  Home -->|"Klik Menu Profil"| Profil
  Home -->|"Klik Menu Akademik"| Akademik
  Home -->|"Klik Menu Berita"| Berita
  Home -->|"Klik DAFTAR PPDB"| PPDBInfo
  Home -->|"Klik Portal Siswa/Alumni"| Portal

  Berita -->|"Klik Judul Artikel"| BeritaDetail
  BeritaDetail -->|"Klik Artikel Terkait"| BeritaDetail

  %% ─── PPDB FLOW CONNECTIONS ───
  PPDBInfo -->|"Klik Daftar Sekarang Online"| PPDBForm
  PPDBInfo -->|"Klik Cek Status"| PPDBStatus
  PPDBForm --> Step1 --> Step2 --> Step3 --> Step4 --> Step5 --> Step6
  Step6 -->|"Submit Berhasil"| PPDBSuccess
  Step6 -->|"Validasi Gagal"| PPDBForm
  PPDBSuccess -->|"Pantau Progres"| PPDBStatus
  PPDBStatus --> StatusResult

  %% ─── AUTH FLOW ───
  Portal -->|"Masukkan Kredensial"| AuthCheck
  AuthCheck -->|"❌ Gagal"| Portal
  AuthCheck -->|"✅ Admin / Staff PPDB"| AdminPanel
  AuthCheck -->|"✅ Siswa / Alumni"| UserDashboard

  %% ─── ADMIN FLOW ───
  AdminPanel --> PPDBAdmin
  AdminPanel --> ManageContent
  PPDBAdmin -->|"Klik Detail Pendaftar"| UpdateStatus
  UpdateStatus -->|"Simpan Perubahan"| PPDBAdmin