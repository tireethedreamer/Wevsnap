# WEVSNAP — Kontrak Data dan Database (Rancangan)

> **PENTING:** Semua tabel di bawah adalah **rencana** dari Class Diagram 10 kelas, **belum migration yang sudah dijalankan**. Review dan sepakati sebagai satu tim sebelum membuat migration; jangan membuat skema berbeda di tiap AI.

## 1. Teknologi dan konvensi

- MySQL melalui Laravel Eloquent ORM + migration; gunakan `utf8mb4`, InnoDB, FK, index yang relevan.
- Nama tabel SQL dipakai **singular `snake_case` berbahasa Indonesia** (`klien`, `booking`, `proyek`, dan lainnya), kecuali akun internal pada tabel standar Laravel `users`. **Setiap model domain** wajib menyetel `protected $table` secara eksplisit. Nama kolom tetap `snake_case`.
- PK `id` unsigned big integer Laravel; FK `{entitas}_id` (`klien_id`, `booking_id`, dst.). `created_at` dan `updated_at` standar untuk audit aplikasi; data UML yang spesifik tetap disimpan di kolom terpisah.
- `nominal/harga` gunakan `decimal(12,2)`, **bukan float**.
- Password internal `password` di DB sebagai **hash** (padanan UML `passwordHash`); tidak pernah plaintext.
- Nilai status disepakati konsisten sebagai nilai machine-friendly, label UI diterjemahkan ke Bahasa Indonesia.

## 2. Pemetaan 10 kelas UML ke rancangan tabel

| Kelas UML | Nama tabel awal | Kolom inti dari UML | Hubungan/ketentuan |
|---|---|---|---|
| `Klien` | `klien` | `id`, `nama`, `nomor_whatsapp` | 1 klien → banyak booking |
| `PaketLayanan` | `paket_layanan` | `id`, `nama_paket`, `deskripsi`, `harga`, `durasi_jam`, `aktif` | 1 paket → banyak booking |
| `Pengguna` | `users` | `id`, `nama`, `username`, `password`, `role`, `aktif` | 1 pengguna → banyak penugasan |
| `Booking` | `booking` | `id`, `kode_booking`, `tanggal_pengajuan`, `jenis_acara`, `tanggal_diminta`, `tanggal_alternatif` nullable, `lokasi`, `catatan` nullable, `alasan_penolakan` nullable, `status_booking`, `klien_id`, `paket_layanan_id` | 1 booking → 0..1 jadwal, 0..1 proyek |
| `Jadwal` | `jadwal` | `id`, `tanggal_final`, `waktu_mulai`, `waktu_selesai`, `lokasi`, `status_jadwal`, `booking_id` unique | Booking boleh belum memiliki jadwal final |
| `Proyek` | `proyek` | `id`, `nama_proyek`, `brief`, `keluaran_video`, `status_proyek`, `catatan`, `booking_id` unique | Dibuat untuk booking disetujui |
| `Penugasan` | `penugasan` | `id`, `nama_tugas`, `deskripsi`, `deadline`, `status_pekerjaan`, `proyek_id`, `pengguna_id` | 1 proyek → banyak tugas |
| `ChecklistPekerjaan` | `checklist_pekerjaan` | `id`, `uraian`, `selesai`, `penugasan_id` | 1 penugasan → banyak checklist |
| `ProgresPekerjaan` | `progres_pekerjaan` | `id`, `waktu_pembaruan`, `status_baru`, `catatan`, `penugasan_id` | Riwayat pembaruan progres |
| `HasilPekerjaan` | `hasil_pekerjaan` | `id`, `tautan_hasil`, `status_pengiriman`, `waktu_pencatatan`, `penugasan_id` | Tautan hasil eksternal, bukan file video besar |

**Keputusan teknis yang dipakai paket ini:** `users` untuk akun Pengguna internal; nama tabel domain **singular** sebagaimana pada tabel. Jangan membuat tabel `pengguna` kedua untuk akun yang sama. Laravel pluralization tidak cocok untuk seluruh nama Indonesia, maka setel `protected $table` secara eksplisit pada setiap model. Jika seluruh tim sepakat mengganti konvensi, ubah dokumen dan migration lewat PR **sebelum fitur lain bergantung padanya**.

## 3. Relasi berdasarkan Class Diagram

1. `Klien 1 — 0..* Booking` (mengajukan)
2. `PaketLayanan 1 — 0..* Booking` (memilih)
3. `Pengguna 1 — 0..* Penugasan` (pelaksana)
4. `Booking 1 — 0..1 Jadwal` (jadwal final)
5. `Booking 1 — 0..1 Proyek` (proyek)
6. `Proyek 1 — 0..* Penugasan` (komposisi)
7. `Penugasan 1 — 0..* ChecklistPekerjaan` (komposisi)
8. `Penugasan 1 — 0..* ProgresPekerjaan` (komposisi)
9. `Penugasan 1 — 0..* HasilPekerjaan` (komposisi)

Foreign key untuk anak wajib **kecuali** skenario draft yang benar-benar disepakati; cascade delete perlu hati-hati terhadap histori booking/proyek. Untuk operasional, utamakan non-destruktif (`restrict`/`softDeletes` bila benar-benar diperlukan) daripada kehilangan audit trail.

## 4. Status: representasi teknis awal (harus konsisten)

| Domain | Nilai DB yang disarankan | Label UI |
|---|---|---|
| Booking | `menunggu_konfirmasi` | Menunggu Konfirmasi |
| Booking | `disetujui` | Disetujui |
| Booking | `ditolak` | Ditolak |
| Proyek | `belum_dimulai`, `berjalan`, `selesai` | Belum Dimulai, Berjalan, Selesai |
| Penugasan | `belum_dikerjakan`, `dikerjakan`, `selesai` | Belum Dikerjakan, Dikerjakan, Selesai |
| Hasil | `siap_dikirim`, `terkirim` | Siap Dikirim, Terkirim |

Status **booking** di atas mengikuti skenario UML. Nilai status **proyek/penugasan/hasil** merupakan normalisasi teknis awal dari contoh pada skenario; konfirmasi ke mitra sebelum dipakai sebagai enum final. Informasi tanggal alternatif **tidak otomatis mengubah booking menjadi disetujui**.

`kode_booking` harus unik, acak dan **tidak mudah ditebak**, bukan sekadar ID berurutan. Cek progres butuh autentikasi/otorisasi akses publik berbasis verifikasi yang telah diputuskan, serta rate limiting.

## 5. Integritas dan keamanan data

- Seluruh create booking dilakukan melalui transaksi DB saat diperlukan, validasi server-side, dan referential integrity.
- Setelah memilih paket, server cek lagi status `aktif`; jangan percaya `harga` yang dikirim browser.
- Cek benturan jadwal ulang pada fiksasi Admin/Owner, bukan hanya saat klien memilih tanggal; gunakan transaksi/locking jika ada operasi bersaing.
- Foto/testimoni portofolio hanya yang hak pakainya sudah disetujui.
- WA pelanggan adalah data pribadi: akses dibatasi, tidak diletakkan di URL/console logs publik.
- Password di-hash lewat fasilitas Laravel; authorization untuk portal internal.

## 6. Kewajiban sebelum tim membuat migration

- Pastikan semua anggota mengikuti nama tabel baku di bagian 2 (`users` untuk akun internal, tabel domain singular) sebelum migration pertama.
- Sepakati format kode booking dan verifikasi pelanggan.
- Putuskan apakah perlu kolom teknis tambahan (`slug`, `foto`, `urutan`, `is_public`, dll.) untuk Figma; jelaskan bahwa itu **tambahan implementasi**, bukan atribut asli Class Diagram.
- Tentukan hubungan jam/waktu untuk jadwal yang belum diketahui klien.
- Update dokumen ini setelah diputuskan, lalu **satu anggota membuat migration inti** supaya tidak terjadi konflik.
