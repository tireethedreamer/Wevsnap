# AGENTS.md — Aturan Wajib Semua AI Coding Wevsnap

**Berlaku untuk ChatGPT, Codex, Cursor, Claude, Copilot, dan AI lain yang bekerja pada source code repo ini.**

## 1. Sebelum mengubah kode

1. Baca `README.md`, `docs/PROJECT_SPEC.md`, `docs/FIGMA_CUSTOMER_UI.md`, `docs/DATA_CONTRACT.md`, `docs/DECISIONS.md`, dan `docs/TEAM_WORKFLOW.md` (sesuai tugas).
2. Periksa file repository yang ada. **Jangan menganggap project, framework, database, route, atau komponen sudah dibuat** jika tidak terlihat.
3. Sebutkan fitur yang dikerjakan, berkas yang akan disentuh, dampak database, dan kriteria selesai.
4. Jika keputusan mitra belum jelas, gunakan implementasi paling minimal yang tidak bertentangan dengan UML; tandai asumsi secara eksplisit dan jangan mengklaim telah disetujui.
5. Hormati modul orang lain; hindari memodifikasi file di luar lingkup tanpa alasan.

## 2. Stack yang wajib dipatuhi

- **PHP + Laravel**, server-rendered **Blade**, **Tailwind CSS**, **Alpine.js**, **MySQL**.
- Gunakan Laravel Routes, Controllers, Models (Eloquent), Form Requests, migrations, seeders, dan tests.
- Frontend responsif dan mendekati Figma **desktop 1440px dan mobile 390px**, tidak hanya screenshot statis.
- Gunakan versi stabil dan kompatibel; ikuti dokumentasi resmi saat menginstal Tailwind/Vite/Alpine. Jangan mengubah stack tanpa kesepakatan tim.
- Gunakan Bahasa Indonesia untuk label UI konsisten dengan Figma; nama kelas/code dapat berbahasa Inggris atau Indonesia secara konsisten setelah disepakati. Untuk **database** gunakan nama snake_case yang dikunci di `docs/DATA_CONTRACT.md`.

## 3. Aturan bisnis tidak boleh dilanggar

1. **Pelanggan tidak mempunyai akun/login.** Login hanya untuk Pengguna internal: Admin, Owner, Videografer/Editor.
2. Booking publik **selalu diajukan** dan **berstatus awal `menunggu_konfirmasi`**. Jangan otomatis menyetujui booking karena kalender tampak kosong.
3. Admin/Owner memeriksa dan memutuskan booking. Owner mempunyai akses fungsi Admin.
4. Tanggal yang tampil di website hanyalah **indikasi awal**. Fiksasi jadwal dan alternatif ditangani Admin/Owner.
5. WhatsApp adalah tautan komunikasi manual, **bukan WhatsApp API atau notifikasi otomatis**.
6. Fitur `Upload Hasil` berarti menyimpan **tautan Google Drive/media eksternal**, **bukan upload video besar ke server**.
7. Pembayaran DP muncul sebagai informasi pada desain Figma; **bukan persetujuan membangun payment gateway**.
8. Jangan menampilkan data pelanggan lain pada cek status/progres; gunakan kode booking yang sukar ditebak, verifikasi tambahan sesuai keputusan tim, rate limiting, dan pembatasan data.
9. Status booking berbeda dari status proyek/penugasan. **Jangan membuat status baru tanpa mencatat keputusan dan migrasi jika diperlukan.**
10. Jangan gunakan data testimonial fiktif sebagai testimoni asli. Gunakan placeholder bertanda simulasi sampai mitra memberikan data resmi.

## 4. Figma dan UML

- Sumber visual: `https://www.figma.com/design/lVkhxKte29qJWZ2uE0coGa/IMK-PROJECT?node-id=712-3` (Pelanggan Desktop) dan halaman **05 – Website Pelanggan Wevsnap (Mobile)**.
- Sumber model: `A_257006111022_TugasUmlArtifacts(2).docx`.
- Gunakan aset Figma asli yang berizin; jangan mengarang logo/warna/foto. **Jika AI tidak bisa membuka Figma, minta ekspor/screenshot frame relevan dan jangan mengklaim pixel-perfect.**
- Ulangi komponen (header, footer, paket, tombol, form input) sebagai Blade component/partial; jangan duplikasi markup tanpa alasan.
- Pertahankan navigasi, state aktif, menu mobile overlay, kalender dan validasi, serta status empty/error/success.

## 5. Keamanan dan kualitas

- Validasi server-side wajib (Form Request), CSRF, output escaping Blade, Eloquent binding, authorization, rate limiting lookup, session security; jangan menyimpan password mentah.
- **Jangan** membuat endpoint admin dapat diakses publik; gunakan middleware `auth` dan pemeriksaan role/policy.
- Batasan dan tipe FK mengikuti `docs/DATA_CONTRACT.md`. Gunakan `decimal` untuk nominal dan hindari floating-point untuk harga.
- Gunakan migrations, bukan mengubah tabel manual diam-diam; buat seed data demo dengan penanda nonproduksi.
- Jangan commit `.env`, API key, kredensial, nomor pelanggan asli, backup DB, atau data sensitif.
- Periksa layout 390px, 768px, 1024px, dan 1440px; cek keyboard, label form, focus state, kontras, kesalahan validasi.
- Buat tes untuk alur utama dan gagal. **Jangan mengaku tes lulus tanpa menjalankannya.**

## 6. Gaya kerja AI — WAJIB

Jika diminta membuat fitur:

1. Mulai dengan ringkasan pemahaman tugas dan file terpengaruh, singkat.
2. Jika benar-benar terhubung repository: inspect kode yang sudah ada, lalu edit secara minimal dan konsisten.
3. Jika tidak terhubung repository: berikan **kode lengkap per path file**, bukan potongan yang tidak tahu dipasang di mana, beserta instruksi menjalankan.
4. Sertakan migration/route/model/view/test yang diperlukan dan sebutkan urutan eksekusi.
5. Jelaskan apa yang **sudah dibuat**, **belum dibuat**, **perlu diintegrasikan**, dan tes yang **dijalankan/belum dijalankan**.
6. Usulkan pesan commit dan nama branch sesuai modul.
7. Jangan mengubah nama tabel, field, route, status, kelas, brand, dan alur bisnis yang telah disetujui tanpa mengumumkan dampaknya.

**Definition of Done:** hasil sesuai spesifikasi, responsif, tervalidasi, aman menurut cakupan fitur, tes berhasil atau keterbatasan dinyatakan, dan siap direview anggota lain. Tidak cukup hanya 'AI membuat kode'.
