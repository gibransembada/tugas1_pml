### Timeline & Checklist Pekerjaan: StockFlow MVP

**Tujuan MVP:** Merilis versi fungsional pertama aplikasi yang memungkinkan pengguna untuk mengelola master bahan, mencatat stok masuk, mencatat stok keluar, dan melihat status stok kritis di dashboard.

---

### **Sprint 0: Persiapan & Fondasi (Durasi: ~2-3 Hari)**

Sprint ini dilakukan sebelum pengembangan fitur utama dimulai. Tujuannya adalah menyiapkan semua alat dan struktur agar tim bisa langsung produktif di Sprint 1.

| Checklist                                               | Estimasi | Status      |
| ------------------------------------------------------- | :------: | ----------- |
| **[SETUP-01]** Inisialisasi proyek Flutter baru         |  0.5 Hari  | `[ ] To Do` |
| **[SETUP-02]** Konfigurasi Version Control (Git & Repo)   |  0.5 Hari  | `[ ] To Do` |
| **[SETUP-03]** Tambahkan semua *dependencies* (Drift, Riverpod, dll.) |  0.5 Hari  | `[ ] To Do` |
| **[SETUP-04]** Buat struktur direktori proyek sesuai SDD |  0.5 Hari  | `[ ] To Do` |
| **[SETUP-05]** Setup dasar GoRouter & Rute Navigasi      |  0.5 Hari  | `[ ] To Do` |
| **[SETUP-06]** Buat tema dasar aplikasi (warna, font)    |  0.5 Hari  | `[ ] To Do` |

---

### **Sprint 1: Core Data & Manajemen Bahan (Durasi: 2 Minggu)**

**Goal:** *Di akhir sprint, aplikasi memiliki fondasi data yang kuat dan pengguna (admin) sudah bisa mengelola daftar bahan baku secara penuh (CRUD).*

| Checklist                                                    | Estimasi | Status      |
| ------------------------------------------------------------ | :------: | ----------- |
| **[DB-01]** Implementasi skema database Drift (Tabel & DAO)    |  1.5 Hari  | `[ ] To Do` |
| **[REPO-01]** Buat *Interface* & *Implementation* Repository Bahan |  1 Hari    | `[ ] To Do` |
| **[PROV-01]** Setup Riverpod Provider untuk Database & Repository |  0.5 Hari  | `[ ] To Do` |
| **[FEAT-INGR-01]** Buat UI: Layar Daftar Bahan Baku (`IngredientListScreen`) |  1.5 Hari  | `[ ] To Do` |
| **[FEAT-INGR-02]** Buat UI: Form Tambah/Edit Bahan (`AddEditIngredientScreen`) |  2 Hari    | `[ ] To Do` |
| **[PROV-02]** Buat `AsyncNotifier` untuk mengelola *state* daftar bahan |  1 Hari    | `[ ] To Do` |
| **[INTEG-01]** Hubungkan UI Manajemen Bahan ke Provider & Repository |  1.5 Hari  | `[ ] To Do` |
| **[TEST-01]** Unit Testing untuk logika di Repository Bahan   |  0.5 Hari  | `[ ] To Do` |
| **[SPRINT-REVIEW-1]** Buffer, Testing Manual, & Review Sprint  |  0.5 Hari  | `[ ] To Do` |

---

### **Sprint 2: Transaksi Stok & Dashboard (Durasi: 2 Minggu)**

**Goal:** *Di akhir sprint, fungsionalitas inti aplikasi selesai. Pengguna bisa mencatat stok masuk & keluar, dan melihat ringkasan stok di dashboard. Aplikasi siap untuk rilis internal/uji coba.*

| Checklist                                                              | Estimasi | Status      |
| ---------------------------------------------------------------------- | :------: | ----------- |
| **[REPO-02]** Buat *Interface* & *Implementation* Repository Transaksi |  1 Hari    | `[ ] To Do` |
| **[FEAT-TRX-01]** Buat UI: Form Catat Stok Masuk (`StockInScreen`)          |  1.5 Hari  | `[ ] To Do` |
| **[FEAT-TRX-02]** Buat UI: Form Catat Stok Keluar (`StockOutScreen`)       |  1.5 Hari  | `[ ] To Do` |
| **[PROV-03]** Buat Provider/Notifier untuk logika transaksi (termasuk update `current_stock`) |  2 Hari    | `[ ] To Do` |
| **[INTEG-02]** Hubungkan UI Transaksi ke Provider & Repository         |  1 Hari    | `[ ] To Do` |
| **[FEAT-DASH-01]** Buat UI: Halaman Dashboard                            |  1.5 Hari  | `[ ] To Do` |
| **[FEAT-DASH-02]** Implementasi logika untuk menampilkan "Bahan Kritis" di dashboard |  1 Hari    | `[ ] To Do` |
| **[UI/UX-01]** Tambahkan *feedback* pengguna (Loading indicator, Snackbar, Dialog) |  0.5 Hari  | `[ ] To Do` |
| **[SPRINT-REVIEW-2]** Testing E2E, Bug Fixing, & Review Final MVP      |  1 Hari    | `[ ] To Do` |

---

### **Post-MVP: Persiapan Rilis (Durasi: ~1 Minggu)**

Setelah MVP fungsional, langkah selanjutnya adalah mempersiapkan untuk rilis.

| Checklist                                            | Estimasi | Status      |
| ---------------------------------------------------- | :------: | ----------- |
| **[REL-01]** Finalisasi App Icon & Splash Screen     |   1 Hari   | `[ ] To Do` |
| **[REL-02]** Lakukan Build Rilis (Android APK/AAB & iOS Archive) |   1 Hari   | `[ ] To Do` |
| **[REL-03]** Testing pada perangkat fisik             |   2 Hari   | `[ ] To Do` |
| **[REL-04]** Persiapan materi untuk App Store / Play Store |   1 Hari   | `[ ] To Do` |

**Catatan:**
*   **Estimasi Hari:** Ini adalah "hari kerja developer" dan sangat bergantung pada tingkat keahlian tim. Angka ini bisa disesuaikan.
*   **Buffer:** Waktu buffer sudah dimasukkan di akhir setiap sprint untuk menangani masalah tak terduga.
*   **Testing:** Meskipun ada alokasi waktu khusus, testing sebaiknya dilakukan secara berkelanjutan selama pengembangan.

Dengan *checklist* ini, proses pengembangan menjadi lebih terstruktur dan terukur. Kita bisa melacak kemajuan setiap hari dan memastikan semua kebutuhan MVP terpenuhi.
