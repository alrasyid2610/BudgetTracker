# Budget Tracker (Android, pemakaian pribadi)

Aplikasi pencatat keuangan pribadi untuk Android. Offline-first, data di SQLite lokal, backup ke Google Drive. Tidak dirilis ke Play Store (APK di-sideload).

Pemilik project: Harun. Bahasa yang dikuasai: PHP dan JavaScript. Komunikasi dalam bahasa Indonesia santai.

## Cara kerja dengan Claude

- Kerjakan per fase sesuai `ROADMAP.md`. Jangan loncat ke fase berikutnya sebelum fase sekarang selesai dan diverifikasi.
- Sebelum menulis kode besar, jelaskan rencana singkat dan tunggu konfirmasi kalau ada keputusan yang bisa dipilih lebih dari satu cara.
- Jangan menyimpulkan kondisi lingkungan (versi Node, Android SDK, emulator) tanpa mengeceknya atau bertanya dulu.
- Setelah selesai satu milestone, update status di `ROADMAP.md` dengan teks biasa: `- Done` atau `- Belum`. Jangan pakai emoji checkbox.
- Commit kecil dan sering, satu perubahan logis per commit, pesan commit dalam bahasa Inggris singkat.

## Keputusan stack (sudah final, jangan diganti tanpa diskusi)

| Lapisan | Pilihan |
|---|---|
| Kerangka app | Capacitor (build ke APK) |
| UI | Vue 3 (Composition API, `<script setup>`) + Vite + TypeScript |
| Komponen UI | Ionic Vue |
| State | Pinia |
| Routing | Vue Router (bawaan Ionic Vue) |
| Database lokal | SQLite via `@capacitor-community/sqlite` |
| Grafik | Chart.js (via `vue-chartjs`) |
| Notifikasi | `@capacitor/local-notifications` |
| Backup tahap awal | Export JSON + `@capacitor/share` (share ke Google Drive) |
| Backup otomatis (fase 6) | Google Sign-In + Drive API, scope `drive.appdata` |
| Scan struk (fase 7, ditunda) | `@capacitor/camera` + proxy PHP di VPS + AI vision |

## Aturan penting

- **Offline-first.** Semua fitur inti harus jalan tanpa internet. Jangan menambah dependensi yang butuh server untuk fitur dasar.
- **Fitur kamera / scan struk DITUNDA** sampai fase 7. Jangan menambahkan plugin kamera atau kode scan sebelum diminta.
- **Tidak ada backend** sampai fase 7. Satu-satunya backend nanti adalah proxy PHP kecil untuk scan struk.
- **API key tidak boleh ada di dalam app/APK.** Kalau nanti butuh, simpan di proxy server.
- **Jangan hardcode secret** (token, key, client ID) di repo. Pakai `.env` yang di-ignore git.
- Nominal uang disimpan sebagai **integer dalam rupiah** (tanpa desimal, tanpa float). Format tampilan: `Rp 1.250.000`.
- Tanggal disimpan sebagai string ISO 8601 (`YYYY-MM-DD` untuk tanggal transaksi, UTC untuk timestamp). Tampilkan sesuai zona waktu perangkat (Asia/Jakarta).
- Semua teks UI dalam bahasa Indonesia.

## Skema database (awal)

Perubahan skema harus lewat migrasi bernomor, jangan edit migrasi yang sudah jalan.

- `categories`: `id`, `name`, `type` (`income` | `expense`), `icon`, `color`, `is_default`, `created_at`
- `transactions`: `id`, `type` (`income` | `expense`), `amount` (integer), `category_id`, `date`, `note`, `created_at`, `updated_at`
- `budgets`: `id`, `category_id`, `month` (`YYYY-MM`), `amount` (integer), unik per (`category_id`, `month`)
- `schema_migrations`: `version`, `applied_at`

Aturan: hapus kategori yang sudah dipakai transaksi harus diblokir atau transaksi dipindahkan, jangan sampai ada data yatim.

## Struktur folder

```
src/
  main.ts
  router/
  views/            # halaman (Dashboard, Transaksi, Anggaran, Laporan, Pengaturan)
  components/       # komponen UI reusable
  stores/           # Pinia stores
  db/
    connection.ts   # inisialisasi SQLite
    migrations/     # 001_init.ts, 002_..., dst
    repositories/   # transactionRepo.ts, categoryRepo.ts, budgetRepo.ts
  services/
    backup.ts       # export/import JSON
  utils/
    money.ts        # format rupiah
    date.ts
  types/
android/            # project native hasil Capacitor (jangan edit manual kecuali perlu)
```

Akses database hanya lewat `repositories/`. Komponen dan store tidak boleh menulis SQL langsung.

## Perintah umum

```
npm install
npm run dev                 # jalankan di browser (SQLite pakai mode web / jeep-sqlite)
npm run build               # build web ke dist/
npx cap sync android        # salin build web ke project Android
npx cap open android        # buka di Android Studio
npx cap run android         # jalankan di emulator/HP
npm run lint
npm run test
```

Build APK dan tes di perangkat dilakukan di laptop pemilik (Android Studio). Di lingkungan cloud (Claude Code web), fokus ke kode web, logic, dan test unit. Jangan mencoba build APK di sana kecuali SDK terbukti tersedia.

## Konvensi kode

- TypeScript strict. Hindari `any`.
- Komponen Vue memakai `<script setup lang="ts">`.
- Nama file komponen `PascalCase.vue`, file lain `camelCase.ts`.
- Fungsi repository mengembalikan Promise dan tipe yang jelas, error dilempar dengan pesan yang bisa dipahami.
- Logic yang bisa dites (format uang, hitung ringkasan, validasi backup) ditaruh di `utils/` atau `services/` dan diberi unit test (Vitest).
- Jangan menambah library baru tanpa alasan yang jelas. Sebutkan alasannya saat mengusulkan.

## Format backup (JSON)

```json
{
  "app": "budget-tracker",
  "schemaVersion": 1,
  "exportedAt": "2026-11-21T12:00:00Z",
  "categories": [],
  "transactions": [],
  "budgets": []
}
```

Saat import: validasi `app` dan `schemaVersion`, tampilkan ringkasan (jumlah data) sebelum menimpa, dan minta konfirmasi. Format ini juga dipakai untuk backup otomatis ke Drive di fase 6.

## Definisi selesai (per fase)

- Fitur jalan di emulator/HP, bukan hanya di browser.
- Tidak ada error di console dan lint bersih.
- Data tetap ada setelah app ditutup dan dibuka lagi.
- Status fase di `ROADMAP.md` sudah diupdate.
