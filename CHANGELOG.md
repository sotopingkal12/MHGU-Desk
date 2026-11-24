# MHGU Desk — CHANGELOG & catatan serah-terima untuk AI

> Baca dulu sebelum mengubah apa pun. `README.md` sudah usang; file ini sumber kebenaran.

## Konteks
- PWA Android **pribadi** (pemilik + kawan), di GitHub Pages. Kawan cukup reload/buka ulang untuk dapat versi terbaru. Konten terus bertambah.
- Fokus saat ini: **matangkan dasar (UI/UX bersih, mudah, hangat). Jangan menambah fitur baru tanpa diminta.** Tidak butuh: dokumentasi publik, lisensi, atribusi, tombol install, CSP, refactor modul, backend.
- Balas pemilik dalam bahasa Indonesia.

## Arsitektur
- `index.html` (~1,6 MB): CSS + HTML + satu `<script>` dengan **10 database gzip+base64** (`RAW_*_GZ_B64`: ARMOR, GUIDES, IG, HBG, LBG, BOW, MON, ITEMS, CB, HAM; baris sangat panjang, **jangan dicetak**). Satu file disengaja agar kode dan data selalu sinkron. Ada **dua `<script>`**: skrip kecil di `<head>` (memasang tema sebelum cat pertama, v12.2) dan skrip besar di akhir `<body>`; `node --check` keduanya. Jenis senjata terdaftar di `WEAPONS_REGISTRY` (hbg, lbg, bow, ig, cb, ham).
- `sw.js`: navigasi network-first, aset statis cache-first. Rilis baru: naikkan `MHGU_APP_VERSION` (index.html) dan `CACHE_VERSION` + komentar header (sw.js).
- Gaya kode: ES5, banyak fungsi satu-baris, `onclick` inline + delegasi `data-act`. Teks dinamis ke HTML wajib lewat `esc()`.

## Bahasa ID/EN (area rawan, baca ini)
- String dinamis: `L(id, en)`. String HTML/JS statis berbahasa Indonesia diterjemahkan mesin kamus `I18N_EXACT` (+ observer) **berdasarkan kecocokan teks persis**.
- **Jika mengubah teks statis, ubah juga kunci kamusnya.** Regresi v12.0: judul Build diubah ("... / CB / Hammer") tapi kunci kamus tidak, sehingga tetap Indonesia di mode EN.
- Mesin hanya menerjemahkan `<main>` dan `sub-controls`; teks statis di modal/luar main harus diberi `id` dan diset lewat `L()` di `setAppLang()`.
- Atribut `title`/`aria-label` (sejak v12.2): diterjemahkan `i18nAttrs()` lewat kamus `I18N_EXACT` (teks ID asli disimpan di `data-i18n-title`/`data-i18n-aria`, dipulihkan saat kembali ke ID). Yang dinamis (disusun di JS) pakai `L()`. Tombol `#btn-lang-toggle` diatur terpisah di `applyStaticLang()`.
- Isi **Panduan (Guides) hanya berbahasa Indonesia** (data JSON ±33 KB), disengaja.
- Cara uji tanpa HP (bahasa): jalankan app di jsdom (npm `jsdom`; polyfill `DecompressionStream/Blob/Response` dari Node), pindah ke EN, kunjungi semua tab dan jenis senjata, cari teks berbahasa Indonesia; lalu kembali ke ID dan cari sisa Inggris.
- Cara uji tanpa HP (tata letak): jsdom TIDAK merender layout. Pakai Chromium (Playwright) viewport 390×844 dan 360×800, `serviceWorkers:'block'`: ukur `getBoundingClientRect` tombol, cari teks terpotong/melebar, ambil screenshot. Bandingkan dengan baseline sebelum menyerahkan perubahan.

## Konvensi UI (sejak v12.2)
- Area ketuk: kontrol utama (tombol topbar, tab, kartu aksi) berukuran nyata ≥ 40–44px; chip padat mendapat hit-area tak terlihat (`::after`, selalu lebih kecil dari separuh jarak antar-elemen agar tidak saling menimpa). Blok CSS ada di akhir `<style>`, diberi komentar `v12.2`.
- Ukuran font minimum 11,5px (label kapital kecil/lencana 11px; `.nb` 11px). Jangan menurunkannya lagi. Pengecualian yang disengaja: matriks peluru `.amx-*` (±11,1px, monospace di grid ketat).
- Warna/kontras ketiga tema TIDAK diubah.

## Riwayat versi
### v12.2 (2026-11-24) — nyaman di mata & di jempol, tanpa fitur baru
- Area ketuk: `.icon-btn` 44, tombol topbar 44, `.m-chip`/`.q-pill`/`.hn-tab`/`.build-cls-btn`/`.search-clear` ≥ 44; `.act-btn` (kartu) 40, `.lv-pill`/`.branch-jump`/`.gd-cta`/`.src-link`/`.lvl-step` 40 + hit-area tak terlihat (efektif ≥ 44); `.link-btn` 39 + hit-area. Chip padat (`.tree-chip` 30, `.mat-chip`/`.pt-chip` ±31, `button.sk-pill` 28) efektif ±36–39px (batas wajar di tabel/kartu padat).
- Font: semua deklarasi ≤ 11px dinaikkan (9–10,5px → 10,5–11,5px; 11px → 11,5px; label tab bawah 12px), termasuk 4 gaya inline di template JS.
- Tema: skrip kecil di `<head>` memasang tema tersimpan SEBELUM cat pertama (sebelumnya baru terpasang setelah skrip 1,5 MB selesai → kilatan terang untuk pengguna Sepia/AMOLED). Bila belum pernah memilih tema, mengikuti sistem (`prefers-color-scheme: dark` → AMOLED; selain itu Terang). `manifest.json`: `background_color` & `theme_color` `#000000` → `#f3f1ed` (splash tidak lagi hitam→krem; berlaku setelah Android memperbarui WebAPK atau app dipasang ulang).
- i18n: `title`/`aria-label` kini bilingual (`i18nAttrs()`, 11 kunci kamus baru + `L()` untuk tooltip filter skill).
- Bug: label slot senjata di tab Build masih "Senjata (HBG / LBG / Bow / IG)" (tanpa CB/Hammer) → "Senjata (semua jenis)" / "Weapon (any type)" (sisa v12.1).
- UI: pill aksen panjang (`.sk-pill.accent`, mis. "Siege: …", "RF: …") sebelumnya terpotong "…" di 360–390px → kini membungkus baris. Subjudul topbar tidak terpotong di layar 360px (media query ≤380px).
### v12.1 (2026-11-24) — audit UI/UX & perapian bahasa, tanpa fitur baru
- Bug: Build tab tidak ikut ganti bahasa (flag salah nama `simInitialized` → `_simReady`).
- Bug: ganti bahasa hanya me-render tab aktif; kartu terbuka di tab lain tertinggal bahasa lama. Kini semua tab di-render ulang.
- Perbaikan CB/Hammer: judul "1. Pilih Senjata & 5 Bagian Armor" (daftar jenis senjata dihapus agar tak basi), label picker "senjata/Weapon", kamus disamakan.
- Judul modal Set Finder kini mengikuti bahasa (`#sf-best-t`).
- Area ketuk `.lvl-step` dan `.link-btn` diperluas (+6px tak terlihat).
### v12.0
- Menambah Charge Blade (79) dan Hammer (131).
### v11.x
- v11.4 SW network-first; v11.3 mesin i18n; v11.0 CSS Material 3, 3 tema (Terang, Sepia, AMOLED).

## Yang masih tersisa (tunggu keputusan pemilik)
- Guides (Panduan) hanya berbahasa Indonesia (±33 KB JSON, 15 sub-materi); menerjemahkan butuh keputusan pemilik dan pemeriksaan istilah game.
- Chip padat (`tree-chip`, `mat-chip`, `pt-chip`, `sk-pill`) hanya efektif ±36–39px (bukan 44). Menaikkannya lagi akan membuat kartu/tabel bahan terlalu longgar; ubah hanya bila ada keluhan salah-ketuk.
- README.md di repo masih v11.0 (usang); CHANGELOG.md ini belum ada di repo sampai diunggah.
- Ide fitur yang **ditunda** (pernah dirancang, TIDAK ada di repo): deteksi update otomatis saat app kembali dibuka, modal Info app + riwayat pembaruan, tombol Reset cache, tautan build, backup file.
- Kontras warna ketiga tema sudah lolos (≥5,2:1). Tidak perlu diubah.
