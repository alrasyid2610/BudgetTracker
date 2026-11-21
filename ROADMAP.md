# Roadmap Budget Tracker

Aplikasi Android pribadi untuk mencatat keuangan. Offline-first, data di SQLite lokal, backup ke Google Drive.

Aturan status: tulis `- Done` atau `- Belum` di bagian Status tiap milestone. Update setelah milestone diverifikasi di emulator/HP.

## Ringkasan

| Fase | Fokus | Status |
|---|---|---|
| 0 | Persiapan | - Belum |
| 1 | Fondasi data | - Belum |
| 2 | Input manual dan daftar transaksi (MVP) | - Belum |
| 3 | Anggaran dan laporan | - Belum |
| 4 | Backup dan restore manual | - Belum |
| 5 | Penyempurnaan | - Belum |
| 6 | Backup otomatis ke Google Drive | - Belum |
| 7 | Scan struk (ditunda) | - Belum |

Titik app mulai berguna dipakai sehari-hari: akhir Fase 2.

---

## Fase 0: Persiapan

Tujuan: project berjalan di HP/emulator.

- Cek lingkungan: versi Node, Android Studio (SDK + emulator), JDK.
  - Status: - Belum
- Buat project Vue 3 + Vite + TypeScript.
  - Status: - Belum
- Tambahkan Ionic Vue dan Capacitor, platform Android.
  - Status: - Belum
- Setup lint, format, dan Vitest.
  - Status: - Belum
- Jalankan app kosong di emulator atau HP (USB debugging).
  - Status: - Belum

Selesai kalau: halaman "Hello World" tampil di HP/emulator.

## Fase 1: Fondasi data

Tujuan: data bisa disimpan dan bertahan setelah app ditutup.

- Pasang `@capacitor-community/sqlite` dan inisialisasi koneksi.
  - Status: - Belum
- Sistem migrasi bernomor (`schema_migrations`).
  - Status: - Belum
- Migrasi 001: tabel `categories`, `transactions`, `budgets`.
  - Status: - Belum
- Repository: `categoryRepo`, `transactionRepo`, `budgetRepo`.
  - Status: - Belum
- Seed kategori default (makanan, transport, tagihan, belanja, hiburan, kesehatan, gaji, lainnya).
  - Status: - Belum
- Unit test untuk repository dan utilitas uang/tanggal.
  - Status: - Belum

Selesai kalau: data tetap ada setelah app ditutup dan dibuka lagi, tes lulus.

## Fase 2: Input manual dan daftar transaksi (MVP)

Tujuan: bisa mencatat keuangan harian.

- Form tambah transaksi (jumlah, tipe, kategori, tanggal, catatan).
  - Status: - Belum
- Ubah dan hapus transaksi (dengan konfirmasi hapus).
  - Status: - Belum
- Daftar transaksi per bulan, dikelompokkan per tanggal, dengan navigasi bulan.
  - Status: - Belum
- Filter berdasarkan tipe dan kategori.
  - Status: - Belum
- Kelola kategori (tambah, ubah, hapus; blokir hapus kalau sudah dipakai).
  - Status: - Belum
- Format rupiah di semua tampilan.
  - Status: - Belum

Selesai kalau: kamu bisa mencatat, mengubah, dan menghapus transaksi di HP tanpa error.

## Fase 3: Anggaran dan laporan

Tujuan: tahu uang habis ke mana dan apakah sesuai anggaran.

- Anggaran per kategori per bulan.
  - Status: - Belum
- Indikator sisa anggaran dan peringatan hampir habis.
  - Status: - Belum
- Dashboard: total pemasukan, pengeluaran, dan saldo bulan ini.
  - Status: - Belum
- Grafik pengeluaran per kategori (Chart.js).
  - Status: - Belum
- Grafik tren bulanan.
  - Status: - Belum

Selesai kalau: angka dashboard cocok dengan hitungan manual dari daftar transaksi.

## Fase 4: Backup dan restore manual

Tujuan: data aman kalau HP hilang atau ganti perangkat.

- Export semua data ke file JSON (format sesuai CLAUDE.md).
  - Status: - Belum
- Kirim file ke Google Drive lewat share sheet (`@capacitor/share`).
  - Status: - Belum
- Import dari file backup dengan validasi `app` dan `schemaVersion`.
  - Status: - Belum
- Ringkasan data sebelum restore dan konfirmasi sebelum menimpa.
  - Status: - Belum
- Unit test untuk export/import (round trip: export lalu import menghasilkan data sama).
  - Status: - Belum

Selesai kalau: backup dari satu HP bisa direstore penuh di HP/emulator lain.

## Fase 5: Penyempurnaan

Tujuan: nyaman dipakai tiap hari.

- Notifikasi pengingat catat pengeluaran harian.
  - Status: - Belum
- Transaksi berulang (langganan, cicilan).
  - Status: - Belum
- Pencarian transaksi.
  - Status: - Belum
- Dark mode.
  - Status: - Belum
- Kunci app (biometrik atau PIN).
  - Status: - Belum
- Ikon dan splash screen.
  - Status: - Belum
- Build APK rilis yang ditandatangani, install ke HP.
  - Status: - Belum

Selesai kalau: APK rilis terpasang di HP dan dipakai harian tanpa masalah.

## Fase 6: Backup otomatis ke Google Drive

Tujuan: backup berjalan sendiri tanpa langkah manual.

- Project Google Cloud, OAuth client Android, dan akun sendiri sebagai test user.
  - Status: - Belum
- Google Sign-In di app.
  - Status: - Belum
- Upload backup JSON ke Drive dengan scope `drive.appdata`.
  - Status: - Belum
- Backup terjadwal (misalnya harian atau tiap ada perubahan) dan simpan beberapa versi terakhir.
  - Status: - Belum
- Restore dari Drive.
  - Status: - Belum

Selesai kalau: backup tersimpan otomatis di Drive dan bisa direstore dari sana.

## Fase 7: Scan struk (ditunda)

Tujuan: foto struk otomatis menjadi transaksi. Jangan dikerjakan sebelum Fase 1 sampai 5 selesai.

- Proxy PHP di VPS (Nginx + HTTPS), API key disimpan di server, akses dengan token.
  - Status: - Belum
- Kamera/galeri (`@capacitor/camera`) dan kompres gambar.
  - Status: - Belum
- Kirim ke proxy, terima JSON terstruktur (toko, tanggal, total, item, kategori).
  - Status: - Belum
- Layar konfirmasi dengan data yang bisa diedit sebelum disimpan.
  - Status: - Belum
- Simpan foto struk sebagai lampiran transaksi.
  - Status: - Belum
- Antrean offline untuk scan yang gagal karena tidak ada internet.
  - Status: - Belum
- Opsional: OCR ML Kit sebagai fallback untuk total dan tanggal.
  - Status: - Belum

Selesai kalau: foto struk menghasilkan transaksi yang benar setelah dikonfirmasi, dan scan yang gagal diproses ulang saat online.
