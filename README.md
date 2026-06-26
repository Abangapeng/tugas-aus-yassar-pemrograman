# Toko Berkah Jaya — Sistem Manajemen Penjualan

Project Java Swing + MySQL, dibuat sesuai isi Laporan Dokumentasi dan Testing Program
(Bab II & III). Project ini berformat **Maven**, sehingga bisa langsung dibuka oleh
NetBeans (File → Open Project, NetBeans otomatis mendeteksi pom.xml).

## 1. Struktur Project
```
TokoBerkahJaya/
├── pom.xml                          <- konfigurasi Maven (NetBeans baca otomatis)
├── database/
│   └── db_berkahjaya.sql            <- script SQL: buat database + data awal
└── src/main/java/berkahjaya/
    ├── Main.java                    <- entry point aplikasi
    ├── model/                       <- User, Barang, Kategori, Customer, Penjualan
    ├── dao/                         <- UserDAO, BarangDAO, CustomerDAO, KategoriDAO, PenjualanDAO
    ├── util/                        <- DBConnection (JDBC), UIHelper (format rupiah & warna)
    └── ui/                          <- LoginForm, MainFrame, DashboardPanel,
                                         BarangPanel, CustomerPanel, TransaksiPanel, LaporanPanel
```

## 2. Persiapan Database (WAJIB sebelum menjalankan aplikasi)

1. Jalankan MySQL Server (bisa lewat XAMPP / Laragon / MySQL Workbench / dll).
2. Import file `database/db_berkahjaya.sql`, misalnya lewat phpMyAdmin
   (klik tab *Import*, pilih file ini) atau lewat terminal:
   ```
   mysql -u root -p < database/db_berkahjaya.sql
   ```
   Script ini otomatis membuat database `db_berkahjaya`, seluruh tabel
   (`tb_user`, `tb_kategori`, `tb_barang`, `tb_customer`, `tb_penjualan`),
   serta mengisi data awal (akun login, kategori, barang, customer, contoh transaksi).

3. Jika username/password MySQL Anda BUKAN `root` tanpa password, sesuaikan di:
   `src/main/java/berkahjaya/util/DBConnection.java`
   ```java
   private static final String USER = "root";
   private static final String PASSWORD = "";
   ```

## 3. Membuka & Menjalankan Project di NetBeans

1. Buka **NetBeans** → menu **File → Open Project...**
2. Arahkan ke folder `TokoBerkahJaya` (folder yang berisi `pom.xml`), lalu klik **Open Project**.
   NetBeans akan otomatis mengenalinya sebagai project Maven dan mengunduh
   dependency `mysql-connector-j` (memerlukan koneksi internet saat pertama kali dibuka).
3. Klik kanan project di panel Projects → **Run** (atau tekan F6).
4. Class yang dijalankan otomatis adalah `berkahjaya.Main` (sudah diatur lewat `pom.xml`).

### Login Demo
| Role  | Username | Password  |
|-------|----------|-----------|
| Admin | admin    | admin123  |
| Kasir | kasir    | kasir123  |

## 4. Menjalankan Tanpa NetBeans (opsional, via terminal)

```
mvn clean package
java -jar target/TokoBerkahJaya.jar
```
(`maven-shade-plugin` di pom.xml sudah membungkus mysql-connector-j ke dalam JAR,
sehingga JAR hasil build bisa langsung dijalankan di komputer mana pun yang punya MySQL).

## 5. Fitur yang Tersedia (sesuai laporan)

- **Login** — autentikasi admin & kasir, validasi username/password salah.
- **Dashboard** — statistik jumlah produk, customer, transaksi, stok menipis,
  total pendapatan, penjualan hari ini, dan tabel transaksi terakhir.
- **Data Barang** — tambah, ubah, hapus, cari barang (lengkap dengan kategori, satuan, harga, stok).
- **Data Customer** — tambah, ubah, hapus, cari data pelanggan.
- **Transaksi Penjualan** — pilih customer & barang, harga dan stok muncul otomatis,
  total bayar dihitung otomatis, validasi stok sebelum transaksi disimpan,
  stok otomatis berkurang setelah transaksi berhasil (memakai SQL transaction agar konsisten).
- **Laporan Penjualan** — total transaksi, total pendapatan, dan tabel riwayat lengkap.

## 6. Catatan Pengembangan Lanjutan (sesuai saran di Bab IV Laporan)

- Password saat ini disimpan plain text — untuk produksi sebaiknya di-hash (misal BCrypt).
- Bisa ditambahkan fitur backup/restore database, export CSV, grafik penjualan,
  serta integrasi printer struk sesuai saran pengembangan pada laporan.
