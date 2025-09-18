Baik, selanjutnya kita akan menyusun dokumen Software Requirements Specification (SRS) berdasarkan PRD dan ERD yang sudah kita buat. SRS ini akan lebih detail dalam mendefinisikan persyaratan fungsional dan non-fungsional, serta batasan sistem.

---

**Software Requirements Specification (SRS)**

**Nama Produk:** CoffeStock - Aplikasi Pengelola Stok Bahan Coffeeshop

**Versi:** 1.0
**Tanggal:** 23 Mei 2024
**Penulis:** [Nama Anda / Tim]

---

**1. Pendahuluan**

Dokumen Software Requirements Specification (SRS) ini merinci persyaratan sistem untuk aplikasi CoffeStock. Aplikasi ini adalah solusi mobile untuk membantu coffeeshop atau restoran kecil dalam manajemen stok bahan baku. Tujuan utamanya adalah memastikan ketersediaan bahan, mengurangi pemborosan, dan menyederhanakan proses pencatatan stok. Dokumen ini akan menjadi referensi utama bagi tim pengembang sepanjang siklus pengembangan produk.

**1.1. Tujuan**

*   Memberikan deskripsi lengkap dan terperinci tentang persyaratan fungsional dan non-fungsional aplikasi CoffeStock.
*   Menjadi dasar untuk desain, implementasi, pengujian, dan pemeliharaan aplikasi.
*   Memastikan pemahaman yang konsisten antara stakeholder dan tim pengembang.

**1.2. Lingkup Produk**

CoffeStock akan menjadi aplikasi mobile (Android & iOS) yang berfokus pada:
*   Pencatatan bahan baku, kategori, dan stok minimum.
*   Pencatatan transaksi bahan masuk (pembelian) dari supplier.
*   Pencatatan transaksi penggunaan bahan (oleh kitchen/bar).
*   Pemantauan stok saat ini dan notifikasi untuk stok di bawah minimum.
*   Penyimpanan data secara lokal (offline-first).

**1.3. Definisi, Akronim, dan Singkatan**

*   **API:** Application Programming Interface
*   **ERD:** Entity-Relationship Diagram
*   **Flutter:** Framework UI untuk membangun aplikasi multi-platform.
*   **MVP:** Minimum Viable Product
*   **PRD:** Product Requirements Document
*   **SRS:** Software Requirements Specification
*   **UI:** User Interface
*   **UX:** User Experience
*   **Drift:** Database lokal untuk Flutter (berbasis SQLite).
*   **Riverpod:** State management untuk Flutter.
*   **Freezed:** Code generator untuk data class di Dart/Flutter.
*   **Go_Router:** Library untuk routing di Flutter.
*   **Fpdart:** Functional programming utilities untuk Dart.
*   **Logger:** Library untuk logging di Dart/Flutter.

**2. Deskripsi Umum**

**2.1. Perspektif Produk**

CoffeStock adalah aplikasi mandiri yang dirancang untuk beroperasi secara offline. Pada MVP, tidak ada integrasi dengan sistem eksternal (seperti POS atau API supplier). Aplikasi ini akan menjadi alat inti untuk manajemen stok di lokasi coffeeshop.

**2.2. Fungsi Produk**

*   **Manajemen Bahan Baku:** Menambah, mengedit, menghapus bahan, kategori, dan mengatur stok minimum.
*   **Manajemen Transaksi Masuk:** Mencatat bahan yang diterima dari supplier, memperbarui stok.
*   **Manajemen Transaksi Keluar:** Mencatat bahan yang digunakan oleh operasional, memperbarui stok.
*   **Dashboard & Pelaporan:** Menampilkan ringkasan stok, notifikasi, dan riwayat transaksi.
*   **Manajemen Supplier:** Mencatat daftar supplier dasar.

**2.3. Karakteristik Pengguna**

*   **Admin Coffeeshop:**
    *   Familiar dengan operasional coffeeshop/restoran.
    *   Dapat melakukan semua fungsi di aplikasi.
    *   Tingkat keahlian teknis: Dasar hingga Menengah.
*   **Staff Kitchen/Bar:**
    *   Familiar dengan proses penggunaan bahan.
    *   Utamanya akan menggunakan fitur pencatatan penggunaan bahan.
    *   Tingkat keahlian teknis: Dasar.

**2.4. Batasan**

*   **Platform:** Aplikasi mobile (Android & iOS).
*   **Offline-First:** Dirancang untuk berfungsi sepenuhnya tanpa koneksi internet.
*   **Tidak ada Integrasi Eksternal:** MVP tidak akan terintegrasi dengan POS, API eksternal, atau sistem ERP lainnya.
*   **Database Lokal:** Menggunakan Drift (SQLite) untuk penyimpanan data.
*   **Satu Lokasi:** Aplikasi dirancang untuk satu lokasi coffeeshop/restoran. Fitur multi-cabang tidak dalam cakupan MVP.
*   **Satu User (MVP):** Untuk MVP, diasumsikan satu user (admin) yang mengelola data. Fitur otentikasi multi-user dengan role berbeda akan menjadi pengembangan di masa depan.

**3. Persyaratan Fungsional**

Ini adalah daftar rinci dari fungsi-fungsi yang harus dilakukan oleh sistem.

**3.1. Manajemen Bahan Baku**
    *   **FR-001 (Lihat Daftar Bahan Baku):** Sistem harus menampilkan daftar semua bahan baku, termasuk nama, satuan, stok saat ini, dan status (normal, warning, habis).
        *   **Input:** Tidak ada, atau filter/pencarian.
        *   **Output:** Daftar bahan baku.
        *   **Kriteria Sukses:** Daftar ditampilkan dengan benar dan dapat diurutkan/difilter.
    *   **FR-002 (Tambah Bahan Baku):** Pengguna harus dapat menambahkan bahan baku baru.
        *   **Input:** Nama bahan (mandatory), Satuan (mandatory, dropdown/input teks), Kategori (opsional), Stok Minimum (opsional, default 0).
        *   **Output:** Bahan baku baru ditambahkan ke database, daftar bahan diperbarui.
        *   **Kriteria Sukses:** Bahan baku berhasil ditambahkan dengan validasi input.
    *   **FR-003 (Edit Bahan Baku):** Pengguna harus dapat mengubah detail bahan baku yang sudah ada.
        *   **Input:** Pilih bahan baku, edit Nama, Satuan, Kategori, Stok Minimum.
        *   **Output:** Bahan baku diperbarui di database, daftar diperbarui.
        *   **Kriteria Sukses:** Perubahan disimpan dengan benar.
    *   **FR-004 (Hapus Bahan Baku):** Pengguna harus dapat menghapus bahan baku setelah konfirmasi.
        *   **Input:** Pilih bahan baku, konfirmasi penghapusan.
        *   **Output:** Bahan baku dihapus dari database, semua transaksi terkait (masuk/keluar) dihapus (CASCADE).
        *   **Kriteria Sukses:** Bahan baku dan transaksi terkait terhapus.
    *   **FR-005 (Manajemen Kategori):** Pengguna dapat menambah, mengedit, dan menghapus kategori bahan.
        *   **Input:** Nama kategori.
        *   **Output:** Kategori baru/teredit/terhapus.
        *   **Kriteria Sukses:** Kategori dikelola dengan benar, bahan baku yang terkait dengan kategori yang dihapus akan memiliki `idKategori` diatur `NULL`.
    *   **FR-006 (Pencarian/Filter Bahan Baku):** Pengguna harus dapat mencari bahan baku berdasarkan nama atau memfilter berdasarkan kategori.
        *   **Input:** Teks pencarian, pilihan kategori.
        *   **Output:** Daftar bahan baku yang difilter/dicari.
        *   **Kriteria Sukses:** Hasil pencarian/filter akurat dan cepat.

**3.2. Pencatatan Transaksi Bahan Masuk**
    *   **FR-007 (Tambah Transaksi Masuk):** Pengguna harus dapat mencatat bahan baku yang diterima.
        *   **Input:** Pilih bahan baku (mandatory), Jumlah (mandatory, angka positif), Tanggal Transaksi (default hari ini), Supplier (opsional, pilih dari daftar/tambah baru).
        *   **Output:** Transaksi tercatat, `stokSaatIni` di `BahanBaku` diperbarui.
        *   **Kriteria Sukses:** Stok bahan baku bertambah sesuai input, transaksi tercatat.
    *   **FR-008 (Lihat Riwayat Transaksi Masuk):** Pengguna harus dapat melihat riwayat semua transaksi bahan masuk.
        *   **Input:** Tidak ada, atau filter berdasarkan tanggal/bahan baku/supplier.
        *   **Output:** Daftar transaksi masuk.
        *   **Kriteria Sukses:** Riwayat ditampilkan dengan benar.

**3.3. Pencatatan Transaksi Bahan Keluar (Penggunaan)**
    *   **FR-009 (Tambah Transaksi Keluar):** Pengguna harus dapat mencatat penggunaan bahan baku.
        *   **Input:** Pilih bahan baku (mandatory), Jumlah (mandatory, angka positif), Tanggal Transaksi (default hari ini), Keterangan (opsional).
        *   **Output:** Transaksi tercatat, `stokSaatIni` di `BahanBaku` diperbarui.
        *   **Kriteria Sukses:** Stok bahan baku berkurang sesuai input, transaksi tercatat.
        *   **Validasi:** Sistem harus mencegah `stokSaatIni` menjadi negatif. Jika jumlah penggunaan melebihi stok yang ada, sistem harus memberikan peringatan dan tidak mengizinkan transaksi tersebut.
    *   **FR-010 (Lihat Riwayat Transaksi Keluar):** Pengguna harus dapat melihat riwayat semua transaksi penggunaan bahan.
        *   **Input:** Tidak ada, atau filter berdasarkan tanggal/bahan baku.
        *   **Output:** Daftar transaksi keluar.
        *   **Kriteria Sukses:** Riwayat ditampilkan dengan benar.

**3.4. Dashboard & Notifikasi**
    *   **FR-011 (Tampilan Dashboard):** Dashboard harus menampilkan ringkasan penting:
        *   Daftar bahan baku yang stoknya di bawah `stokMinimum` (highlighted).
        *   Jumlah total item bahan baku yang sedang dalam status warning/habis.
        *   Link cepat ke input transaksi masuk dan keluar.
        *   **Output:** Ringkasan data real-time.
        *   **Kriteria Sukses:** Informasi relevan ditampilkan secara jelas.
    *   **FR-012 (Notifikasi Stok Minimum):** Sistem harus secara aktif memberi tahu pengguna jika ada bahan baku yang stoknya di bawah `stokMinimum` melalui antarmuka pengguna (misalnya, ikon peringatan di samping item bahan, atau banner di dashboard).
        *   **Input:** Perubahan stok.
        *   **Output:** Indikator visual/teks peringatan.
        *   **Kriteria Sukses:** Notifikasi muncul secara otomatis saat kondisi terpenuhi.

**3.5. Manajemen Supplier**
    *   **FR-013 (Tambah/Lihat Supplier):** Pengguna dapat menambah nama supplier baru atau memilih dari daftar supplier yang sudah ada saat mencatat `TransaksiMasuk`.
        *   **Input:** Nama supplier (mandatory).
        *   **Output:** Supplier baru ditambahkan, daftar supplier diperbarui.
        *   **Kriteria Sukses:** Supplier dapat dikelola.

**4. Persyaratan Non-Fungsional**

**4.1. Kinerja**
    *   **NFR-001 (Waktu Respon UI):** Waktu muat untuk semua layar utama (Dashboard, Daftar Bahan, Input Transaksi) tidak boleh melebihi 2 detik pada perangkat Android/iOS kelas menengah.
    *   **NFR-002 (Waktu Pemrosesan Transaksi):** Penambahan/pembaruan transaksi stok harus selesai dalam waktu kurang dari 500ms.
    *   **NFR-003 (Efisiensi Sumber Daya):** Aplikasi harus memiliki footprint memori yang rendah dan tidak menguras baterai secara signifikan.

**4.2. Keamanan**
    *   **NFR-004 (Integritas Data):** Data yang disimpan di database lokal harus konsisten dan tidak boleh rusak. Mekanisme transaksi database harus digunakan untuk operasi kritis.
    *   **NFR-005 (Privasi Data):** Data sensitif (jika ada di masa depan, seperti data personal) harus dilindungi. Untuk MVP, data stok dianggap non-sensitif.
    *   **NFR-006 (Akses Lokal):** Data hanya dapat diakses melalui aplikasi itu sendiri pada perangkat lokal.

**4.3. Usability**
    *   **NFR-007 (Intuitif):** Pengguna baru harus dapat memahami alur kerja dasar aplikasi dalam waktu 15 menit tanpa pelatihan ekstensif.
    *   **NFR-008 (Minimal Input):** Tugas-tugas umum (misalnya, mencatat penggunaan) harus memerlukan jumlah input minimum.
    *   **NFR-009 (Feedback Visual):** Aplikasi harus memberikan feedback visual yang jelas untuk setiap aksi pengguna (misalnya, indikator loading, pesan sukses/error).
    *   **NFR-010 (Desain Konsisten):** Desain UI harus konsisten di seluruh aplikasi untuk mengurangi beban kognitif pengguna.

**4.4. Reliabilitas**
    *   **NFR-011 (Ketersediaan):** Aplikasi harus tersedia dan berfungsi penuh selama jam operasional coffeeshop.
    *   **NFR-012 (Penanganan Error):** Aplikasi harus dapat menangani error dengan gracefully dan memberikan pesan yang informatif kepada pengguna, bukan crash.
    *   **NFR-013 (Toleransi Kesalahan):** Sistem harus memiliki mekanisme validasi input untuk mencegah data yang tidak valid masuk ke database.

**4.5. Skalabilitas & Pemeliharaan**
    *   **NFR-014 (Arsitektur Bersih):** Aplikasi harus dibangun dengan arsitektur yang modular dan bersih (Clean Architecture / Layered Architecture) untuk memudahkan penambahan fitur di masa depan.
    *   **NFR-015 (Keterbacaan Kode):** Kode harus ditulis dengan standar dan kon