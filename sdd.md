## Software Design Document (SDD) - StockFlow

**Versi:** 1.0
**Tanggal:** 26 Oktober 2023

---

### 1. Pendahuluan

#### 1.1 Tujuan
Dokumen ini menyediakan desain teknis tingkat tinggi dan rendah untuk aplikasi "StockFlow". Tujuannya adalah untuk memandu tim pengembang dalam mengimplementasikan fungsionalitas yang telah dijabarkan dalam Software Requirements Specification (SRS) v1.0, dengan memastikan konsistensi, skalabilitas, dan kemudahan pemeliharaan kode.

#### 1.2 Lingkup
Dokumen ini mencakup:
*   Arsitektur perangkat lunak yang dipilih.
*   Desain database menggunakan Drift.
*   Pola manajemen state dengan Riverpod Generator.
*   Struktur navigasi dengan GoRouter.
*   Desain model data dengan Freezed.
*   Pola penanganan *error* dengan fpdart.
*   Struktur direktori proyek.

#### 1.3 Referensi
*   Software Requirements Specification (SRS) - StockFlow v1.0
*   Entity-Relationship Diagram (ERD) - StockFlow v1.0

### 2. Tinjauan Sistem

Aplikasi StockFlow akan dibangun sebagai aplikasi mobile *cross-platform* menggunakan Flutter. Aplikasi ini akan mengikuti arsitektur berlapis (*layered architecture*) untuk memisahkan antara presentasi (UI), logika bisnis (aplikasi), dan akses data (data). Semua data akan disimpan secara lokal di perangkat menggunakan database SQLite yang dikelola oleh *library* Drift, memastikan fungsionalitas penuh saat *offline*.

### 3. Desain Arsitektur

Kami akan mengadopsi **Arsitektur Bersih (Clean Architecture)** yang disederhanakan, yang terdiri dari tiga lapisan utama:

1.  **Presentation Layer:** Bertanggung jawab atas semua hal yang berkaitan dengan UI.
    *   **Komponen:** Widgets (Screens, UI Components), Animation.
    *   **Teknologi:** Flutter.
    *   **Deskripsi:** Lapisan ini hanya akan berisi logika UI minimal. Untuk mendapatkan dan memanipulasi data, ia akan berinteraksi dengan *provider* dari lapisan Aplikasi (Riverpod).

2.  **Application/Domain Layer:** Berisi logika bisnis aplikasi.
    *   **Komponen:** Riverpod Providers (Notifiers, Providers), Services, Data Transfer Objects (DTOs), dan model data (dibuat dengan Freezed).
    *   **Teknologi:** Riverpod Generator, Freezed, fpdart.
    *   **Deskripsi:** Lapisan ini menjadi jembatan antara UI dan Data. *Provider* Riverpod akan memanggil metode dari *Repository* di lapisan Data untuk mengambil atau mengubah data, lalu mengelola *state* dan menyediakannya ke UI.

3.  **Data Layer:** Bertanggung jawab atas sumber dan pengelolaan data.
    *   **Komponen:** Repositories, Data Sources (Lokal), Data Models (Drift Tables).
    *   **Teknologi:** Drift.
    *   **Deskripsi:** Lapisan ini mengimplementasikan *Repository Pattern*. *Repository* akan menyediakan *interface* yang bersih untuk diakses oleh lapisan Aplikasi, menyembunyikan detail implementasi dari mana data berasal (dalam kasus ini, database Drift lokal).

#### Diagram Arsitektur
```
+---------------------------+
|   Presentation Layer      |
|  (Flutter Widgets, UI)    |
+-------------^-------------+
              | (Listens to State)
              v
+---------------------------+
|    Application Layer      |  <-- (Uses fpdart for error handling)
| (Riverpod Providers, Logic) |
+-------------^-------------+
              | (Calls Repository Methods)
              v
+---------------------------+
|       Data Layer          |
| (Repositories, Drift DB)  |
+---------------------------+
```

### 4. Desain Detail Komponen

#### 4.1 Desain Database (Drift)
Sesuai dengan ERD, kita akan membuat *file* `.drift` atau `.dart` untuk mendefinisikan tabel:

*   **Tabel `Users`:**
    ```dart
    class Users extends Table {
      IntColumn get id => integer().autoIncrement()();
      TextColumn get username => text().unique()();
      TextColumn get passwordHash => text()();
      TextColumn get role => text()();
      DateTimeColumn get createdAt => dateTime().withDefault(currentDateAndTime)();
    }
    ```

*   **Tabel `Ingredients`:**
    ```dart
    class Ingredients extends Table {
      IntColumn get id => integer().autoIncrement()();
      TextColumn get name => text().unique()();
      TextColumn get category => text()();
      TextColumn get unit => text()();
      RealColumn get minStockLevel => real().withDefault(const Constant(0.0))();
      RealColumn get currentStock => real().withDefault(const Constant(0.0))();
      DateTimeColumn get updatedAt => dateTime().withDefault(currentDateAndTime)();
    }
    ```

*   **Tabel `Transactions`:**
    ```dart
    class Transactions extends Table {
      IntColumn get id => integer().autoIncrement()();
      IntColumn get ingredientId => integer().references(Ingredients, #id)();
      TextColumn get transactionType => text()(); // 'in', 'out'
      RealColumn get quantity => real()();
      DateTimeColumn get transactionDate => dateTime().withDefault(currentDateAndTime)();
      TextColumn get notes => text().nullable()();
      IntColumn get userId => integer().references(Users, #id)();
    }
    ```Drift akan men-*generate* kelas `AppDatabase`, *Data Access Objects* (DAO), dan kelas model data secara otomatis.

#### 4.2 Manajemen State (Riverpod Generator)
Kita akan menggunakan Riverpod Generator untuk mengurangi *boilerplate*.

*   **Dependency Injection:** *Repository* dan *Services* akan disediakan menggunakan `Provider` sederhana.
    ```dart
    @riverpod
    IngredientRepository ingredientRepository(IngredientRepositoryRef ref) {
      final db = ref.watch(appDatabaseProvider);
      return IngredientRepositoryImpl(db.ingredientsDao);
    }
    ```

*   **State Management:** Untuk data yang bersifat asinkron (misalnya, mengambil daftar bahan), kita akan menggunakan `AsyncNotifierProvider`.
    ```dart
    @riverpod
    class IngredientList extends _$IngredientList {
      @override
      Future<List<Ingredient>> build() async {
        final repository = ref.watch(ingredientRepositoryProvider);
        return repository.getAllIngredients();
      }

      Future<void> addIngredient(Ingredient newIngredient) async {
        // ... logic to add ingredient and refresh state
      }
    }
    ```

#### 4.3 Navigasi (GoRouter)
Navigasi akan dikelola secara terpusat.
*   **Konfigurasi Router:** Sebuah file `app_router.dart` akan mendefinisikan semua rute.
    ```dart
    final GoRouter router = GoRouter(
      initialLocation: '/splash',
      routes: [
        GoRoute(path: '/splash', builder: (context, state) => SplashScreen()),
        GoRoute(path: '/login', builder: (context, state) => LoginScreen()),
        GoRoute(
          path: '/dashboard',
          builder: (context, state) => DashboardScreen(),
          routes: [
            GoRoute(
              path: 'ingredients', // -> /dashboard/ingredients
              builder: (context, state) => IngredientListScreen(),
            ),
            GoRoute(
              path: 'transaction/new', // -> /dashboard/transaction/new
              builder: (context, state) => NewTransactionScreen(),
            ),
          ]
        ),
      ],
      // ... redirect logic for authentication
    );
    ```

#### 4.4 Model Data (Freezed)
Selain model dari Drift untuk database, kita akan menggunakan Freezed untuk model data di lapisan Aplikasi/Domain jika diperlukan (misalnya, untuk *state* UI yang kompleks atau DTO). Ini memastikan *immutability*.

#### 4.5 Penanganan Error (fpdart)
Untuk menghindari `try-catch` yang berlebihan di lapisan UI dan Aplikasi, metode di *Repository* akan mengembalikan `Either<Failure, SuccessType>`.

*   **Contoh di Repository Interface:**
    ```dart
    abstract class IngredientRepository {
      Future<Either<Failure, List<Ingredient>>> getAllIngredients();
      Future<Either<Failure, void>> addIngredient(Ingredient ingredient);
    }
    ```

*   **Penggunaan di Provider:**
    ```dart
    final result = await repository.addIngredient(newIngredient);
    result.fold(
      (failure) => // handle error state
      (success) => // handle success state
    );
    ```
`Failure` akan menjadi kelas *sealed* untuk menangani berbagai jenis *error* (misal: `DatabaseFailure`, `ValidationFailure`).

### 5. Struktur Direktori Proyek

Struktur direktori akan diorganisir berdasarkan fitur (*feature-first*) untuk skalabilitas.

```
lib/
|-- core/                     # Kode yang di-share (utils, constants, etc.)
|   |-- config/               # Router, Theme
|   |-- constants/            # Konstanta aplikasi
|   |-- error/                # Failure & Exception classes
|   `-- utils/                # Helper functions
|
|-- data/                     # Lapisan Data (umum)
|   |-- local/                # Konfigurasi Drift Database, DAOs
|   `-- models/               # Model data Drift
|
|-- domain/                   # Lapisan Domain (umum)
|   `-- repositories/         # Abstract repository interfaces
|
|-- features/                 # Fitur-fitur aplikasi
|   |-- auth/                 # Fitur Autentikasi
|   |   |-- application/      # Riverpod providers for auth
|   |   |-- data/             # Implementasi auth repository
|   |   `-- presentation/     # Layar Login, widgets
|   |
|   |-- ingredients/          # Fitur Manajemen Bahan
|   |   |-- application/
|   |   |-- data/
|   |   `-- presentation/
|   |
|   |-- dashboard/            # Fitur Dashboard
|   |   `-- presentation/
|   |
|   `-- transactions/         # Fitur Transaksi
|       |-- application/
|       |-- data/
|       `-- presentation/
|
`-- main.dart                 # Entry point aplikasi
```

### 6. Desain Antarmuka Pengguna (UI)
*   **Pustaka Komponen:** Menggunakan Material 3 di Flutter.
*   **Tema:** Aplikasi akan menggunakan tema sentral (`ThemeData`) yang didefinisikan di `core/config/theme.dart` untuk konsistensi warna, tipografi, dan gaya komponen.
*   **Desain Responsif:** Meskipun target utama adalah *handphone*, tata letak akan dirancang agar tidak rusak di layar yang lebih besar (misalnya, tablet).

---

Dokumen ini menyediakan kerangka kerja teknis yang solid untuk memulai pengembangan aplikasi StockFlow. Desain ini memprioritaskan pemisahan tugas (*separation of concerns*), keterujian (*testability*), dan skalabilitas.
