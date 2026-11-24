# MHGU Desk — PWA Android (v11.0)

## Cara Upload / Update ke GitHub Pages
1. Ekstrak seluruh isi ZIP ini dan unggah **isi file-nya langsung** ke root repo `MHGU-Desk`: `index.html`, `sw.js`, `manifest.json`, `icon.svg`, `icon-192.png`, `icon-512.png`, `README.md`.
2. Tunggu ~1 menit sampai GitHub Pages selesai deploy.

## Perubahan di v11.0
- Penulisan ulang penuh blok CSS (`<style>`) dari nol bergaya Material 3 Android native (bersih dari konflik patch lama, 3 tema: Terang, Hangat/Sepia, AMOLED).
- Perbaikan alignment seluruh kartu Armor, Senjata, Monster, Bahan, dan Panduan agar judul & badge rata kiri sejajar stat.
- Tinggi Level Carousel kini dinamis mengikuti tinggi slide level aktif (bebas ruang kosong besar di bawah LV1/LV2).
- Tombol `Bandingkan` dan aksi di dalam kartu terbuka kini berkontras jelas (`--surface` + border) dan strip aksi membungkus rapi tanpa terpotong tepi kanan.
- Seluruh 8 database Gzip+Base64, Build Simulator, Pin/Wishlist lokal, dan Service Worker Network-First (`sw.js` v11.0) tetap utuh.
