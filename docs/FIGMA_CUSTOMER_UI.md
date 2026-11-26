# WEVSNAP — Kontrak Tampilan Pelanggan dari Figma

## 1. Sumber visual (jangan direka ulang)

**Figma:** https://www.figma.com/design/lVkhxKte29qJWZ2uE0coGa/IMK-PROJECT?node-id=712-3

File memiliki halaman:
- **03 – Portal Pelanggan Wevsnap (Desktop)** (`712:3`)
- **05 – Website Pelanggan Wevsnap (Mobile)** (`1394:3`)
- **02 – Portal Tim Wevsnap (Desktop)** (`297:3`) dan **06 – Portal Tim Wevsnap (Mobile)** (`1460:3`) — fase tim nanti.

Resolusi acuan: **desktop sekitar 1440 px** dan **mobile 390 × 844 px**. Implementasi tetap responsif di antara keduanya. Figma adalah *visual source of truth*, tidak otomatis menentukan implementasi database atau aturan bisnis.

## 2. Inventaris layar pelanggan yang sudah ditemukan

| Fitur | Frame desktop (Figma) | Frame mobile (Figma) | Prioritas |
|---|---|---|---|
| Beranda Pelanggan | `712:3134` | `1397:3` | P1 |
| Pilihan Paket | `712:4013` | `1397:91` | P1 |
| Detail Emerald Moment | `712:4617` | `1397:157` | P1 |
| Detail Sapphire Moment | `712:4930` | `1397:225` | P1 |
| Detail Diamond Moment | `712:5243` | `1397:293` | P1 |
| Portofolio | `1128:215` | `1397:361` | P1 |
| Form Booking | `712:3790` | `1398:511` | P1 |
| Form per paket | Tidak perlu menduplikasi jika bisa diparameterkan | `1398:584`, `1398:651`, `1398:718` | P1 |
| Permintaan Terkirim | `712:3901` | `1399:329` | P1 |
| Cek Progres | `712:4239` | `1399:371` | P1 |
| Hasil Cek Progres | `712:4541` | `1399:440` | P1/P2* |
| Variasi status progres | `1634:4908`, `1634:4976`, `1634:5044` | `1581:4957`, `1581:5002`, `1581:5047` | P2* |
| Akses hasil Drive | `1634:5133` | `1583:277` | P2* |
| Dialog data belum lengkap | `1634:4707`, `1634:4716` | `1629:1343`, `1587:377` | P1 |
| Tanggal tidak tersedia | `1771:1121` | `1771:1125` | P1 |
| Menu hamburger | — | `1572:640`, variasi `1629:1205` dkk. | P1 |

`*` Data progres nyata baru tersedia ketika Admin/Owner dan kru sudah dapat mengubah status; hingga saat itu hanya demo yang jelas diberi label **data simulasi**.

## 3. Detail perilaku antarmuka

### Beranda
- Identitas/branding Wevsnap; navigasi Beranda, Paket, Portofolio, Cek Progres, Booking/WhatsApp.
- Hero, galeri karya, pilihan paket, cara booking, testimoni, ajakan booking, dan kontak/footers.
- Figma menampilkan tahap “Pembayaran DP” pada ilustrasi cara booking; **jangan** langsung implementasi transaksi digital. Bila perlu, jelaskan pembayaran/manual konfirmasi Admin.
- Jangan memakai ulasan ilustratif Figma sebagai ulasan pelanggan sungguhan.

### Paket dan detail
- Tiga paket **Emerald Moment, Sapphire Moment, Diamond Moment**. Tampilkan data dari MySQL agar dapat diperbarui Admin nanti.
- Perbedaan kartu/halaman harus mengikuti Figma; **jangan berasumsi harga, fasilitas, atau urutan final tanpa memeriksa data desain dan validasi mitra**.
- Gunakan satu template Blade detail paket yang data-driven, bukan tiga salinan HTML dengan kode berbeda.

### Form booking
- Input inti sesuai Use Case: nama, nomor WhatsApp, jenis acara, paket, tanggal diminta, lokasi, catatan.
- Paket dapat terisi otomatis dari tombol “Ajukan Booking” pada halaman detail paket, tetapi tetap diverifikasi server.
- State tanggal aktif, tidak tersedia, dan input wajib kosong harus jelas sesuai Figma.
- Jika isian salah, pertahankan data lama dan tampilkan pesan dekat input; jangan simpan booking.
- Jika berhasil, simpan di DB dan arahkan ke halaman **Permintaan Terkirim** dengan `kode_booking` dan status **Menunggu Konfirmasi**, bukan otomatis “Disetujui”.

### Cek progres
- Figma menyediakan layar pencarian booking, pengisian kode dan nomor WhatsApp, hasil cek progres, dan variasi hasil per tahapan.
- Implementasikan pencarian lewat **POST** agar kredensial akses tidak tersebar lewat URL query log; gunakan rate limit dan error generik, jangan bocorkan informasi milik pelanggan lain.
- Tampilan tahapan (penerimaan booking, sesi foto/video, editing, upload/pengiriman) dibentuk dari data proyek/tugas yang diizinkan tampil, **bukan array dummy yang selamanya statis**.
- **Sistem akses berdasarkan kode + WhatsApp merupakan rencana implementasi untuk disetujui**, bukan metode keamanan yang dipastikan dalam dokumen UML. Gunakan kode acak sukar ditebak, normalisasi WA, pembatasan percobaan, serta pertimbangkan tautan bertoken aman jika disetujui.

### Kontak WhatsApp
- Tombol membuka `wa.me` atau tautan resmi nomor mitra. Nomor harus disetujui mitra sebelum dipublikasikan.
- Tidak mengirim pesan otomatis lewat API dan tidak membuat booking secara diam-diam.

## 4. Standar implementasi visual

1. Cocokkan screenshot Figma target **satu per satu**. AI harus memeriksa frame yang diminta sebelum coding.
2. Gunakan Tailwind yang konsisten untuk token warna, tipografi, radius, spacing, container, dan breakpoint; jangan menebak palet jika belum membuka frame.
3. Blade component reusable: `header`, `footer`, `button`, `package-card`, `input`, `booking-status`, `progress-step`, `mobile-nav`.
4. Alpine.js hanya untuk interaksi ringan: menu hamburger/overlay, galeri/slider kecil, pemilih tanggal, dialog. Operasi DB tetap melalui Laravel.
5. Gunakan asset yang berizin dari desain asli; optimasi ukuran gambar dan `alt` text.
6. Tampilkan state: loading, empty, error, success; focus keyboard, label form, dan kontras tetap terbaca.
7. Uji 390px, 768px, 1024px, 1440px tanpa horizontal scroll tak sengaja.

## 5. Rute web publik yang disarankan (belum dikoding)

| Metode | URI | Fungsi |
|---|---|---|
| GET | `/` | Beranda |
| GET | `/paket` | Pilihan Paket |
| GET | `/paket/{paket}` | Detail Paket |
| GET | `/portofolio` | Galeri |
| GET | `/booking` | Form Booking |
| POST | `/booking` | Validasi + simpan permintaan |
| GET | `/booking/terkirim` | Halaman sukses dari **flash session** setelah POST, tidak menaruh kode booking rahasia di URL |
| GET | `/cek-progres` | Form cek |
| POST | `/cek-progres` | Validasi kode/WA dan tampilkan ringkasan miliknya |

Rute ini **kontrak awal yang diusulkan**, bukan fakta bahwa route sudah ada di repository. Catat perubahan disetujui di `DECISIONS.md` sebelum implementasi anggota lain bergantung padanya.
