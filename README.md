### 1. **Entity Relationship Diagram (ERD)**

Entitas utama:  
- **User** (id_user, nama, email, password, role, alamat, telepon)  
- **Produk** (id_produk, nama_produk, deskripsi, harga, stok, id_penjual)  
- **Keranjang** (id_keranjang, id_user, id_produk, kuantitas)  
- **Pesanan** (id_pesanan, id_user, status_pesanan, tanggal_pesanan)  
- **DetailPesanan** (id_detail, id_pesanan, id_produk, kuantitas, subtotal)  
- **Pembayaran** (id_pembayaran, id_pesanan, metode_pembayaran, status_pembayaran, tanggal_pembayaran)  

Relasi:  
- Satu **User** dapat menjadi pembeli atau penjual (Role).  
- Satu **User (Penjual)** memiliki banyak **Produk**.  
- Satu **User (Pembeli)** memiliki banyak **Keranjang**.  
- Satu **Pesanan** memiliki banyak **DetailPesanan**.  
- Satu **Pesanan** memiliki satu **Pembayaran**.  

---

### 2. **Context Diagram**

**Sistem E-Commerce**  
- **Input:** Data produk dari penjual, data pembeli, pesanan, dan pembayaran.  
- **Output:** Informasi produk, status pesanan, riwayat transaksi, laporan penjualan.  
- **Actors:** Penjual, Pembeli, Sistem Pembayaran.  

---

### 3. **Data Flow Diagram (DFD)**

#### Level 0:
- **Proses Utama:**  
  1. Kelola Akun Pengguna  
  2. Kelola Katalog Produk  
  3. Proses Pesanan  
  4. Proses Pembayaran  
  5. Tampilkan Laporan  

#### Level 1:  
- **Kelola Akun Pengguna**: Registrasi, login, pengelolaan profil.  
- **Kelola Katalog Produk**: Menambahkan, mengedit, menghapus, dan menampilkan produk.  
- **Proses Pesanan**: Pembeli menambahkan ke keranjang, membuat pesanan, memeriksa status.  
- **Proses Pembayaran**: Verifikasi pembayaran dan notifikasi status.  
- **Tampilkan Laporan**: Menghasilkan data penjualan.  

---

### 4. **Model Relasional**

1. **Tabel User**  
   - `id_user` (PK), `nama`, `email`, `password`, `role`, `alamat`, `telepon`.  

2. **Tabel Produk**  
   - `id_produk` (PK), `nama_produk`, `deskripsi`, `harga`, `stok`, `id_penjual` (FK ke User).  

3. **Tabel Keranjang**  
   - `id_keranjang` (PK), `id_user` (FK ke User), `id_produk` (FK ke Produk), `kuantitas`.  

4. **Tabel Pesanan**  
   - `id_pesanan` (PK), `id_user` (FK ke User), `status_pesanan`, `tanggal_pesanan`.  

5. **Tabel DetailPesanan**  
   - `id_detail` (PK), `id_pesanan` (FK ke Pesanan), `id_produk` (FK ke Produk), `kuantitas`, `subtotal`.  

6. **Tabel Pembayaran**  
   - `id_pembayaran` (PK), `id_pesanan` (FK ke Pesanan), `metode_pembayaran`, `status_pembayaran`, `tanggal_pembayaran`.  

---

### 5. **File SQL**

Berikut adalah contoh file SQL untuk membuat tabel dan relasi:

```sql
CREATE DATABASE e_commerce;

USE e_commerce;

-- Tabel User
CREATE TABLE User (
    id_user INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    role ENUM('Pembeli', 'Penjual'),
    alamat TEXT,
    telepon VARCHAR(15)
);

-- Tabel Produk
CREATE TABLE Produk (
    id_produk INT AUTO_INCREMENT PRIMARY KEY,
    nama_produk VARCHAR(100),
    deskripsi TEXT,
    harga DECIMAL(10,2),
    stok INT,
    id_penjual INT,
    FOREIGN KEY (id_penjual) REFERENCES User(id_user)
);

-- Tabel Keranjang
CREATE TABLE Keranjang (
    id_keranjang INT AUTO_INCREMENT PRIMARY KEY,
    id_user INT,
    id_produk INT,
    kuantitas INT,
    FOREIGN KEY (id_user) REFERENCES User(id_user),
    FOREIGN KEY (id_produk) REFERENCES Produk(id_produk)
);

-- Tabel Pesanan
CREATE TABLE Pesanan (
    id_pesanan INT AUTO_INCREMENT PRIMARY KEY,
    id_user INT,
    status_pesanan ENUM('Diproses', 'Dikirim', 'Selesai'),
    tanggal_pesanan DATETIME,
    FOREIGN KEY (id_user) REFERENCES User(id_user)
);

-- Tabel DetailPesanan
CREATE TABLE DetailPesanan (
    id_detail INT AUTO_INCREMENT PRIMARY KEY,
    id_pesanan INT,
    id_produk INT,
    kuantitas INT,
    subtotal DECIMAL(10,2),
    FOREIGN KEY (id_pesanan) REFERENCES Pesanan(id_pesanan),
    FOREIGN KEY (id_produk) REFERENCES Produk(id_produk)
);

-- Tabel Pembayaran
CREATE TABLE Pembayaran (
    id_pembayaran INT AUTO_INCREMENT PRIMARY KEY,
    id_pesanan INT,
    metode_pembayaran ENUM('Transfer Bank', 'E-Wallet', 'Kartu Kredit'),
    status_pembayaran ENUM('Pending', 'Berhasil', 'Gagal'),
    tanggal_pembayaran DATETIME,
    FOREIGN KEY (id_pesanan) REFERENCES Pesanan(id_pesanan)
);
```

---

### 6. **Laporan Keseluruhan**

Laporan akan mencakup:  
1. **Deskripsi Sistem** (seperti yang telah Anda tulis).  
2. **Diagram**: ERD, Context Diagram, DFD.  
3. **Desain Basis Data**: Model relasional dan relasi antar tabel.  
4. **Implementasi SQL**: File SQL yang telah disusun.  
5. **Uji Coba**: Contoh pengisian data dan query untuk memverifikasi fungsionalitas sistem.  
6. **Kesimpulan**: Evaluasi performa sistem dan saran pengembangan.
