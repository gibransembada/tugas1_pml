Oke, ide bagus! Kita akan membuat skema database (Entity-Relationship Diagram - ERD) terlebih dahulu. Ini akan menjadi fondasi yang kuat sebelum kita menyentuh kode.

Skema database ini akan dirancang untuk **Drift** (sebelumnya Moor), yang merupakan database lokal berbasis SQLite. Kita akan fokus pada entitas utama dan relasinya.

---

**Skema Database (ERD) - CoffeStock**

**Entitas (Tabel):**

1.  **`BahanBaku` (Ingredients)**
    *   Tabel ini menyimpan daftar semua bahan baku yang digunakan di coffeeshop.
    *   **Kolom:**
        *   `id` (INTEGER PRIMARY KEY AUTOINCREMENT) - ID unik untuk setiap bahan baku.
        *   `nama` (TEXT NOT NULL) - Nama bahan baku (contoh: "Biji Kopi Arabika", "Susu Full Cream").
        *   `satuan` (TEXT NOT NULL) - Satuan dasar untuk bahan ini (contoh: "gram", "ml", "pcs", "kg", "liter").
        *   `stokSaatIni` (REAL NOT NULL DEFAULT 0.0) - Jumlah stok bahan baku saat ini. Menggunakan `REAL` untuk mendukung nilai desimal.
        *   `stokMinimum` (REAL NOT NULL DEFAULT 0.0) - Batas stok minimum.
        *   `idKategori` (INTEGER NULL) - Foreign Key ke tabel `KategoriBahan`. Bisa NULL jika tidak ada kategori.
        *   `createdAt` (INTEGER NOT NULL) - Timestamp pembuatan record (UNIX timestamp).
        *   `updatedAt` (INTEGER NOT NULL) - Timestamp terakhir kali record diperbarui (UNIX timestamp).

2.  **`KategoriBahan` (IngredientCategories)**
    *   Tabel ini untuk mengelompokkan bahan baku.
    *   **Kolom:**
        *   `id` (INTEGER PRIMARY KEY AUTOINCREMENT) - ID unik untuk setiap kategori.
        *   `nama` (TEXT NOT NULL UNIQUE) - Nama kategori (contoh: "Kopi", "Susu", "Sirup", "Roti").
        *   `createdAt` (INTEGER NOT NULL) - Timestamp pembuatan record.
        *   `updatedAt` (INTEGER NOT NULL) - Timestamp terakhir kali record diperbarui.

3.  **`TransaksiMasuk` (IncomingTransactions)**
    *   Tabel ini mencatat setiap kali bahan baku masuk (dari supplier).
    *   **Kolom:**
        *   `id` (INTEGER PRIMARY KEY AUTOINCREMENT) - ID unik untuk setiap transaksi.
        *   `idBahanBaku` (INTEGER NOT NULL) - Foreign Key ke tabel `BahanBaku`.
        *   `jumlah` (REAL NOT NULL) - Jumlah bahan yang masuk.
        *   `satuan` (TEXT NOT NULL) - Satuan yang digunakan untuk transaksi ini. (Ini penting jika suatu bahan bisa dibeli dengan satuan berbeda dari satuan dasar, meskipun untuk MVP mungkin akan selalu sama dengan `BahanBaku.satuan`).
        *   `tanggalTransaksi` (INTEGER NOT NULL) - Tanggal dan waktu transaksi (UNIX timestamp).
        *   `idSupplier` (INTEGER NULL) - Foreign Key ke tabel `Supplier`. Bisa NULL jika supplier tidak dicatat.
        *   `createdAt` (INTEGER NOT NULL) - Timestamp pembuatan record.

4.  **`TransaksiKeluar` (OutgoingTransactions)**
    *   Tabel ini mencatat setiap kali bahan baku digunakan (oleh kitchen/bar).
    *   **Kolom:**
        *   `id` (INTEGER PRIMARY KEY AUTOINCREMENT) - ID unik untuk setiap transaksi.
        *   `idBahanBaku` (INTEGER NOT NULL) - Foreign Key ke tabel `BahanBaku`.
        *   `jumlah` (REAL NOT NULL) - Jumlah bahan yang digunakan.
        *   `satuan` (TEXT NOT NULL) - Satuan yang digunakan untuk transaksi ini.
        *   `tanggalTransaksi` (INTEGER NOT NULL) - Tanggal dan waktu transaksi (UNIX timestamp).
        *   `keterangan` (TEXT NULL) - Deskripsi opsional tentang penggunaan (misal: "Untuk 5x Latte", "Membuat 2 Roti").
        *   `createdAt` (INTEGER NOT NULL) - Timestamp pembuatan record.

5.  **`Supplier` (Suppliers)**
    *   Tabel ini menyimpan informasi dasar supplier.
    *   **Kolom:**
        *   `id` (INTEGER PRIMARY KEY AUTOINCREMENT) - ID unik untuk setiap supplier.
        *   `nama` (TEXT NOT NULL UNIQUE) - Nama supplier.
        *   `createdAt` (INTEGER NOT NULL) - Timestamp pembuatan record.
        *   `updatedAt` (INTEGER NOT NULL) - Timestamp terakhir kali record diperbarui.

**Relasi:**

*   **`BahanBaku` 1-to-Many `TransaksiMasuk`**: Satu bahan baku dapat memiliki banyak transaksi masuk.
    *   `TransaksiMasuk.idBahanBaku` REFERENCES `BahanBaku.id` ON DELETE CASCADE
*   **`BahanBaku` 1-to-Many `TransaksiKeluar`**: Satu bahan baku dapat memiliki banyak transaksi keluar.
    *   `TransaksiKeluar.idBahanBaku` REFERENCES `BahanBaku.id` ON DELETE CASCADE
*   **`KategoriBahan` 1-to-Many `BahanBaku`**: Satu kategori dapat memiliki banyak bahan baku.
    *   `BahanBaku.idKategori` REFERENCES `KategoriBahan.id` ON DELETE SET NULL
*   **`Supplier` 1-to-Many `TransaksiMasuk`**: Satu supplier dapat memiliki banyak transaksi masuk.
    *   `TransaksiMasuk.idSupplier` REFERENCES `Supplier.id` ON DELETE SET NULL

**Diagram Sederhana:**

```
+------------------+       +-------------------+       +--------------------+
| BahanBaku        |       | KategoriBahan     |       | Supplier           |
+------------------+       +-------------------+       +--------------------+
| id (PK)          |<------| id (PK)           |       | id (PK)            |
| nama             |       | nama (UNIQUE)     |       | nama (UNIQUE)      |
| satuan           |       | createdAt         |       | createdAt          |
| stokSaatIni      |       | updatedAt         |       | updatedAt          |
| stokMinimum      |       +-------------------+       +--------------------+
| idKategori (FK)  |
| createdAt        |
| updatedAt        |
+--------^---------+
         |
         | 1
         |
         | M
+--------v---------+       +--------------------+
| TransaksiMasuk   |       | TransaksiKeluar    |
+------------------+       +--------------------+
| id (PK)          |<------| id (PK)            |
| idBahanBaku (FK) |       | idBahanBaku (FK)   |
| jumlah           |       | jumlah             |
| satuan           |       | satuan             |
| tanggalTransaksi |       | tanggalTransaksi   |
| idSupplier (FK)  |       | keterangan         |
| createdAt        |       | createdAt          |
+------------------+       +--------------------+
    |
    | M
    |
    | 1
    +-------------------> Supplier (via idSupplier)
```
Berikut adalah representasi visual dari ERD yang telah kita diskusikan:

