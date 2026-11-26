# WEVSNAP — Prompt Tugas per Modul (Siap Salin)

**Cara pakai:** 1) Di awal chat AI teman, tempelkan isi `MASTER PROMPT AI WEVSNAP.md` **sekali**. 2) Setelah itu, salin salah satu prompt di bawah pada **chat yang sama**. 3) Jika AI bisa mengakses repository/Figma, berikan akses yang tersedia. Jika tidak, lampirkan file yang perlu diedit.

## Prompt A — Setup Laravel dan integrasi stack

```text
TUGAS MODUL A: Setup awal proyek Wevsnap.
Ikuti seluruh MASTER PROMPT AI WEVSNAP yang sudah saya kirim. Saya menggunakan Windows + VS Code + Laragon + MySQL. Buat langkah setup Laravel, Blade, Tailwind CSS, Alpine.js dan Vite yang kompatibel dengan versi stabil terpasang. Jangan pakai React/Next.js. Jangan mengasumsikan instalasi berhasil sebelum diverifikasi. Sebutkan perintah terminal, struktur folder, file yang disentuh, langkah cek localhost & DB, serta perintah Git. JANGAN membuat fitur booking terlebih dahulu.
```

## Prompt B — Beranda dan komponen UI pelanggan

```text
TUGAS MODUL B: Implementasikan Beranda Pelanggan Wevsnap sesuai frame Figma desktop 712:3134 dan mobile 1397:3. Buat Blade components reusable untuk header, footer, CTA, navbar mobile, dan card paket; styling Tailwind, interaksi ringan dengan Alpine. Pertahankan branding dan aset Figma asli (minta screenshot/aset jika tidak bisa membukanya). Halaman harus responsif di 390px dan 1440px. Jangan buat data testimonial seolah asli. Berikan daftar path file/kode lengkap, nama branch, cara run, serta checklist verifikasi visual.
```

## Prompt C — Halaman Paket dan Detail Paket

```text
TUGAS MODUL C: Implementasikan daftar paket dan detail Emerald, Sapphire, Diamond dari Figma. Gunakan satu komponen/template Blade detail yang dinamis, ambil data paket dari MySQL lewat Laravel Eloquent, dan hanya tampilkan paket aktif. Jangan membuat harga/fasilitas final tanpa data resmi. Tombol Ajukan Booking harus mengirim ID paket terpilih ke formulir (divalidasi server). Berikan route, Model, Controller, View, seed demo jelas diberi label, dan Feature Test. Jangan mengubah skema tabel tanpa memeriksa DATA_CONTRACT.
```

## Prompt D — Form Booking dan status awal

```text
TUGAS MODUL D: Implementasikan Form Booking pelanggan dari Figma desktop 712:3790 dan mobile 1398:511 serta halaman Permintaan Terkirim. Data minimal: nama, WhatsApp, jenis acara, paket, tanggal diminta, lokasi, catatan. Buat server-side validation, cek paket aktif, simpan klien dan booking dengan kode acak unik serta status menunggu_konfirmasi, bukan disetujui otomatis. Tampilkan error field dan sukses sesuai Figma. Buat Laravel migration/model/relationship, Form Request, controller, route, Blade view, dan Feature Tests; jika migration inti sudah ada, gunakan yang ada dan jangan duplikat. Sertakan kode lengkap per file dan perintah uji.
```

## Prompt E — Cek Progres dan keamanan akses

```text
TUGAS MODUL E: Buat halaman Cek Progres pelanggan dari Figma desktop 712:4239 & 712:4541 dan mobile 1399:371 & 1399:440. Data harus diambil dari DB, hanya booking milik pelanggan yang lolos mekanisme verifikasi disetujui. Jika metode verifikasi belum diputuskan, jangan mengklaim final; ajukan rekomendasi kode acak + verifikasi tambahan, throttling, dan POST agar kode tidak tampil di URL. Jangan tampilkan progres palsu sebagai data nyata; buat empty state saat proyek belum dibuat. Sertakan route, controller, query, view, test valid/invalid, pengujian kebocoran data dan rate limit.
```

## Prompt F — Audit integrasi UI, backend dan database

```text
TUGAS MODUL F: Review kode repo Wevsnap dari beberapa branch yang telah digabung. Jangan mengganti seluruh arsitektur. Cocokkan dengan AGENTS.md, PROJECT_SPEC.md, DATA_CONTRACT.md, dan Figma pelanggan. Audit route, migration, relasi, booking status, akses pelanggan, broken navigation, validasi, CSRF, rate limiting, kualitas responsive. Laporkan masalah terurut prioritas, file dan lokasi masalah, perbaikan minimal, hasil tes yang benar-benar dijalankan, serta hal yang masih memerlukan persetujuan tim. Jangan mengklaim semuanya lulus tanpa bukti.
```

## Prompt G — Portal Tim setelah fase pelanggan

```text
TUGAS MODUL G: Rencanakan tahap berikutnya portal Admin/Owner dan Videografer/Editor, mengacu pada 10 kelas UML dan 5 Sequence Diagram. Jangan mengubah UI pelanggan atau membuat akun pelanggan. Prioritaskan login internal, pemeriksaan booking manual, fiksasi jadwal, proyek, penugasan, checklist, update progres, dan penyimpanan tautan hasil eksternal. Berikan pembagian modul dan kontrak endpoint/model agar bisa dikerjakan paralel tanpa konflik, tanpa mengklaim coding sudah dilakukan.
```
