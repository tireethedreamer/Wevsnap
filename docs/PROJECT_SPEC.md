# WEVSNAP — Spesifikasi Kebutuhan Sistem (RPL)

## 1. Identitas dan batas proyek

**Judul:** Sistem Informasi Manajemen Proyek dan Booking Wevsnap.

**Mitra:** Wevsnap, layanan dokumentasi acara dan penyuntingan video.

**Masalah dari analisis kelompok:** informasi jadwal bercampur agenda pribadi, potensi benturan jadwal booking, informasi brief terlambat/tidak lengkap, dan pemantauan progres melalui banyak media. Tujuan aplikasi adalah **sentralisasi pengajuan booking, penjadwalan, proyek, penugasan, progres, dan pencatatan tautan hasil**.

**Status:** analisis/perancangan UML tersedia; desain pelanggan & tim di Figma; aplikasi Laravel belum dikoding pada tanggal penyusunan paket ini.

## 2. Aktor dan kewenangan

| Aktor | Akses | Perilaku utama |
|---|---|---|
| Klien (publik) | Tanpa akun | Paket, portofolio, pengajuan booking, kode booking, cek status/progres sesuai verifikasi, hubungi Admin di WhatsApp |
| Admin | Login | Data klien, booking, verifikasi jadwal, alternatif/penolakan, kalender, proyek, penugasan, progres |
| Owner | Login | **Mewarisi seluruh hak Admin**, serta tugas operasional yang sesuai kebutuhan mitra |
| Videografer/Editor | Login | Melihat tugas, deadline, memperbarui status/checklist, mencatat tautan hasil |

Dalam implementasi model internal dapat memakai satu tabel `pengguna`/`users` dan `role`; tidak perlu tabel `owner` terpisah hanya untuk generalization aktor.

## 3. Fungsi utama sesuai 24 Use Case Scenario

### Klien/publik (8)
1. Cek Status Booking
2. Melihat Paket Layanan
3. Mengajukan Permintaan Booking
4. Formulir Booking
5. Memilih Tanggal
6. Ketersediaan Jadwal
7. Chat Via Admin WhatsApp
8. Melihat Progres

### Admin/Owner dan autentikasi (12)
9. Login
10. Fiksasi Jadwal
11. Mengelola Data Klien
12. Mengelola Booking
13. Memproses Permintaan Booking
14. Memeriksa Ketersediaan Jadwal
15. Tawarkan Jadwal Lain
16. Menolak Booking
17. Mengelola Kalender
18. Mengelola Proyek
19. Memantau Progres
20. Menugaskan Pekerjaan

### Videografer/Editor & deadline (4)
21. Melihat Penugasan
22. Memperbarui Progres Pekerjaan
23. Upload Hasil
24. Mengecek Deadline

## 4. Aturan alur bisnis yang telah ditetapkan

### Alur pengajuan publik

1. Klien membuka daftar paket dan memilih paket acara.
2. Klien mengisi nama, nomor WhatsApp, jenis acara, tanggal yang diinginkan, lokasi, catatan, dan paket terpilih.
3. Sistem memvalidasi isian wajib dan paket masih aktif. Jika invalid, tampilkan field yang salah, jangan simpan booking.
4. Sistem bisa menampilkan **indikasi ketersediaan awal** berdasar jadwal yang ada; **bukan konfirmasi otomatis**.
5. Data valid disimpan sebagai `Booking` dengan status **Menunggu Konfirmasi**, `kodeBooking` unik, dan waktu pengajuan.
6. Halaman sukses menunjukkan nomor booking dan cara mengecek status. Admin/Owner memproses permintaan di tahap berikutnya.

### Alur keputusan Admin/Owner

1. Admin/Owner melihat booking berstatus menunggu konfirmasi.
2. Pemeriksaan jadwal melibatkan catatan kalender dan **penilaian manual kesiapan operasional**.
3. Jika tersedia dan disepakati klien: fiksasi jadwal, ubah status booking `Disetujui`.
4. Jika bentrok: tawarkan alternatif **secara manual via WhatsApp** dan catat tanggal alternatif/tindak lanjut; booking **belum otomatis disetujui**.
5. Jika klien menerima alternatif: **periksa ulang jadwal**, baru bisa fiksasi.
6. Jika permintaan tak dapat dipenuhi: Admin/Owner mencatat alasan dan mengubah status `Ditolak`.
7. Proyek dapat dibuat dari booking yang sudah disetujui.

### Alur proyek internal

1. Admin/Owner melengkapi brief dan informasi proyek.
2. Admin/Owner membuat penugasan kepada Pengguna Videografer/Editor dan mengatur deadline.
3. Pelaksana melihat tugas, mengubah progres, dan menandai checklist.
4. Pelaksana menyimpan URL hasil dari Google Drive/media eksternal beserta status pengiriman.
5. Admin/Owner memantau tugas dan progres. Klien yang memiliki akses sah hanya melihat ringkasan yang boleh dipublikasikan.

## 5. Prioritas implementasi

**Fase 1 — Pelanggan (sedang diprioritaskan; belum mulai coding):** Beranda, Portofolio, Paket, Detail Paket, Form Booking, Permintaan Terkirim, Cek Status/Progres.

**Fase 2 — Internal penting untuk operasional:** Login, Admin/Owner Booking, pemeriksaan jadwal, fiksasi/alternatif/penolakan, Proyek, Penugasan, update progres, Hasil.

**Fase 3 — Penyempurnaan:** Pengujian end-to-end, performa, izin akses, audit dasar, responsif, evaluasi mitra, deployment/backups.

## 6. Bukan cakupan yang disepakati

- Pelanggan membuat akun/login.
- Persetujuan booking otomatis.
- WhatsApp Bot/API atau pesan otomatis.
- Payment gateway/DP otomatis.
- Upload file video besar langsung ke server.
- Algoritma optimasi kapasitas kru kompleks.
- Harga final atau testimoni palsu seolah-olah data asli mitra.

Fitur di atas **bisa diusulkan**, tetapi jangan diimplementasikan sebagai keputusan final tanpa persetujuan kelompok dan mitra.

## 7. Acuan 7 diagram yang sudah ada dalam UML Artifacts

- 1 Use Case Diagram — 4 aktor, 24 use case.
- 1 Class Diagram — 10 kelas, 9 relasi utama.
- 5 Sequence Diagram: Mengajukan Permintaan Booking; Memproses Permintaan Booking; Menugaskan Pekerjaan; Memperbarui Progres Pekerjaan; Upload Hasil.

Activity Diagram dan Statechart yang dibahas dalam percakapan adalah **tambahan usulan**, bukan bukti sudah terintegrasi ke dokumen UML terakhir; jika diadopsi, cocokkan dengan aturan bisnis di atas.
