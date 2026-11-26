# WEVSNAP — Panduan Implementasi Kolaboratif

> **Status:** Desain antarmuka pelanggan dan tim sudah tersedia di Figma; dokumen analisis/perancangan UML sudah dibuat; **coding belum dimulai** saat paket panduan ini disusun (26 November 2026). Jangan mengklaim fitur sudah berjalan sebelum diuji.

## Cara menggunakan paket ini

**Untuk teman yang memakai ChatGPT/Claude/Gemini tanpa akses repository:** buka dan **salin seluruh isi** [`MASTER PROMPT AI WEVSNAP.md`](./MASTER%20PROMPT%20AI%20WEVSNAP.md) ke awal percakapan mereka. Setelah itu, kirim tugas spesifik dari [`docs/AI_TASK_PROMPTS.md`](./docs/AI_TASK_PROMPTS.md). Jika mereka memiliki akses ke Figma dan dokumen UML, berikan tautan/berkas sumber juga.

**Untuk AI coding yang bekerja langsung di VS Code/repository:** simpan seluruh folder ini di root repository (berisi `AGENTS.md` dan folder `docs/`). Minta AI membaca `AGENTS.md` terlebih dahulu, kemudian dokumen yang relevan sebelum mengubah kode.

**Untuk anggota tim:** baca [`docs/TEAM_WORKFLOW.md`](./docs/TEAM_WORKFLOW.md), ambil satu tugas, buat branch terpisah, dan ajukan Pull Request. Hindari perubahan ke file bersama tanpa komunikasi.

## Keputusan teknologi

| Lapisan | Teknologi yang disepakati |
|---|---|
| Bahasa backend | PHP |
| Framework backend / rendering | Laravel + Blade |
| UI | Tailwind CSS |
| Interaktivitas ringan | Alpine.js |
| Database relasional | MySQL |
| Akses data | Eloquent ORM dan migration Laravel |
| Lingkungan kerja | Windows, VS Code, Laragon, GitHub |

**Arsitektur awal:** aplikasi Laravel monolitik (Blade di-render server); **jangan** mengubah menjadi React SPA / Next.js / Node.js API tanpa keputusan kelompok.

## Sumber acuan

1. **Sumber visual utama:** [Figma IMK-PROJECT — Pelanggan Desktop](https://www.figma.com/design/lVkhxKte29qJWZ2uE0coGa/IMK-PROJECT?node-id=712-3) dan halaman Figma *05 – Website Pelanggan Wevsnap (Mobile)*.
2. **Sumber alur bisnis/UML:** `A_257006111022_TugasUmlArtifacts(2).docx` (24 use case, 24 skenario, 10 kelas, 5 sequence diagram).
3. **Sumber perubahan:** persetujuan bersama kelompok/mitra, dicatat di `docs/DECISIONS.md`.

**Jika bertentangan:** kebutuhan operasional yang dikonfirmasi mitra > UML untuk aturan bisnis > Figma untuk penampilan/interaksi > panduan ini. **Jangan diam-diam mengganti rancangan**; dokumentasikan perbedaan dan minta keputusan kelompok untuk hal yang berpengaruh pada bisnis/data.

## Fokus pekerjaan saat ini

1. Pondasi proyek Laravel, MySQL, Vite/Tailwind/Alpine.
2. Halaman **pelanggan**: beranda, portofolio, paket, detail paket, form booking, permintaan terkirim, cek status/progres.
3. Penyimpanan pengajuan booking, status **Menunggu Konfirmasi**, kode booking unik, dan validasi.
4. Setelah itu integrasi portal Admin/Owner dan Videografer/Editor sehingga progres pelanggan berasal dari data operasional nyata.

Baca [`docs/PROJECT_SPEC.md`](./docs/PROJECT_SPEC.md) dan [`docs/FIGMA_CUSTOMER_UI.md`](./docs/FIGMA_CUSTOMER_UI.md) sebelum coding.

## Struktur repo yang dituju (bukan klaim sudah dibuat)

```text
wevsnap/
├── app/                 # Controllers, Models, Form Requests, Services
├── database/            # Migrations, seeders, factories
├── resources/views/     # Blade views / components
├── resources/css/       # Tailwind dan CSS proyek
├── resources/js/        # Alpine, interaksi ringan
├── routes/web.php       # Route publik dan internal
├── tests/               # Feature dan Unit Tests
├── AGENTS.md            # Aturan utama AI coding
└── docs/                # Spesifikasi dan kontrak kelompok
```

## Inisialisasi proyek (saat tim siap)

```bash
composer create-project laravel/laravel wevsnap
cd wevsnap
# Buat .env dari .env.example sesuai OS, lalu atur DB_* lokal
php artisan key:generate
npm install
php artisan migrate
composer run dev
```

**Catatan:** aktifkan Tailwind CSS dan Alpine.js sesuai dokumentasi versi stabil saat instalasi; jangan menyalin konfigurasi plugin yang tidak kompatibel dari tutorial lama. Laragon/MySQL harus dinyalakan. Jangan commit `.env`, key, password, backup database, atau data pelanggan asli. Commit `composer.lock` dan `package-lock.json`.

## Isi folder

- [`AGENTS.md`](./AGENTS.md) — perintah operasional untuk AI coding.
- [`MASTER PROMPT AI WEVSNAP.md`](./MASTER%20PROMPT%20AI%20WEVSNAP.md) — konteks lengkap untuk disalin ke AI teman.
- [`docs/PROJECT_SPEC.md`](./docs/PROJECT_SPEC.md) — kebutuhan bisnis dan batasan.
- [`docs/FIGMA_CUSTOMER_UI.md`](./docs/FIGMA_CUSTOMER_UI.md) — inventaris UI pelanggan dan cara menerapkannya.
- [`docs/DATA_CONTRACT.md`](./docs/DATA_CONTRACT.md) — tabel, relasi, status, dan rancangan migration.
- [`docs/IMPLEMENTATION_PLAN.md`](./docs/IMPLEMENTATION_PLAN.md) — urutan implementasi dan tes penerimaan.
- [`docs/TEAM_WORKFLOW.md`](./docs/TEAM_WORKFLOW.md) — aturan Git, perubahan bersama, review.
- [`docs/AI_TASK_PROMPTS.md`](./docs/AI_TASK_PROMPTS.md) — prompt siap pakai menurut modul.
- [`docs/DECISIONS.md`](./docs/DECISIONS.md) — keputusan tetap dan hal yang masih perlu validasi.
