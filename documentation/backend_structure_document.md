# Backend Structure Document
## Arsitektur Backend & Desain Skema Basis Data SMAS Katolik Makale

Dokumen ini menguraikan arsitektur backend, pemodelan data relasional dengan Drizzle ORM, mekanisme autentikasi, serta integrasi endpoint API pada situs web SMAS Katolik Makale.

---

## 1. Arsitektur Backend (Backend Architecture)

Sistem backend dibangun di atas runtime **Node.js** yang terintegrasi secara modular dalam **Next.js 15 (App Router)**. Pola arsitektur yang diterapkan adalah pola berlapis (*Layered Architecture*):

1. **Presentation / Route Layer**:
   - *Next.js Server Actions*: Menangani pengiriman formulir secara langsung dari antarmuka React 19 (misalnya pendaftaran PPDB, pencarian status seleksi, dan formulir konsultasi).
   - *Route Handlers (`/app/api/*`)*: Menyediakan endpoint RESTful JSON untuk kebutuhan konsumsi data asinkron, webhooks, atau integrasi pihak ketiga.
2. **Business & Validation Layer**:
   - Menggunakan skema deklaratif **Zod v4** untuk memvalidasi kelengkapan data, keabsahan nomor identitas (NIK/NISN), serta sanitasi input sebelum masuk ke lapisan query.
3. **Data Access Layer (ORM)**:
   - Menggunakan **Drizzle ORM** yang bertipe aman (*type-safe*) untuk berinteraksi langsung dengan basis data **PostgreSQL**.

---

## 2. Manajemen Basis Data (Database Management)

- **Sistem Database**: PostgreSQL 16 (Dijalankan via Docker Compose pada profil `postgres`).
- **Connection Pooling**: Dikelola secara efisien melalui driver `pg` bawaan Node.js dengan batas koneksi terkontrol.
- **Migrasi & Versioning**: Skema didefinisikan dalam berkas TypeScript di direktori `db/schema/`. Setiap pembaruan skema didokumentasikan melalui migrasi SQL yang dihasilkan oleh perintah `npm run db:generate` dan diaplikasikan via `npm run db:migrate`.
- **Inspeksi Data**: GUI web interaktif untuk pengelolaan data lokal menggunakan `npm run db:studio`.

---

## 3. Skema Basis Data (Database Schema)

### 3.1. Entitas Autentikasi & Pengguna (Better-Auth)

```typescript
// Tabel Pengguna Sistem (Admin, Guru/Staf, Calon Siswa, Alumni)
export const users = pgTable('users', {
  id: text('id').primaryKey(),
  name: text('name').notNull(),
  email: text('email').notNull().unique(),
  emailVerified: boolean('email_verified').default(false).notNull(),
  image: text('image'),
  role: text('role').default('user').notNull(), // 'admin' | 'staff_ppdb' | 'user'
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

// Tabel Sesi Pengguna
export const sessions = pgTable('sessions', {
  id: text('id').primaryKey(),
  userId: text('user_id').notNull().references(() => users.id, { onDelete: 'cascade' }),
  token: text('token').notNull().unique(),
  expiresAt: timestamp('expires_at').notNull(),
  ipAddress: text('ip_address'),
  userAgent: text('user_agent'),
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

// Tabel Akun Pihak Ketiga & Verifikasi
export const accounts = pgTable('accounts', {
  id: text('id').primaryKey(),
  userId: text('user_id').notNull().references(() => users.id, { onDelete: 'cascade' }),
  accountId: text('account_id').notNull(),
  providerId: text('provider_id').notNull(),
  accessToken: text('access_token'),
  refreshToken: text('refresh_token'),
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

export const verifications = pgTable('verifications', {
  id: text('id').primaryKey(),
  identifier: text('identifier').notNull(),
  value: text('value').notNull(),
  expiresAt: timestamp('expires_at').notNull(),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});
```

---

### 3.2. Entitas Sistem PPDB Online

```typescript
// Tabel Registrasi Pendaftar PPDB
export const ppdbRegistrations = pgTable('ppdb_registrations', {
  id: uuid('id').defaultRandom().primaryKey(),
  registrationNumber: varchar('registration_number', { length: 30 }).notNull().unique(), // Contoh: PPDB-2025-0012
  
  // Data Pribadi Calon Siswa
  fullName: varchar('full_name', { length: 150 }).notNull(),
  nik: varchar('nik', { length: 20 }).notNull(),
  nisn: varchar('nisn', { length: 20 }).notNull(),
  gender: varchar('gender', { length: 1 }).notNull(), // 'L' (Laki-laki) | 'P' (Perempuan)
  birthPlace: varchar('birth_place', { length: 100 }).notNull(),
  birthDate: date('birth_date').notNull(),
  religion: varchar('religion', { length: 50 }).notNull(), // 'Katolik', 'Kristen Protestan', dll.
  address: text('address').notNull(),
  phoneNumber: varchar('phone_number', { length: 30 }).notNull(), // WhatsApp Siswa
  
  // Asal Sekolah & Akademik
  originSchool: varchar('origin_school', { length: 150 }).notNull(),
  originSchoolCity: varchar('origin_school_city', { length: 100 }).notNull(),
  averageScore: decimal('average_score', { precision: 5, scale: 2 }),
  
  // Preferensi Pendaftaran
  admissionTrack: varchar('admission_track', { length: 30 }).notNull(), // 'prestasi' | 'beasiswa_yayasan' | 'reguler'
  academicInterest: varchar('academic_interest', { length: 20 }).notNull(), // 'mipa' | 'ips' | 'bahasa'
  needsDormitory: boolean('needs_dormitory').default(false).notNull(), // Fasilitas Asrama Putra/Putri
  
  // Data Orang Tua / Wali
  parentFatherName: varchar('parent_father_name', { length: 150 }).notNull(),
  parentMotherName: varchar('parent_mother_name', { length: 150 }).notNull(),
  parentPhone: varchar('parent_phone', { length: 30 }).notNull(), // WhatsApp Orang Tua
  parentOccupation: varchar('parent_occupation', { length: 100 }),
  
  // Status Seleksi Panitia
  status: varchar('status', { length: 30 }).default('submitted').notNull(),
  // Nilai status: 'submitted' | 'under_review' | 'interview_scheduled' | 'accepted' | 'rejected'
  adminNotes: text('admin_notes'),
  
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

// Tabel Berkas Lampiran Pendaftaran PPDB
export const ppdbDocuments = pgTable('ppdb_documents', {
  id: uuid('id').defaultRandom().primaryKey(),
  registrationId: uuid('registration_id').notNull().references(() => ppdbRegistrations.id, { onDelete: 'cascade' }),
  documentType: varchar('document_type', { length: 50 }).notNull(), 
  // 'kartu_keluarga' | 'rapor' | 'akta_kelahiran' | 'surat_baptis' | 'sertifikat_prestasi'
  fileName: varchar('file_name', { length: 255 }).notNull(),
  fileUrl: text('file_url').notNull(),
  fileSize: integer('file_size').notNull(), // dalam bytes
  mimeType: varchar('mime_type', { length: 100 }).notNull(),
  uploadedAt: timestamp('uploaded_at').defaultNow().notNull(),
});
```

---

### 3.3. Entitas Warta, Pengumuman, & Acara Kampus

```typescript
// Tabel Warta & Berita Sekolah
export const articles = pgTable('articles', {
  id: uuid('id').defaultRandom().primaryKey(),
  title: varchar('title', { length: 255 }).notNull(),
  slug: varchar('slug', { length: 255 }).notNull().unique(),
  excerpt: text('excerpt').notNull(),
  content: text('content').notNull(),
  category: varchar('category', { length: 50 }).notNull(), 
  // 'prestasi' | 'akademik' | 'kesiswaan' | 'kerohanian' | 'pengumuman'
  featuredImage: text('featured_image').notNull(),
  isFeatured: boolean('is_featured').default(false).notNull(),
  isPublished: boolean('is_published').default(true).notNull(),
  authorId: text('author_id').references(() => users.id),
  viewsCount: integer('views_count').default(0).notNull(),
  publishedAt: timestamp('published_at').defaultNow().notNull(),
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

// Tabel Agenda & Kalender Kegiatan
export const events = pgTable('events', {
  id: uuid('id').defaultRandom().primaryKey(),
  title: varchar('title', { length: 255 }).notNull(),
  description: text('description'),
  eventDate: timestamp('event_date').notNull(),
  endDate: timestamp('end_date'),
  location: varchar('location', { length: 150 }).default('Kampus SMAS Katolik Makale').notNull(),
  category: varchar('category', { length: 50 }).notNull(), // 'akademik' | 'rohani' | 'ppdb' | 'umum'
  createdAt: timestamp('created_at').defaultNow().notNull(),
});

// Tabel Pesan Masuk / Helpdesk PPDB
export const contactInquiries = pgTable('contact_inquiries', {
  id: uuid('id').defaultRandom().primaryKey(),
  name: varchar('name', { length: 100 }).notNull(),
  email: varchar('email', { length: 150 }).notNull(),
  phoneNumber: varchar('phone_number', { length: 30 }),
  subject: varchar('subject', { length: 200 }).notNull(),
  message: text('message').notNull(),
  isResolved: boolean('is_resolved').default(false).notNull(),
  createdAt: timestamp('created_at').defaultNow().notNull(),
});
```

---

## 4. Spesifikasi API Endpoints & Server Actions

| Metode / Action | Endpoint / Fungsi | Deskripsi | Otorisasi |
|---|---|---|---|
| `POST` | `submitPPDBRegistration(formData)` | Mendaftarkan calon peserta didik baru dan mengunggah berkas | Publik |
| `GET` | `/api/ppdb/status?code=...` | Mengambil status seleksi pendaftar berdasarkan nomor registrasi | Publik |
| `GET` | `/api/news?category=...&page=...` | Mengambil daftar artikel warta dengan filter kategori | Publik |
| `GET` | `/api/news/[slug]` | Mengambil rincian artikel warta lengkap dan menambah views count | Publik |
| `GET` | `/api/events` | Mengambil daftar kalender kegiatan mendatang | Publik |
| `POST` | `submitContactInquiry(data)` | Mengirimkan formulir konsultasi PPDB atau pesan kontak umum | Publik |
| `GET` | `/api/admin/ppdb/list` | Mengambil seluruh pendaftar PPDB untuk panel panitia | `staff_ppdb` / `admin` |
| `PATCH`| `/api/admin/ppdb/[id]/status` | Memperbarui status verifikasi berkas atau jadwal wawancara | `staff_ppdb` / `admin` |
| `POST` | `/api/admin/articles` | Menerbitkan warta berita atau pengumuman baru | `admin` |