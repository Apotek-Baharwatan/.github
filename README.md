# Apotek Baharwatan

<div align="center">

# 🏥 SISINFOHARWAT — Pharmacy Management System (PMS) Enterprise
### Ekosistem Manajemen Farmasi Terpadu Berbasis Regulasi Kemenkes RI, BPOM CDOB & Standar USP <795>

[![Next.js](https://img.shields.io/badge/Frontend-Next.js%2016-black?style=for-the-badge&logo=next.js)](https://github.com/Apotek-Baharwatan/frontend-apotek)
[![NestJS](https://img.shields.io/badge/Backend-NestJS%2011-E0234E?style=for-the-badge&logo=nestjs)](https://github.com/Apotek-Baharwatan/backend-apotek)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL%2016-336791?style=for-the-badge&logo=postgresql)](https://github.com/Apotek-Baharwatan/workspace-apotek)
[![Prisma](https://img.shields.io/badge/ORM-Prisma%206-2D3748?style=for-the-badge&logo=prisma)](https://github.com/Apotek-Baharwatan/backend-apotek)
[![TypeScript](https://img.shields.io/badge/Language-TypeScript%205.9-3178C6?style=for-the-badge&logo=typescript)](https://github.com/Apotek-Baharwatan)
[![Compliance](https://img.shields.io/badge/Compliance-BPOM%20CDOB%20%26%20SIPNAP-008080?style=for-the-badge)](https://github.com/Apotek-Baharwatan/workspace-apotek)

<p align="center">
  <b>Apotek Baharwatan</b> mengoperasikan solusi digital farmasi modular end-to-end yang mengintegrasikan Pusat Distribusi (Pusdis), seluruh cabang gerai apotek fisik, penatausahaan persediaan multi-satuan (Multi-UOM), kasir POS cepat, racikan berstandar Beyond Use Date (BUD), hingga pelaporan kepatuhan regulasi Kementerian Kesehatan RI.
</p>

</div>

---

## 🏛️ Repositori Ekosistem (Ecosystem Repositories)

| Repositori | Cakupan & Deskripsi | Teknologi Kunci | Tautan Repositori |
|:---|:---|:---|:---:|
| **Frontend Web & POS** | Dashboard operasional modern glassmorphism, POS kasir responsif, skrining resep 3 lapis, racikan obat, penerimaan faktur kurir, dan kontrol persediaan multi-cabang. | Next.js 16 (Turbopack, App Router), React 19, TypeScript 5.9, Tailwind CSS v4, Lucide Icons | [🔗 frontend-apotek](https://github.com/Apotek-Baharwatan/frontend-apotek) |
| **Backend REST API** | REST API engine performa tinggi, isolasi data multi-cabang (`X-Branch-Id`), pessimistic row locking (`SELECT FOR UPDATE`), 49 tabel relasional, dan RBAC stateles JWT. | NestJS 11, PostgreSQL 16, Prisma Client ORM 6, Passport JWT, Class Validator | [🔗 backend-apotek](https://github.com/Apotek-Baharwatan/backend-apotek) |
| **Workspace & Dokumentasi** | Dokumentasi arsitektur resmi (*Modular Monolith*), kamus data 49 tabel, model ERD, regulasi kefarmasian BPOM/Kemenkes, dan strategi pengujian 10 skenario UAT. | Markdown, ERD Model, Technical System Development (TSD), SRS Specification | [🔗 workspace-apotek](https://github.com/Apotek-Baharwatan/workspace-apotek) |
| **Organisasi & Profil** | Profil publik GitHub organisasi, template issues, standar kontribusi, dan alur kerja integrasi antar-repositori. | GitHub Org Profile, Community Health, Automation | [🔗 .github](https://github.com/Apotek-Baharwatan/.github) |

---

## 🛡️ Fitur Unggulan & Kepatuhan Farmasi (Pharmacy Protections)

### 1. Persediaan Fisik & Aturan Multi-UOM
* **Penyimpanan Satuan Terkecil (*Base Unit*):** Seluruh saldo fisik pada batch disimpan dalam satuan terkecil (Tablet, Kapsul, Botol, Tube, Ampul).
* **Konversi Multi-UOM Transparan:** Konversi otomatis dari pembelian kemasan besar (Box/Strip) ke satuan jual dengan faktor perkalian baku:
  $$\text{Kuantitas Base} = \text{Kuantitas Masukan} \times \text{Faktor Multiplier}$$
* **Kebijakan Nol Stok Negatif:** Larangan tegas saldo stok fisik di bawah nol melalui check constraint basis data (`CHECK (currentStock >= 0)`).

### 2. Alokasi FEFO & Early Warning System (EWS)
* **FEFO Real-time:** Pengeluaran batch obat diurutkan berdasarkan tanggal kedaluwarsa terdekat (`expiryDate ASC`).
* **Peringatan Kedaluwarsa Berlapis:**
  * Peringatan Dini: ED $\le 12\text{ bulan}$ (Prioritas mutasi/diskon).
  * Siaga Kritis: ED $\le 3\text{ bulan}$ (Pemberitahuan harian).
  * Blokir Otomatis: ED $\le 1\text{ bulan}$ otomatis dialihkan ke status karantina (*quarantine*).
* **Inspeksi QC CDOB Faktur PBF:** Penerimaan barang dari kurir PBF memvalidasi sisa masa simpan $> 12\text{ bulan}$ dan mencatat rantai dingin ($2\text{--}8^\circ\text{C}$).

### 3. Skrining Resep & Beyond Use Date (BUD)
* **Skrining Resep 3 Lapis:** Apoteker memverifikasi keabsahan Administratif, Farmasetik, dan Klinis sebelum obat diracik.
* **Kalkulasi BUD Sesuai USP <795>:**
  * **Puyer / Kapsul Kering:** $\min(\text{Tanggal Buat} + 180\text{ hari},\, \min(\text{ED Bahan}) \times 25\%)$.
  * **Larutan Oral Berair:** $\text{Tanggal Buat} + 14\text{ hari}$ (Simpan lemari pendingin $2\text{--}8^\circ\text{C}$).
  * **Salep / Krim / Pot Semi-padat:** $\min(\text{Tanggal Buat} + 30\text{ hari},\, \min(\text{ED Bahan}))$.
* **Etiket Digital Otomatis:** Cetak Etiket Putih untuk obat dalam (oral) dan Etiket Biru wajib bertanda **"OBAT LUAR - TIDAK BOLEH DITELAN"**.

### 4. Alur Resolusi Stok 3 Pintu
Saat resep pasien mengalami kekosongan stok di gerai, sistem kasir & farmasi menyediakan 3 opsi instan:
1. **Pintu 1 (Ambil Pusdis):** Pengiriman ekspres dari Pusat Distribusi Harwathan.
2. **Pintu 2 (Pinjam Apotek Terdekat):** Formulir peminjaman antar-gerai apotek mitra.
3. **Pintu 3 (Substitusi Generik / Racikan Setara):** Rekomendasi obat bioekivalen berzat aktif sama.

### 5. Regulasi Pelaporan SIPNAP Kemenkes
* Modul audit penatausahaan Narkotika, Psikotropika, dan Prekursor Farmasi.
* Ekspor berkas laporan resmi siap unggah ke portal SIPNAP Kementerian Kesehatan RI sesuai Permenkes No. 3 Tahun 2015.

---

## 👥 Matriks 8 Peran RBAC (Role-Based Access Control)

| Peran | Tanggung Jawab Utama | Batasan & Keamanan Khusus |
|:---|:---|:---|
| **OWNER / SUPERADMIN** | Akses penuh multi-cabang, konsolidasi laba-rugi, dan log audit transaksi. | Memegang hak konfigurasi global. |
| **SENIOR APOTEKER (APA)** | Verifikasi Surat Pesanan (SP) ber-SIPA, persetujuan CITO, laporan SIPNAP Kemenkes. | Otoritas tertinggi kepatuhan medis apotek. |
| **APOTEKER** | Skrining resep 3 lapis, penyiapan racikan BOM, dan eksekusi resolusi stok 3 pintu. | Wajib memiliki SIPA aktif. |
| **ASISTEN APOTEKER (TTK)** | Pelayanan resep dokter, penimbangan bahan racikan, cetak etiket, dan kasir POS. | Di bawah supervisi Apoteker. |
| **KASIR POS** | Transaksi kasir retail/OTC, buka/tutup shift kasir, pelunasan non-tunai (QRIS/Debit). | **Dilarang melihat HPP unit & margin keuntungan.** |
| **STAF GUDANG** | Penerimaan faktur distributor PBF (QC CDOB) dan pengiriman mutasi pasokan antarcabang. | Kontrol suhu rantai dingin dan nomor batch. |
| **PURCHASING** | Analisis batas reorder buffer (ROP) dan penyusunan draf usulan SP. | **Dilarang menyetujui (approve) SP sendiri.** |
| **FINANCE / ACCOUNTING** | Pelunasan hutang dagang (AP), pencatatan OPEX non-obat, rekonsiliasi kas harian, neraca. | Buku besar akuntansi bersifat *append-only*. |

---

## 🛠️ Ringkasan Arsitektur Teknologi

```
                                  [ Browser Klien / PC Kasir ]
                                               │
                                               ▼  (Port 3000)
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                      FRONTEND WEB — Next.js 16 (App Router & Turbopack)                     │
│  Tailwind CSS v4 • Glassmorphism Tokens • Type-Safe API Client • Dynamic Host Resolution    │
└──────────────────────────────────────┬──────────────────────────────────────────────────────┘
                                       │ HTTP / REST API (Bearer JWT, X-Branch-Id)
                                       ▼  (Port 3001)
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       BACKEND API — NestJS 11 (Modular Monolith)                            │
│   Auth Guard (RBAC) • Strict DTO Validation • Dynamic LAN CORS • Optimistic/Pessimistic Lock│
└──────────────────────────────────────┬──────────────────────────────────────────────────────┘
                                       │ Prisma ORM v6
                                       ▼  (Port 5432)
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                      DATABASE — PostgreSQL 16 Relational Engine                             │
│       49 Tabel Relasional • Foreign Key Constraints • Audit Trails • Non-Negative Stock     │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Panduan Memulai Cepat (Local Development)

### 1. Kloning Repositori
```bash
# Kloning frontend
git clone https://github.com/Apotek-Baharwatan/frontend-apotek.git
# Kloning backend
git clone https://github.com/Apotek-Baharwatan/backend-apotek.git
# Kloning workspace dokumentasi
git clone https://github.com/Apotek-Baharwatan/workspace-apotek.git
```

### 2. Jalankan Backend API
```bash
cd backend-apotek
pnpm install
npx prisma generate
npx prisma migrate deploy
pnpm start:dev
# Backend siap di http://localhost:3001
```

### 3. Jalankan Frontend Web
```bash
cd frontend-apotek
pnpm install
pnpm dev
# Frontend siap di http://localhost:3000
```

---

## 📄 Tata Kelola & Standar Kontribusi
Seluruh kontribusi kode wajib mematuhi aturan baku pada:
* Panduan Rekayasa Tertinggi: [`AGENTS.md`](https://github.com/Apotek-Baharwatan/workspace-apotek/blob/main/AGENTS.md)
* Kamus Data Otoritatif 49 Tabel: [`Docs/03-database/data-dictionary.md`](https://github.com/Apotek-Baharwatan/workspace-apotek/blob/main/Docs/03-database/data-dictionary.md)
* Matriks Verifikasi: 10 Skenario UAT Mandatori (*Exit Code 0 on Build & Typecheck*).

---

<div align="center">
  <sub>© 2026 Apotek Baharwatan. Hak Cipta Dilindungi Undang-Undang.</sub><br/>
  <sub>Kepatuhan Farmasi: Permenkes RI No. 73/2016, Permenkes No. 3/2015, dan Perka BPOM No. 9/2019 tentang CDOB.</sub>
</div>

