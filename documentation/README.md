# 📚 Dokumentasi — Portal Web & PPDB Online SMAS Katolik Makale

> Folder ini adalah pusat seluruh dokumentasi teknis dan arsitektur proyek **Portal Web Resmi & Sistem PPDB Online SMAS Katolik Makale**.

---

## 🗂️ Peta Dokumen Teknis

| Berkas | Deskripsi |
|---|---|
| [`project_requirements_document.md`](./project_requirements_document.md) | Persyaratan proyek, cakupan fitur (modul PPDB, beranda, profil, warta), dan target pengguna |
| [`tech_stack_document.md`](./tech_stack_document.md) | Penjelasan seluruh teknologi: Next.js 15, React 19, Tailwind v4, Drizzle ORM, PostgreSQL, Better-Auth |
| [`frontend_guidelines_document.md`](./frontend_guidelines_document.md) | Panduan sistem desain, palet warna, tipografi, komponen, dan struktur direktori frontend |
| [`backend_structure_document.md`](./backend_structure_document.md) | Arsitektur backend, skema database Drizzle ORM, dan spesifikasi API/Server Actions |
| [`app_flow_document.md`](./app_flow_document.md) | Alur perjalanan pengguna: pengunjung publik, pendaftar PPDB, admin, dan panitia seleksi |
| [`app_flowchart.md`](./app_flowchart.md) | Diagram alur Mermaid: peta navigasi lengkap dari beranda hingga panel admin |
| [`security_guideline_document.md`](./security_guideline_document.md) | Panduan keamanan: autentikasi, RBAC, perlindungan data NIK/NISN, keamanan upload berkas |

---

## 🎨 Desain & Referensi Visual Mockup

Folder-folder berikut berisi berkas mockup visual (*code.html* dan *screen.png*) dari setiap halaman utama website, serta file sistem desain referensi resmi:

| Folder | Konten |
|---|---|
| [`beranda_smas_katolik_makale/`](./beranda_smas_katolik_makale/) | Mockup HTML + screenshot halaman Beranda (Hero, 4 Pilar, Program Unggulan, Warta, Testimoni) |
| [`profil_sejarah_smas_katolik_makale/`](./profil_sejarah_smas_katolik_makale/) | Mockup HTML + screenshot halaman Profil & Sejarah Sekolah sejak 1958 |
| [`akademik_kesiswaan_smas_katolik_makale/`](./akademik_kesiswaan_smas_katolik_makale/) | Mockup HTML + screenshot halaman Akademik (Kurikulum Merdeka) & Kesiswaan (Ekskul) |
| [`berita_acara_smas_katolik_makale/`](./berita_acara_smas_katolik_makale/) | Mockup HTML + screenshot halaman Warta Kampus, Berita Unggulan & Kalender Agenda |
| [`ppdb_online_smas_katolik_makale/`](./ppdb_online_smas_katolik_makale/) | Mockup HTML + screenshot halaman Pusat Informasi & Formulir PPDB Online |
| [`international_academy/DESIGN.md`](./international_academy/DESIGN.md) | **Sistem Desain Resmi**: token warna, tipografi, spasi, komponen, dan panduan estetika *Elite Academic Editorial* |

---

## 🏫 Tentang Proyek

**SMAS Katolik Makale** adalah sekolah menengah atas berakreditasi Unggul (Akreditasi A BAN-S/M) yang berdiri sejak **1958** di Makale, Kabupaten Tana Toraja, Sulawesi Selatan. Sekolah ini mengemban misi pendidikan holistik dengan semboyan:

> *"Fides, Scientia, Humaniora"* — Iman, Ilmu Pengetahuan, dan Kemanusiaan

Platform ini dibangun sebagai portal web institusional modern untuk:
- Mempresentasikan identitas akademik yang berwibawa kepada publik luas.
- Memfasilitasi pendaftaran peserta didik baru (PPDB) secara daring (*online*) dari seluruh penjuru Tana Toraja dan daerah sekitarnya.
- Menjadi kanal resmi publikasi warta prestasi, pengumuman, dan agenda kampus.

---

## 🛠️ Perintah Pengembangan Utama

```bash
# Menjalankan server pengembangan lokal
npm run dev

# Menjalankan database PostgreSQL lokal via Docker
npm run db:up

# Menghasilkan file migrasi dari perubahan skema Drizzle
npm run db:generate

# Menerapkan migrasi ke database
npm run db:migrate

# Membuka antarmuka visual Drizzle Studio
npm run db:studio
```
