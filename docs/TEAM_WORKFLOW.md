# WEVSNAP — Aturan Kerja Bersama dengan AI, VS Code, dan GitHub

## 1. Aturan satu sumber

- Semua anggota **menggunakan paket Markdown yang sama** sebagai konteks AI.
- `main` adalah versi hasil review; jangan push langsung ke `main`.
- Figma (visual) dan UML (bisnis/relasi) adalah referensi, bukan lisensi mengarang fitur.
- Semua perubahan spesifikasi penting harus disepakati dan dicatat di `docs/DECISIONS.md`.

## 2. Cara anggota mulai

1. Clone repo resmi dan pastikan branch lokal `main` terbaru: `git checkout main && git pull origin main`.
2. Pilih tugas satu modul di `docs/IMPLEMENTATION_PLAN.md`.
3. Buat branch baru: `git checkout -b feature/customer-booking-form`.
4. Buka AI teman dan salin **seluruh isi** `MASTER PROMPT AI WEVSNAP.md`, lalu kirim prompt tugas modul dari `docs/AI_TASK_PROMPTS.md`.
5. Jalankan perubahan di proyek, periksa error dan tes, lalu commit/push:

```bash
git status
git add .
git commit -m "feat: implement customer booking form"
git push -u origin feature/customer-booking-form
```

6. Ajukan Pull Request ke `main`, minta review anggota lain, baru merge.
7. Anggota lain tarik `main` terbaru dan jangan mengandalkan potongan kode yang belum di-merge.

## 3. Aturan file bersama agar tidak konflik

| Berkas | Aturan |
|---|---|
| `routes/web.php` | Satu integrator review setiap perubahan route; hindari duplikasi URL/nama |
| `database/migrations/*` | Satu penanggung jawab skema inti; revisi dengan migration baru, jangan diam-diam edit migration yang telah dipakai anggota lain |
| `resources/css/app.css` | Satu pemilik token CSS/Tailwind; anggota lain gunakan utilities/komponen |
| `.env` | Lokal masing-masing, **jangan commit** |
| `composer.json`, `package.json` | Perubahan dependency harus disebut di PR dan dikomit lockfile terkait |
| `AGENTS.md` dan `docs/*.md` | Perubahan kontrak disertai review kelompok |

## 4. Format penyerahan tugas anggota ke integrator

```md
### Fitur
Nama fitur dan alasan.

### Branch / Commit
Nama branch dan hash commit atau tautan PR.

### File berubah
- path/file1
- path/file2

### Hasil yang sudah diverifikasi
- [ ] Tampilan mobile
- [ ] Tampilan desktop
- [ ] Validasi
- [ ] Tes yang dijalankan: (isi perintah dan hasil)

### Masih kurang / perlu integrasi
Daftar batasan apa adanya.

### Perubahan kontrak
Field DB, rute, nama status, komponen baru, atau tidak ada.
```

## 5. Aturan review pull request

- Tidak menerima kode yang mengganti stack tanpa persetujuan.
- Tidak menerima klaim “selesai” jika hanya mockup tanpa DB untuk fitur yang seharusnya dinamis.
- Tidak menerima kredensial, token, key, data pelanggan nyata, atau file backup.
- Pastikan tidak bertentangan dengan aturan **booking menunggu konfirmasi**, pelanggan tanpa akun, WhatsApp manual.
- Minta screenshot sebelum/sesudah di resolusi mobile dan desktop untuk pekerjaan UI.
- Setiap PR di-review minimal satu anggota lain.

## 6. Saat AI memberikan jawaban berbeda

Jangan menggabungkan semuanya sekaligus. Bandingkan dengan `AGENTS.md`, `PROJECT_SPEC.md`, `DATA_CONTRACT.md` dan Figma; pilih implementasi yang mengikuti kontrak. Jika kontrak belum memutuskan, catat opsi di `DECISIONS.md`, lalu sepakati bersama.
