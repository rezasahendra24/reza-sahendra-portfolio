# Cadangan dan pemulihan

## Cakupan cadangan

GitHub menyimpan riwayat kode, aset bawaan, serta snapshot JSON dan media unggahan pada folder `backups/2026-11-26/`. Google Drive menyimpan arsip ZIP dari repositori pada tanggal yang sama. Kedua lokasi bersifat privat. Tidak ada sinkronisasi otomatis setelah snapshot dibuat.

Snapshot JSON diambil dari API admin (`site-content`, `entries`, `gallery`, `messages`). Media di `public/` sudah ada di Git, sedangkan file pada Netlify Blobs store `media` ada di `backups/2026-11-26/media/netlify-blobs/`. File rahasia lingkungan Netlify tidak ikut diekspor.

## Cara membuat cadangan baru

1. Pastikan kode lokal sudah sesuai dengan situs aktif; periksa `git status` dan commit terbaru.
2. Buat folder `backups/YYYY-MM-DD/`. Masuk ke admin situs, lalu ekspor respons JSON dari keempat API admin. Akses tersebut memerlukan sesi admin yang sah. Simpan dengan nama `site-content.json`, `entries.json`, `gallery.json`, dan `messages.json`.
3. Daftarkan objek store dengan `npx netlify blobs:list media --json`. Simpan setiap objek dengan `npx netlify blobs:get media <key> --output <lokasi>`.
4. Buat `manifest.json` berisi waktu, URL sumber, jumlah item, dan SHA-256 setiap berkas. Pastikan semua referensi `/api/media/...` dalam JSON punya file yang cocok.
5. Commit dan push ke repositori privat. Buat ZIP dari berkas yang dilacak Git serta snapshot, lalu unggah ke folder privat Google Drive. Verifikasi jumlah berkas dan checksum ZIP.

## Cara memulihkan

1. Clone repositori privat atau unduh ZIP dari Drive. Periksa `manifest.json` dan checksum sebelum memakai data.
2. Siapkan proyek Netlify baru, pasang dependensi dengan `npm ci`, dan terapkan migrasi dalam `drizzle/` pada database baru.
3. Pulihkan variabel lingkungan rahasia melalui pengaturan Netlify secara terpisah. Nilai lama tidak tersedia di cadangan ini.
4. Masukkan kembali konten JSON ke tabel yang sesuai atau melalui editor/API admin. `site-content.json` memetakan ke `site_settings` key `content`; `entries.json` ke `entries`; `gallery.json` ke `gallery_photos`; `messages.json` ke `messages`. Pertahankan ID, slug, urutan, dan status terbit sesuai data ekspor.
5. Unggah kembali objek dalam `media/netlify-blobs/` ke store `media` dengan **key yang sama**, agar referensi `/api/media/<key>` tetap bekerja.
6. Deploy, lalu cek halaman publik, editor admin, foto, pemutar audio, dan tautan. Jangan menyalin state `admin_pin` karena itu hanya pembatasan percobaan login.

JSON snapshot adalah ekspor aplikasi, bukan dump SQL siap impor otomatis. Jika skema aplikasi berubah di kemudian hari, sesuaikan proses impor dengan migrasi pada versi kode yang dipakai.
