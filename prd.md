# PRD — Halaman Download GEOTRANS

## 1. Overview
Satu halaman landing untuk mendownload file APK aplikasi **GEOTRANS** (aplikasi absensi). File disimpan di Google Drive, halaman di-host di Cloudflare Workers.

## 2. Goals
- Pengguna membuka halaman → melihat info aplikasi → klik tombol → file APK langsung terunduh.
- Tampilan mobile-first (pengguna absensi memakai HP).
- Bahasa antarmuka: Indonesia.
- Satu versi aplikasi aktif (link Drive tunggal).

## 3. Non-goals
- Admin panel, login, user management.
- Database, penghitung unduhan, analytics.
- Multi-aplikasi / multi-versi.
- Logo, gambar brand, screenshot.
- Update checker, riwayat versi.

## 4. Teknologi
- Vanilla HTML + CSS + JS, semua UI dalam 1 file `public/index.html`.
- Dikirim/di-host oleh Cloudflare Worker tanpa build step.
- Tools: `wrangler deploy`.

## 5. Struktur Project
```
download/
├── prd.md
├── wrangler.toml
└── public/
    └── index.html
```

## 6. Alur Download
1. Konstanta `DRIVE_ID` (ID file Google Drive) disimpan di `index.html`.
2. Tombol Download APK menjadi tautan ke:
   `https://drive.usercontent.google.com/download?id={DRIVE_ID}&export=download`
3. Tautan `usercontent.google.com/...&export=download` memaksa unduhan langsung dari server Google Drive — tanpa membuka halaman konfirmasi Drive.
4. Ganti versi file: cukup ganti `DRIVE_ID` di one place.

## 7. Isi Halaman
- Judul: **GEOTRANS**
- Subjudul: "Aplikasi Absensi"
- Deskripsi singkat aplikasi
- Tombol besar: **Download APK**
- Informasi file: ukuran (MB) + nomor versi
- Footer: versi Android minimum, kontak/support

## 8. Data Konten (TODO — diisi saat file siap)
| Item | Nilai |
|------|-------|
| DRIVE_ID | — |
| Versi aplikasi | — |
| Ukuran file (MB) | — |
| Deskripsi singkat | — |
| Versi Android minimum | — |
| Kontak/support | — |

## 9. Kriteria Sukses
- Klik tombol pada HP → APK terunduh langsung.
- Halaman load < 2 detik, tampil rapi di layar 360px.
- Link Drive mati/hapus → halaman tetap berfungsi, hanya perlu ganti `DRIVE_ID`.

## 10. Risiko
- File di Google Drive dihapus/diganti → link mati. Mitigasi: `DRIVE_ID` mudah diganti.
- Google mengubah perilaku link `usercontent.google.com` → jaga fallback ke `https://drive.google.com/uc?export=download&id={DRIVE_ID}`.