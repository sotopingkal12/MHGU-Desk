# MHGU Desk — Paket PWA Siap GitHub Pages

## Cara Pasang Sekali Saja (Gratis, 2 Menit)
1. Buat repository baru di GitHub (misal `mhgu-desk`, Public).
2. Ekstrak ZIP ini, lalu upload seluruh file (`index.html`, `manifest.json`, `sw.js`, `icon.svg`, `icon-192.png`, `icon-512.png`) ke root repository.
3. Masuk ke **Settings → Pages → Build and deployment → Source: Deploy from a branch (`main` / `/root`) → Save**.
4. Tunggu ~1 menit, buka URL GitHub Pages di **Chrome Android**, lalu ketuk menu `⋮` → **Install App / Tambahkan ke Layar Utama**.

## Cara Update Tanpa Build Ulang APK
1. Setiap kali ada `index.html` versi baru, ganti `index.html` di GitHub dan naikkan `CACHE_VERSION` di baris 3 `sw.js` (misal `'mhgu-desk-v8.1'`).
2. Buka **MHGU Desk** di Android saat online → ketuk tombol **`↻`** di pojok kanan atas → ketuk **Terapkan**.
3. Seluruh data **Pin, Build, Progres Kurikulum, dan Skor Ujian** tetap aman tersimpan di perangkat.
