Baik, mari kita susun Product Requirements Document (PRD) untuk aplikasi pengelola stok bahan coffeshop Anda.

---

**Product Requirements Document (PRD)**

**Nama Produk:** CoffeStock - Aplikasi Pengelola Stok Bahan Coffeeshop

**Versi:** 1.0
**Tanggal:** 23 Mei 2024
**Penulis:** [Nama Anda / Tim]

---

**1. Pendahuluan**

Dokumen ini menjelaskan persyaratan fungsional dan non-fungsional untuk "CoffeStock", sebuah aplikasi mobile (Android & iOS) yang dirancang untuk membantu coffeeshop atau restoran kecil dalam mengelola stok bahan baku secara efisien. Tujuan utama aplikasi ini adalah untuk menyediakan alat yang sederhana, ringan, dan intuitif untuk melacak bahan masuk dari supplier, memantau penggunaan harian oleh kitchen dan bar, serta mencegah terjadinya kekurangan stok.

**2. Tujuan Proyek**

*   Mengurangi kerugian akibat bahan baku yang habis atau tidak terkontrol.
*   Meningkatkan efisiensi operasional dalam pengelolaan stok.
*   Menyediakan visibilitas stok real-time bagi manajemen coffeeshop.
*   Membuat aplikasi yang mudah digunakan oleh admin atau staff dengan minim pelatihan.
*   Membangun fondasi yang kuat untuk pengembangan fitur di masa depan.

**3. Target Pengguna**

*   **Admin Coffeeshop/Restoran:** Bertanggung jawab atas pembelian, pemantauan stok, dan pelaporan.
*   **Staff Kitchen/Bar:** Bertanggung jawab untuk mencatat penggunaan bahan harian.

**4. Fitur-fitur (Fungsional)**

Aplikasi ini akan mencakup fitur-fitur utama berikut:

**4.1. Manajemen Bahan Baku**
    *   **FR-001: Daftar Bahan Baku:** Pengguna dapat melihat daftar semua bahan baku yang tersedia, termasuk nama bahan, satuan (misalnya: gram, ml, pcs, kg, liter), dan jumlah stok saat ini.
    *   **FR-002: Tambah Bahan Baku:** Pengguna dapat menambahkan bahan baku baru ke dalam sistem dengan nama dan satuan.
    *   **FR-003: Edit Bahan Baku:** Pengguna dapat mengubah nama dan satuan dari bahan baku yang sudah ada.
    *   **FR-004: Hapus Bahan Baku:** Pengguna dapat menghapus bahan baku dari sistem (dengan konfirmasi).
    *   **FR-005: Kategori Bahan Baku:** Pengguna dapat mengelompokkan bahan baku ke dalam kategori (misalnya: Kopi, Susu, Sirup, Roti, Buah).
    *   **FR-006: Stok Minimum:** Pengguna dapat menetapkan batas stok minimum untuk setiap bahan baku.
    *   **FR-007: Satuan Khusus:** Aplikasi mendukung berbagai jenis satuan dan memungkinkan penambahan satuan kustom jika diperlukan.

**4.2. Pencatatan Bahan Masuk (Pembelian)**
    *   **FR-008: Tambah Transaksi Bahan Masuk:** Pengguna dapat mencatat bahan baku yang diterima dari supplier. Input meliputi:
        *   Tanggal transaksi (default hari ini).
        *   Nama bahan baku (pilihan dari daftar yang sudah ada).
        *   Jumlah yang masuk.
        *   Satuan (otomatis sesuai bahan, bisa diubah jika perlu).
    *   **FR-009: Pembaruan Stok Otomatis:** Setiap transaksi bahan masuk akan otomatis menambah jumlah stok yang tersedia untuk bahan tersebut.
    *   **FR-010: Riwayat Bahan Masuk:** Pengguna dapat melihat riwayat semua transaksi bahan masuk.

**4.3. Pencatatan Penggunaan Bahan**
    *   **FR-011: Tambah Transaksi Penggunaan:** Pengguna dapat mencatat penggunaan bahan baku oleh kitchen atau bar. Input meliputi:
        *   Tanggal transaksi (default hari ini).
        *   Nama bahan baku (pilihan dari daftar yang sudah ada).
        *   Jumlah yang digunakan.
        *   Satuan (otomatis sesuai bahan, bisa diubah jika perlu).
        *   Keterangan (opsional, misal: "Untuk Latte", "Buat Roti").
    *   **FR-012: Pembaruan Stok Otomatis:** Setiap transaksi penggunaan akan otomatis mengurangi jumlah stok yang tersedia untuk bahan tersebut.
    *   **FR-013: Riwayat Penggunaan Bahan:** Pengguna dapat melihat riwayat semua transaksi penggunaan bahan.

**4.4. Pemantauan Stok & Laporan**
    *   **FR-014: Dashboard Utama:** Menampilkan ringkasan status stok:
        *   Daftar bahan yang stoknya di bawah batas minimum (highlighted).
        *   Jumlah total bahan yang tersedia saat ini.
    *   **FR-015: Laporan Stok Detil:** Menampilkan daftar lengkap semua bahan baku beserta stok saat ini, satuan, dan status (Cukup, Peringatan (di bawah min), Habis).
    *   **FR-016: Notifikasi Stok Minimum:** Aplikasi akan memberikan notifikasi (di dalam aplikasi, notifikasi push opsional di masa depan) ketika stok suatu bahan mencapai atau berada di bawah batas minimum yang ditetapkan.
    *   **FR-017: Pencarian/Filter Bahan Baku:** Pengguna dapat mencari atau memfilter bahan baku berdasarkan nama atau kategori.

**4.5. Pengelolaan Supplier (MVP - Cukup Input Nama Saja)**
    *   **FR-018: Daftar Supplier:** Pengguna dapat menambahkan nama supplier. (Detail kontak dll. dapat ditambahkan di masa depan).

**5. Non-Fungsional Requirements (NFR)**

*   **NFR-001: Kinerja:** Aplikasi harus responsif dan ringan, dengan waktu muat layar kurang dari 2 detik pada perangkat menengah.
*   **NFR-002: Keamanan:** Data stok harus disimpan dengan aman secara lokal. (Otentikasi/Autorisasi lebih lanjut bisa dipertimbangkan di fase berikutnya).
*   **NFR-003: Usability:** Antarmuka pengguna harus intuitif, mudah dipelajari, dan minim langkah untuk tugas-tugas umum.
*   **NFR-004: Skalabilitas (Internal):** Struktur kode harus mendukung penambahan fitur di masa depan tanpa refactoring besar-besaran.
*   **NFR-005: Offline Mode:** Aplikasi harus berfungsi sepenuhnya bahkan tanpa koneksi internet, dengan data disimpan secara lokal.
*   **NFR-006: Kompatibilitas:** Aplikasi harus kompatibel dengan Android (versi terbaru dan 2 versi sebelumnya) dan iOS (versi terbaru dan 2 versi sebelumnya).
*   **NFR-007: Bahasa:** Aplikasi akan menggunakan Bahasa Indonesia sebagai bahasa utama.
*   **NFR-008: Arsitektur:** Aplikasi akan dibangun menggunakan Flutter, Riverpod Generator, Freezed, Go_Router, Fpdart, Logger, dan Drift (sebagai database lokal).

**6. Desain Antarmuka Pengguna (UI/UX) - Gambaran Umum**

*   **Skema Warna:** Bersih, profesional, dengan aksen yang menenangkan mata.
*   **Tata Letak:** Layout yang sederhana, fokus pada fungsionalitas inti.
*   **Navigasi:** Menggunakan BottomNavigationBar untuk navigasi utama (Dashboard, Bahan Baku, Input Masuk, Input Keluar).
*   **Input Data:** Menggunakan form yang jelas dengan validasi input.
*   **Visualisasi Stok:** Menggunakan indikator visual (warna) untuk status stok (misalnya: hijau = cukup, kuning = peringatan, merah = habis).

**7. Alur Pengguna (User Flow - Contoh Sederhana)**

*   **Alur Menambah Bahan Baru:**
    1.  Buka aplikasi.
    2.  Pilih tab "Bahan Baku".
    3.  Ketuk tombol "Tambah Bahan".
    4.  Input "Nama Bahan" dan "Satuan".
    5.  (Opsional) Pilih "Kategori".
    6.  (Opsional) Set "Stok Minimum".
    7.  Ketuk "Simpan".

*   **Alur Mencatat Bahan Masuk:**
    1.  Buka aplikasi.
    2.  Pilih tab "Bahan Masuk".
    3.  Ketuk tombol "Tambah Transaksi".
    4.  Pilih "Nama Bahan" dari daftar.
    5.  Input "Jumlah Masuk".
    6.  Ketuk "Simpan".

*   **Alur Mencatat Penggunaan Bahan:**
    1.  Buka aplikasi.
    2.  Pilih tab "Penggunaan".
    3.  Ketuk tombol "Tambah Transaksi".
    4.  Pilih "Nama Bahan" dari daftar.
    5.  Input "Jumlah Digunakan".
    6.  (Opsional) Input "Keterangan".
    7.  Ketuk "Simpan".

**8. Ruang Lingkup (In/Out of Scope) - MVP**

*   **In Scope:**
    *   Semua fitur yang dijelaskan di bagian 4 (Fungsional Requirements).
    *   Pengelolaan data secara lokal menggunakan Drift.
    *   Antarmuka pengguna yang sederhana dan intuitif.
    *   Offline functionality.

*   **Out of Scope (untuk MVP awal, bisa menjadi fase berikutnya):**
    *   Integrasi dengan sistem POS (Point of Sale).
    *   Multi-user dengan role/hak akses yang berbeda.
    *   Fitur resep produk (mengurangi stok otomatis berdasarkan penjualan produk jadi).
    *   Sinkronisasi cloud / multi-device.
    *   Fitur order otomatis ke supplier.
    *   Laporan keuangan detil (costing, profit).
    *   Manajemen shift staff.
    *   Fitur barcode scanning.
    *   Notifikasi push ke perangkat.

**9. Metrik Keberhasilan**

*   **Adopsi:** Minimal 80% staff yang relevan menggunakan aplikasi untuk pencatatan harian.
*   **Akurasi Stok:** Deviasi antara stok fisik dan stok aplikasi kurang dari 5%.
*   **Frekuensi Notifikasi:** Penurunan insiden "bahan habis tak terduga" sebesar 50%.
*   **Kepuasan Pengguna:** Tingkat kepuasan pengguna (berdasarkan survei/feedback) minimal 4 dari 5.

**10. Lampiran (Opsional - Bisa Ditambahkan Nanti)**

*   Wireframes dasar.
*   Diagram arsitektur sistem.
*   Daftar entitas database awal.

---
