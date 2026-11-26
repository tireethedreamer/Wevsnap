# WEVSNAP — Rencana Implementasi dan Pengujian

## Prinsip prioritas

Fokus **pelanggan dahulu**, tetapi desain data dan route disiapkan supaya modul Admin/Owner dan Videografer/Editor nanti bisa terhubung tanpa menulis ulang semua fitur. Semua checklist adalah **rencana**, bukan klaim sudah berjalan.

## Tahap 0 — Persiapan bersama

- [ ] Seluruh anggota mengunduh dan membaca `MASTER PROMPT AI WEVSNAP.md` / `AGENTS.md`.
- [ ] Tentukan satu repository GitHub resmi, maintainer utama, branch `main` terlindungi.
- [ ] Sepakati `docs/DATA_CONTRACT.md` dan isi `docs/DECISIONS.md`.
- [ ] Instal PHP, Composer, MySQL (Laragon), Node.js/npm, Git, VS Code.
- [ ] Buat proyek Laravel dan jalankan page default + koneksi DB lokal.
- [ ] Pasang dan konfigurasi Tailwind + Alpine sesuai dokumentasi resmi/versi terpasang.

## Tahap 1 — UI pelanggan (berdasarkan Figma)

- [ ] Header/footer/responsive navigation desktop-mobile, menu hamburger dan overlay.
- [ ] Beranda dengan hero, portofolio, CTA, cara booking, kontak.
- [ ] Halaman daftar paket Emerald/Sapphire/Diamond (data DB/seed contoh dengan penanda).
- [ ] Template detail paket dinamis; tombol Ajukan Booking membawa paket terpilih.
- [ ] Form booking: paket, nama, WA, acara, tanggal, lokasi, catatan; state invalid, valid, aktif.
- [ ] Halaman Permintaan Terkirim/berhasil; menampilkan kode booking dan status awal.
- [ ] Form Cek Progres; hasil hanya milik pemegang akses yang sah.
- [ ] Layout tidak pecah di 390px, 768px, 1024px, 1440px.

## Tahap 2 — Booking tersimpan di MySQL

- [ ] Migration inti `Klien`, `PaketLayanan`, `Booking` dan relasinya.
- [ ] Form Request + Controller + service/query untuk submission valid.
- [ ] Cek paket aktif dan tanggal valid; buat kode booking acak, simpan `menunggu_konfirmasi`.
- [ ] Indikasi ketersediaan tanggal (tanpa konfirmasi otomatis).
- [ ] Proteksi cek status: kode booking + verifikasi, throttling, pesan error generik.
- [ ] Tests: valid, invalid, paket nonaktif, unik kode, status awal, kebocoran data.

## Tahap 3 — Portal tim (agar sistem nyata end-to-end)

- [ ] Login internal + role Admin/Owner/Videografer/Editor.
- [ ] Daftar booking, detail, pemeriksaan jadwal, alternatif, tolak, fiksasi.
- [ ] Kalender dan proyek dari booking disetujui.
- [ ] Penugasan, deadline, checklist, progres.
- [ ] URL hasil eksternal + status pengiriman, view ringkas untuk pelanggan.
- [ ] Role authorization dan validasi kepemilikan penugasan.

## Tahap 4 — Pengujian dan kesiapan operasional

- [ ] Unit/feature test Laravel; manual click-through seluruh alur.
- [ ] Uji dua pengajuan di tanggal sama, validasi jadwal saat admin fiksasi.
- [ ] Uji data salah / link hasil invalid / booking tidak ada.
- [ ] Uji akses data antar pelanggan, rate limiting, role-based authorization.
- [ ] Uji mobile dan desktop, focus/keyboard, loading/error/empty.
- [ ] Backup DB, keamanan environment, HTTPS, hosting Laravel, monitoring dasar.
- [ ] Uji coba bersama mitra dan catat hasil penyesuaian.

## Matriks penerimaan minimum fitur pelanggan

| Skenario | Hasil yang wajib terlihat |
|---|---|
| Buka paket | Tiga paket yang **aktif** tampil sesuai data DB dan tata letak Figma |
| Submit data kosong | Validasi per field; tidak ada booking baru di DB |
| Submit data valid | Tercipta booking + kode unik; status `menunggu_konfirmasi` |
| Jadwal terlihat kosong | Booking **tetap** menunggu keputusan Admin |
| Tanggal indikasi bentrok | Muncul peringatan/opsi perubahan; bukan persetujuan otomatis |
| Cek kode salah | Tidak menampilkan data pelanggan lain; rate limit aktif |
| Cek kode dan verifikasi sah | Hanya status & ringkasan yang diizinkan |
| Tombol WhatsApp | Membuka chat manual menuju nomor resmi yang disetujui |
| Responsif mobile 390px | Tidak ada overflow tidak sengaja; form dan menu dapat dioperasikan |
| Data progres belum tersedia | Empty state jujur, tidak menampilkan progres palsu sebagai data nyata |

## Pembagian kerja berdasarkan modul (bukan nama orang)

- **Modul A — UI Foundation:** layout, komponen Blade, Tailwind, hamburger.
- **Modul B — Content:** beranda, portofolio, paket & detail.
- **Modul C — Booking:** form, validasi, controller, halaman sukses.
- **Modul D — Database:** migrations, model, seeders, relasi, kode booking.
- **Modul E — QA & Integrasi:** cek status/progres, test, merge, review responsive.

Pembagian final personel harus diputuskan kelompok; hindari dua anggota menulis ulang file yang sama.
