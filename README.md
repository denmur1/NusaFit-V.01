# NusaFit Android v1.2.1 — Stable & Fixed

Versi ini memperbarui UI mengikuti konsep mockup modern NusaFit dan memperbaiki navigasi serta kestabilan tracking.

## Perbaikan utama
- Bottom navigation: Beranda, Riwayat, Peta, Statistik, Profil.
- Menu tidak lagi hanya bergantung pada dialog; setiap menu membuka layar yang sesuai.
- Peta dibuat ulang dengan fragment transaction yang aman dan `commitNowAllowingStateLoss`.
- Broadcast receiver didaftarkan/dilepas secara aman agar tidak menyebabkan crash.
- Tracking GPS menggunakan Foreground Service bertipe `location`.
- Tracking tetap berjalan ketika pengguna menekan tombol Home.
- Service tidak dihentikan ketika task aplikasi di-swipe dari Recents (`stopWithTask=false`).
- Status tracking disimpan sehingga UI dapat memulihkan status setelah Activity dibuat ulang.
- Notifikasi tracking permanen dapat diketuk untuk kembali ke NusaFit.
- Tombol pengaturan baterai membantu mengurangi pembatasan background dari sistem/ROM perangkat.
- Riwayat dan statistik dapat dibuka dari bottom navigation.
- Versi aplikasi 1.2.1 / versionCode 4.

## Catatan GPS di HP
Untuk tracking yang stabil:
1. Beri izin Lokasi.
2. Izinkan notifikasi jika diminta.
3. Mulai tracking ketika NusaFit sedang terbuka.
4. Setelah tracking dimulai, tekan Home. NusaFit akan tetap berjalan melalui Foreground Service dan menampilkan notifikasi tracking.
5. Pada perangkat tertentu seperti Vivo, aktifkan izin berjalan di latar belakang / tanpa pembatasan baterai untuk NusaFit jika sistem masih menghentikan layanan.

## Build GitHub Actions
Project tetap menggunakan GitHub Actions yang sudah ada. Workflow membangun Debug APK dan Signed Release APK menggunakan Gradle 8.13 dan JDK 17.

## Profil & Kalkulator Kesehatan v1.2.0
- Profil sekarang menyimpan jenis kelamin, tanggal lahir, usia yang dihitung otomatis, tinggi badan, dan berat badan.
- Halaman Profil menampilkan ringkasan BMI dengan visual gauge modern untuk pengguna dewasa.
- Untuk pengguna di bawah 18 tahun, aplikasi tidak memberikan target berat dewasa; penilaian pertumbuhan diarahkan ke BMI menurut usia/jenis kelamin dan kurva pertumbuhan yang sesuai.


## Perbaikan stabilitas v1.2.1
- Logo terlampir dipakai untuk ikon launcher, ikon round, adaptive icon Android 8+, dan splash Android 12+.
- Detail sesi (nama dan jenis olahraga) tetap dipertahankan ketika dialog izin Android muncul.
- Foto/video aktivitas memakai pemilih dokumen dengan izin baca yang dapat dipertahankan.
- Lampiran aktivitas disimpan oleh foreground service bersama hasil sesi, sehingga tidak bergantung pada Activity masih terbuka.
- Fragment Google Maps dibersihkan saat berpindah halaman untuk mencegah map/fragment stale setelah navigasi berulang.
- Jika Google Maps API key belum diisi, halaman menampilkan pesan yang aman; tracking GPS dan penyimpanan aktivitas tetap dapat digunakan.


## Google Maps — otomatis untuk semua pengguna

NusaFit menggunakan satu API key Google Maps milik aplikasi (Pilihan A). Pengguna tidak perlu memasukkan API key. API key diambil saat build dari GitHub Actions secret `MAPS_API_KEY` melalui Secrets Gradle Plugin dan disuntikkan ke manifest pada saat build. `secrets.properties` tidak disimpan di repository.

### Konfigurasi sekali di Google Cloud
1. Buat/ gunakan project Google Cloud milik NusaFit dan aktifkan **Maps SDK for Android**.
2. Buat API key khusus NusaFit.
3. Pada **Application restrictions**, pilih **Android apps**.
4. Tambahkan package name `com.nusafit.app`.
5. Tambahkan SHA-1 sertifikat **release** yang digunakan untuk menandatangani APK NusaFit. Jika APK juga diuji dengan debug signing, tambahkan SHA-1 debug secara terpisah.
6. Pada **API restrictions**, batasi minimal ke **Maps SDK for Android**.
7. Di repository GitHub NusaFit: **Settings → Secrets and variables → Actions → New repository secret**, buat `MAPS_API_KEY` dan isi dengan API key tersebut.

Setelah secret tersedia, setiap workflow release akan otomatis memasukkan API key ke APK. Tidak ada kolom API key yang perlu diisi pengguna.

**Penting:** API key Android memang berada di aplikasi hasil build. Keamanannya bergantung pada pembatasan package name + SHA-1 dan pembatasan API di Google Cloud.
