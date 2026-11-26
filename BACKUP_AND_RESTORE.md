# Cadangan dan pemulihan

## Cakupan

Repositori GitHub ini **publik**. Kode dan data snapshot 26 November 2026 tersimpan dalam [arsip ZIP](reza-portfolio-2026-11-26.zip). Saat ini kode di dalam ZIP belum ditampilkan sebagai folder terpisah di GitHub. Folder Google Drive yang menyimpan salinan ZIP tetap **privat**. Tidak ada sinkronisasi otomatis.

ZIP berisi kode Next.js, aset bawaan `public/`, migrasi `drizzle/`, ekspor JSON dari API admin (`site-content`, `entries`, `gallery`, `messages`), dan media Netlify Blobs pada `backups/2026-11-26/media/netlify-blobs/`. PIN, token, session secret, dan koneksi database tidak disertakan.

## Memulihkan

1. Unduh dan ekstrak ZIP. Baca `README.md`, `docs/CONTENT_MAP.md`, dan `backups/2026-11-26/manifest.json`. Verifikasi SHA-256 berkas di manifest.
2. Siapkan proyek Netlify, pasang dependensi dengan `npm ci`, lalu terapkan migrasi di `drizzle/` pada database baru.
3. Atur rahasia lingkungan Netlify secara terpisah. Nilainya tidak tersedia dalam cadangan.
4. Pulihkan `site-content.json` ke `site_settings` key `content`, `entries.json` ke `entries`, `gallery.json` ke `gallery_photos`, dan `messages.json` ke `messages`. Pertahankan ID, slug, urutan, dan status terbit.
5. Unggah kembali objek dari `media/netlify-blobs/` ke store `media` dengan key yang sama.
6. Deploy lalu cek halaman publik, admin, foto, audio, dan tautan.

JSON adalah ekspor aplikasi, bukan dump SQL yang langsung bisa diimpor. Jika skema berubah, sesuaikan proses pemulihan.
