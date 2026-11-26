# WEVSNAP — Keputusan Proyek dan Hal yang Perlu Dikonfirmasi

## Keputusan yang sudah jelas dari diskusi dan sumber

| Keputusan | Status | Dampak |
|---|---|---|
| Studi kasus Wevsnap | Tetap | Satu sistem untuk RPL dan tugas terkait |
| Fokus pengembangan awal | Tetap | Portal pelanggan dulu, lalu integrasi tim |
| Bahasa & stack | Dipilih kelompok melalui diskusi | PHP, Laravel, Blade, Tailwind CSS, Alpine.js, MySQL |
| Klien tanpa akun | Sesuai UML | Form publik dan cek status terbatas |
| Booking tidak otomatis diterima | Sesuai UML | Status awal Menunggu Konfirmasi, keputusan Admin/Owner |
| Owner memiliki akses Admin | Sesuai UML | `role` pada user internal |
| WhatsApp manual | Sesuai UML | Tombol tautan, tidak ada bot/API otomatis |
| Hasil disimpan berupa tautan | Sesuai UML | Tidak mengunggah video besar ke server |
| UI pelanggan ada desktop + mobile | Terlihat di Figma | Responsif, pola visual mengikuti Figma |
| Desain Figma sudah ada, coding belum dimulai | Status terakhir dari kelompok | Semua implementasi dalam panduan adalah **rencana** |

## Belum final — perlu diputuskan dan ditulis di sini sebelum coding terkait

| Topik | Usulan default aman | Yang harus dikonfirmasi |
|---|---|---|
| Cek progres publik | Kode booking acak + nomor WA tervalidasi, rate limit, pesan error generik | Apakah mitra setuju metode kode+WA, atau memakai tautan token/OTP? |
| DP/pembayaran | Hanya informasi dan konfirmasi manual, tanpa payment gateway | Langkah DP pada Figma akan dioperasikan bagaimana oleh mitra? |
| Harga/fasilitas paket | Diambil dari data mitra yang dikonfirmasi, bukan diarang | Paket mana aktif, harga sah, isi layanan final |
| Testimoni/portofolio | Gunakan aset dan testimoni asli berizin atau placeholder bertanda demo | Persetujuan publikasi konten |
| Struktur tabel user | **Dipilih dalam paket ini: `users`** Laravel untuk `Pengguna` internal | Tim memastikan semua modul memakai satu tabel yang sama |
| Penamaan tabel domain | **Dipilih dalam paket ini:** nama singular berbahasa Indonesia dari `DATA_CONTRACT.md` | Jangan mengubah tanpa PR dan pembaruan kontrak |
| Status proyek/hasil | Gunakan label status awal di `DATA_CONTRACT.md` | Persetujuan mitra akan alur kerja nyata |
| Informasi jam acara | Tanggal wajib, jam final dikelola Admin | Apakah klien harus memilih jam saat booking? |
| Hosting/domain dan backup | Baru pilih setelah aplikasi dan kebutuhan privasi jelas | Siapa operator, biaya, hosting, domain, HTTPS, backup |

## Format pencatatan keputusan baru

```md
### YYYY-MM-DD — Judul keputusan
- Masalah: ...
- Disetujui oleh: ...
- Keputusan akhir: ...
- Berkas terdampak: ...
- Alasan / catatan: ...
```

**Aturan:** AI tidak boleh mengubah kolom `Status` dari “belum final” menjadi “disetujui” tanpa keputusan nyata dari tim/mitra.
