# Peta situs dan data

Dokumen ini ditujukan untuk pembaca manusia maupun asisten AI yang akan melanjutkan pekerjaan di situs ini.

## Bagian situs

| Bagian | Kode utama | Sumber konten |
| --- | --- | --- |
| Beranda, Tentang, Karya, Galeri, Kontak | `app/page.tsx` dan komponen di `app/` | `site_settings` (`content`), `entries`, `gallery_photos` |
| Catatan | `app/catatan/page.tsx` | `entries` dengan `kind=post` |
| Detail catatan | `app/jurnal/[slug]/page.tsx` | `entries` dengan `kind=post` |
| Editor admin | `app/admin/` | API `app/api/admin/` |
| Pesan kontak | `app/api/contact/route.ts` | `messages` |
| SEO | `app/layout.tsx`, `app/sitemap.ts`, `app/robots.ts` | metadata dan konten situs |

## Tempat penyimpanan

- **Kode, migrasi, dan aset bawaan:** repositori Git ini. Gambar bawaan ada di `public/images/` dan `public/media/`.
- **Konten editor:** Netlify Database. Definisi tabel ada di `db/schema.ts`: `entries`, `gallery_photos`, `site_settings`, `messages`, dan `admin_pin`.
- **Unggahan baru:** Netlify Blobs store `media`; file bisa diakses lewat `app/api/media/[key]/route.ts`.
- **Pengaturan rahasia:** variabel lingkungan Netlify. Nilainya sengaja tidak masuk cadangan Git.

## Isi snapshot

`backups/2026-11-26/` berisi `site-content.json`, `entries.json`, `gallery.json`, `messages.json`, dan `media/netlify-blobs/`. JSON tersebut adalah hasil API admin pada tanggal snapshot, termasuk draf jika ada. `gallery.json` dapat berisi galeri bawaan ketika tabel galeri kosong. Snapshot ini bukan dump SQL mentah dan tidak mencakup state pembatasan percobaan PIN (`admin_pin`). Lihat `manifest.json` untuk jumlah item dan checksum.

## Aturan kerja untuk AI

1. Baca README dan dokumen ini sebelum mengubah situs.
2. Periksa konten aktif di database/admin sebelum mengubah teks bawaan di `lib/site-content-defaults.ts`; nilai aktif dapat menimpa nilai bawaan.
3. Jangan menaruh PIN, hash PIN, session secret, token, atau URL koneksi database dalam commit, log, atau jawaban.
4. Jangan mempublikasikan repositori atau folder cadangan tanpa persetujuan pemilik.
5. Setelah perubahan konten admin, buat snapshot baru dan perbarui manifest. Jangan menganggap snapshot lama sebagai data terkini.
