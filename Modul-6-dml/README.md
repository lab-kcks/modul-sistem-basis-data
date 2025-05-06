# Modul 8: Join Table, Trigger & View

## Daftar Isi

- [1.Pengenalan JOIN dalam SQL](#1-pengenalan-join-dalam-sql)
  - [1.1 Self Join](#11-self-join)
  - [1.2 Outer Join pada Dua Tabel](#12-outer-join-pada-dua-tabel)
  - [1.3 Join Lebih dari Dua Tabel](#13-join-lebih-dari-dua-tabel)
- [2. Trigger](#2-trigger)
  - [2.1 Pengenalan](#21-pengenalan-trigger)
  - [2.2 Sintaks](#22-sintaks-trigger)
  - [2.3 Contoh Implementasi](#23-contoh-implementasi-trigger)
  - [2.4 Pengujian Trigger](#24-pengujian-trigger)
  - [2.5 Kapan Menggunakan Trigger (dan Kapan Tidak)](#25-kapan-menggunakan-trigger-dan-kapan-tidak)
- [3. View](#2-view)
  - [3.1 Pengenalan](#31-pengenalan-view)
  - [3.2 Sintaks](#32-sintaks-view)
  - [3.3 Contoh Implementasi](#33-contoh-implementasi-view)
  - [3.4 Pengujian View](#34-pengujian-view)
  - [3.5 Keuntungan dan Keterbatasan View](#35-keuntungan-dan-keterbatasan-view)

---
## 1. Pengenalan JOIN dalam SQL

![image](https://github.com/user-attachments/assets/12c166d6-6c5c-4290-bd7f-eee39ae8127e)

`JOIN` merupakan salah satu perintah penting dalam bahasa SQL (Structured Query Language) yang digunakan untuk menggabungkan data dari dua tabel atau lebih dengan memanfaatkan kolom yang memiliki hubungan logis di antara tabel-tabel tersebut. Dengan menggunakan perintah `JOIN`, kita dapat menyatukan data yang sebelumnya tersebar di berbagai tabel menjadi satu tampilan yang utuh, sehingga mempermudah dalam melakukan analisis atau pelaporan yang membutuhkan data dari banyak sumber.

#### Lalu mengapa JOIN ini diperlukan? 
Melalui mekanisme ini, kita dapat mengakses informasi yang lebih lengkap dan bermakna karena hubungan antar data menjadi lebih terlihat. `JOIN` sangat berguna dalam konteks sistem basis data relasional, di mana data biasanya disimpan dalam bentuk tabel-tabel terpisah yang saling terhubung melalui relasi tertentu seperti foreign key.

Secara umum, JOIN dalam SQL dibagi menjadi dua kelompok besar berdasarkan sifat penggabungannya:

- Self Join: Merupakan jenis join yang dilakukan pada tabel itu sendiri, yaitu ketika sebuah tabel digabungkan dengan salinan dirinya sendiri. Ini biasanya digunakan untuk membandingkan baris antar entri dalam satu tabel, misalnya untuk menemukan data yang memiliki atribut serupa.

- Outer Join: Digunakan untuk menggabungkan dua atau lebih tabel yang berbeda, termasuk baris-baris data yang mungkin tidak memiliki pasangan yang cocok di tabel lainnya. Outer Join sendiri terdiri dari beberapa variasi seperti Left Join, Right Join, dan Full Outer Join, yang masing-masing memiliki cara kerja dan hasil penggabungan yang berbeda tergantung kebutuhan.

---

### 1.1 Self Join

Self Join merupakan proses penggabungan data yang dilakukan antar baris dalam satu tabel. intinya, `JOIN` jenis ini berguna ketika kita ingin membandingkan data yang berada dalam satu tabel yang sama. Untuk contohnya bisa dilihat dibawah ini.

### Contoh Struktur Tabel:

![image](https://github.com/user-attachments/assets/f0354718-7bb9-4352-9513-3a21382c6bf7)

### Query:

```sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
````

### Penjelasan:

Query ini digunakan untuk mencari pasangan pelanggan yang tinggal di kota yang sama, tetapi merupakan entri data yang berbeda. Terlihat pada query tabel yang disebutkan hanyalah tabel `Customer` jadi bisa dibilang ini adalah self join.

### Hasil:

![image](https://github.com/user-attachments/assets/cd685058-2fc8-4aed-b62f-d972b674a682)

---

### 1.2 Outer Join pada Dua Tabel

![image](https://github.com/user-attachments/assets/2a23340e-e7ff-4023-a7ec-aaf73e36e30e)

Outer Join digunakan untuk menggabungkan isi dua tabel atau lebih, baik yang memiliki kecocokan maupun yang tidak. Dengan kata lain, jenis JOIN ini dapat menampilkan semua data dari salah satu tabel meskipun tidak ditemukan pasangan di tabel lainnya. Untuk contoh kasusnya bisa diliat dibawah.

### Struktur dan Data Contoh:

```sql
CREATE TABLE mahasiswa (
    nim INT(10),
    nama VARCHAR(100),
    alamat VARCHAR(100),
    PRIMARY KEY(nim)
);

CREATE TABLE transaksi (
    id_transaksi INT(10),
    nim INT(10),
    buku VARCHAR(100),
    PRIMARY KEY(id_transaksi)
);

INSERT INTO mahasiswa VALUES 
(21400200,"faqih","bandung"),
(21400201,"ina","jakarta"),
(21400202,"anto","semarang"),
(21400203,"dani","padang"),
(21400204,"jaka","bandung"),
(21400205,"nara","bandung"),
(21400206,"senta","semarang");

INSERT INTO transaksi VALUES 
(1,21400200,"Buku Informatika"),
(2,21400202,"Buku Teknik Elektro"),
(3,21400203,"Buku Matematika"),
(4,21400206,"Buku Fisika"),
(5,21400207,"Buku Bahasa Indonesia"),
(6,21400210,"Buku Bahasa Daerah"),
(7,21400211,"Buku Kimia");
```

---

#### 1.2.1 Inner Join

```sql
SELECT *
FROM mahasiswa
INNER JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Inner JOIN Menampilkan data mahasiswa yang memiliki relasi transaksi, dan hanya baris yang cocok pada kedua tabel yang akan ditampilkan. In kalkulus terms, intinya Inner JOIN itu menampilkan irisan dari kedua tabel tersebut. 

![image](https://github.com/user-attachments/assets/ededd437-1980-42d6-b6fa-1d945c4da599)

---

#### 1.2.2 Left Join

```sql
SELECT *
FROM mahasiswa
LEFT JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Menampilkan semua mahasiswa beserta transaksinya, jika ada. Jika tidak ada, maka kolom dari tabel `transaksi` akan berisi `NULL`.

![image](https://github.com/user-attachments/assets/895393af-7e96-4eee-a283-54eb3a1ca804)

---

#### 1.2.3 Right Join

```sql
SELECT *
FROM mahasiswa
RIGHT JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Menampilkan semua data dari `transaksi`, dan akan melampirkan data mahasiswa jika ditemukan pasangan yang cocok.

![image](https://github.com/user-attachments/assets/3f148eef-bb9f-456c-8951-e946fba6c307)

---

#### 1.2.4 Full Outer Join

```sql
SELECT *
FROM mahasiswa
FULL OUTER JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Jika sistem basis data tidak mendukung `FULL OUTER JOIN`, dapat disiasati dengan:

```sql
SELECT *
FROM mahasiswa
LEFT JOIN transaksi ON mahasiswa.nim = transaksi.nim
UNION ALL
SELECT *
FROM mahasiswa
RIGHT JOIN transaksi ON mahasiswa.nim = transaksi.nim
WHERE mahasiswa.nim IS NULL;
```

Menggabungkan semua baris dari kedua tabel, baik yang cocok maupun tidak. Simplenya ini adalah gabungan dari kedua isi tabel yang disebutkan di query.

![image](https://github.com/user-attachments/assets/b870fba3-29a4-461d-8f22-4e6dffa55cc5)

---

### 1.3 Join Lebih dari Dua Tabel

Untuk menggabungkan lebih dari dua tabel, cukup menambahkan klausa `JOIN` secara berurutan dengan kondisi penghubung yang sesuai. Untuk studi kasusnya bisa dilihat dibawah. 

### Contoh Studi Kasus:

```sql
CREATE TABLE Pemain (
    id_pemain CHAR(5),
    nama CHAR(20),
    id_posisi CHAR(5),
    id_status CHAR(5)
);

INSERT INTO Pemain VALUES
('1', 'Budi', '3','2'),
('2', 'Rindang', '3','1'),
('3', 'Bintang', '3','1'),
('4', 'Wana', '4','3'),
('5', 'Orion', '1','3');

CREATE TABLE Posisi (
    id_posisi CHAR(5),
    ket_posisi CHAR(20)
);

INSERT INTO Posisi VALUES
('1', 'Penyerang'),
('2', 'PenyerangLapis2'),
('3', 'Gelandang'),
('4', 'SayapKanan'),
('5', 'SayapKiri');

CREATE TABLE Status (
    id_status CHAR(5),
    ket_status CHAR(20)
);

INSERT INTO Status VALUES
('1', 'Sehat'),
('2', 'Cedera'),
('3', 'Dipinjam');
```

### Query:

```sql
SELECT nama, ket_status, ket_posisi 
FROM pemain
JOIN posisi ON pemain.id_posisi = posisi.id_posisi
JOIN status ON pemain.id_status = status.id_status
WHERE posisi.ket_posisi = 'Gelandang'
AND status.ket_status = 'Cedera';
```

Query diatas ini akan mencari pemain dengan posisi "Gelandang" yang sedang dalam kondisi "Cedera". Maka dari itu, untuk mencarinya kita memerlukan penggabungan 3 tabel yaitu tabel `pemain`,`posisi` dan `status`

![image](https://github.com/user-attachments/assets/1a6e540b-213d-44fb-9a9a-98ff054827f7)


Pemahaman terhadap berbagai jenis JOIN sangat penting dalam pengelolaan data relasional. Dengan memilih jenis JOIN yang sesuai, kita dapat menyusun query yang efisien dan informatif, serta memperoleh data dari banyak tabel secara menyeluruh.

## 2. Trigger

### 2.1 Pengenalan Trigger

**Apa itu Trigger?**

Trigger adalah sebuah prosedur tersimpan (stored procedure) dalam database yang **otomatis berjalan (*auto execute*)** ketika terjadi suatu *event* (kejadian) tertentu pada sebuah tabel. Event ini biasanya adalah operasi Data Manipulation Language (DML) seperti `INSERT`, `UPDATE`, atau `DELETE`.

**Analogi Sederhana:**

Bayangkan ketika kita memiliki sistem alarm bel pada pintu. Ketika seseorang menekan tombol pada bel (`event` = menekan tombol), maka bel akan berbunyi (`action` = bunyi bel). Trigger pada database memiliki cara kerja yang mirip dengan seperti itu. Ketika dilakukan *insert/update/delete* pada sebuah tabel tertentu (`event`), maka trigger akan menjalankan serangkaian perintah SQL (`action`) secara otomatis.

**Mengapa Trigger Penting?**

1.  **Otomatisasi:** Menjalankan tugas-tugas secara otomatis tanpa perlu intervensi manual atau kode aplikasi tambahan.
2.  **Integritas Data:** Memastikan aturan bisnis yang kompleks atau validasi data tetap terjaga di level database secara otomatis. Contoh: Stok parfum pada toko GPI tidak akan memiliki nilai negatif.
3.  **Auditing/Logging:** Mencatat perubahan data secara otomatis ke tabel log. Ini sangat bermanfaat untuk mengetahui catatan akses seseorang pada sebuah database.
4.  **Sinkronisasi Data:** Memperbarui data di tabel lain secara otomatis ketika data di satu tabel berubah.

### 2.2 Sintaks Trigger

Sintaks dasar untuk membuat trigger bisa sedikit berbeda antar sistem manajemen basis data (DBMS) seperti MySQL, PostgreSQL, SQL Server, atau Oracle. Namun, konsep dasarnya mirip. Berikut adalah contoh sintaks umum (mirip MySQL/PostgreSQL):

```sql
DELIMITER $$
CREATE TRIGGER nama_trigger
{BEFORE | AFTER} {INSERT | UPDATE| DELETE }
    ON nama_table 
    FOR EACH ROW
BEGIN
    KODE SQL
END$$
DELIMITER ;
```
**Penjelasan Komponen Sintaks Trigger:**

Untuk memulai menggunakan TRIGGER, kita menggunakan `CREATE TRIGGER` dilanjutkan dengan nama TRIGGER yang ingin dibuat. `{BEFORE | AFTER}` adalah waktu TRIGGER akan dijalankan, apakah sebelum atau sesudah database dimodifikasi oleh perintah DML. `{INSERT | UPDATE | DELETE}` adalah perintah DML yang mengaktifkan TRIGGER. `ON` mendefinisikan table yang mengaktifkan TRIGGER. `BEGIN END` adalah pernyataan yang membungkus kode TRIGGER. Pastikan diawal gunakan `DELIMITER $$` dan diakhir dikembalikan ke `DELIMITER;`

Jenis-jenis TRIGGER dapat dibagi menjadi beberapa jenis, yang akan dijelaskan di tabel berikut:

| No | Waktu TRIGGER | Keterangan TRIGGER                                     |
|----|---------------|--------------------------------------------------------|
| 1  | BEFORE INSERT | TRIGGER yang dijalankan sebelum record dimasukkan ke database |
| 2  | AFTER INSERT  | TRIGGER yang dijalankan sesudah record dimasukkan ke database  |
| 3  | BEFORE UPDATE | TRIGGER yang dijalankan sebelum record diubah di database     |
| 4  | AFTER UPDATE  | TRIGGER yang dijalankan sesudah record diubah database        |
| 5  | BEFORE DELETE | TRIGGER yang dijalankan sebelum record dihapus di database    |
| 6  | AFTER DELETE  | TRIGGER yang dijalankan sesudah record dihapus di database     |

## 2.3 Contoh Implementasi Trigger

**Studi Kasus: Membuat Log Audit Perubahan Harga Produk**

Di banyak sistem e-commerce atau inventaris, penting untuk melacak riwayat perubahan harga produk. Kita bisa menggunakan trigger untuk mencatat setiap kali harga produk diubah.

**Langkah-langkah:**

1.  **Buat Tabel Produk:**
    ```sql
    CREATE TABLE Produk (
        IDProduk INT PRIMARY KEY AUTO_INCREMENT,
        NamaProduk VARCHAR(100) NOT NULL,
        Harga DECIMAL(10, 2) NOT NULL,
        Stok INT DEFAULT 0
    );
    ```

2.  **Buat Tabel Log Perubahan Harga:**
    ```sql
    CREATE TABLE LogPerubahanHarga (
        IDLog INT PRIMARY KEY AUTO_INCREMENT,
        IDProduk INT,
        HargaLama DECIMAL(10, 2),
        HargaBaru DECIMAL(10, 2),
        WaktuPerubahan TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        PenggunaUbah VARCHAR(50), -- Opsional: Siapa yang mengubah
        -- FOREIGN KEY (IDProduk) REFERENCES Produk(IDProduk) -- Sebaiknya ditambahkan
    );
    ```

3.  **Buat Trigger:**
    Trigger ini akan berjalan *setelah* ada `UPDATE` pada tabel `Produk` dan hanya jika kolom `Harga` benar-benar berubah.
    *(Contoh sintaks MySQL)*
    ```sql
    DELIMITER $$ -- Mengubah delimiter sementara agar bisa pakai ; di dalam trigger

    CREATE TRIGGER trg_after_update_harga_produk
    AFTER UPDATE ON Produk
    FOR EACH ROW
    BEGIN
        -- Hanya catat jika harga benar-benar berubah
        IF OLD.Harga <> NEW.Harga THEN
            INSERT INTO LogPerubahanHarga (IDProduk, HargaLama, HargaBaru, PenggunaUbah)
            VALUES (OLD.IDProduk, OLD.Harga, NEW.Harga, USER()); -- USER() mengambil user DB saat ini
        END IF;
    END; $$

    DELIMITER ; -- Mengembalikan delimiter ke ;
    ```

**Penjelasan Trigger:**

*   Trigger bernama `trg_after_update_harga_produk`.
*   Berjalan `AFTER UPDATE` pada tabel `Produk`.
*   `FOR EACH ROW` agar trigger dieksekusi untuk setiap baris produk yang diupdate.
*   `IF OLD.Harga <> NEW.Harga THEN ... END IF;`: Ini adalah bagian penting. Trigger hanya akan memasukkan data ke `LogPerubahanHarga` jika nilai `Harga` yang lama (`OLD.Harga`) tidak sama dengan nilai `Harga` yang baru (`NEW.Harga`). Ini mencegah logging jika ada `UPDATE` pada baris tapi kolom harga tidak ikut diubah.
*   `INSERT INTO LogPerubahanHarga ...`: Memasukkan data ID produk, harga lama, harga baru, dan user database saat ini ke tabel log.

### 2.4 Pengujian Trigger

Setelah trigger dibuat, kita perlu mengujinya untuk memastikan ia bekerja sesuai harapan.

1.  **Masukkan Data Awal ke Tabel Produk:**
    ```sql
    INSERT INTO Produk (NamaProduk, Harga, Stok)
    VALUES ('ASUS TUF F15 FX5027ZM', 19000000.00, 10),
           ('Noir M1 Nex', 275000.00, 50);
    ```

2.  **Lakukan Operasi UPDATE yang Mempengaruhi Harga:**
    ```sql
    -- Naikkan harga Laptop
    UPDATE Produk
    SET Harga = 19500000.00
    WHERE IDProduk = 1;

    -- Coba update stok saja (harga tidak berubah)
    UPDATE Produk
    SET Stok = 45
    WHERE IDProduk = 2;

    -- Coba update harga Noir
    UPDATE Produk
    SET Harga = 300000.00, Stok = 40
    WHERE IDProduk = 2;
    ```

3.  **Periksa Isi Tabel Log:**
    ```sql
    SELECT * FROM LogPerubahanHarga;
    ```

**Hasil yang Diharapkan:**

*   Anda akan melihat dua baris di `LogPerubahanHarga`:
    *   Satu untuk perubahan harga `ASUS TUF F15 FX5027ZM` (dari 19000000.00 ke 19500000.00).
    *   Satu lagi untuk perubahan harga `Noir M1 Nex` (dari 350000.00 ke 375000.00).
*   Tidak ada log untuk `UPDATE` kedua yang hanya mengubah `Stok`, karena kondisi `IF OLD.Harga <> NEW.Harga` tidak terpenuhi.

### 2.5 Kapan Menggunakan Trigger (dan Kapan Tidak)

**Gunakan Trigger Ketika:**

*   Perlu **otomatisasi** logika yang *harus* terjadi setiap kali data dimanipulasi (`INSERT`/`UPDATE`/`DELETE`).
*   Ingin memastikan **konsistensi data** atau aturan bisnis yang kompleks di level database, terlepas dari aplikasi mana yang mengaksesnya.
*   Membutuhkan **auditing/logging** perubahan data yang andal.
*   Perlu melakukan **aksi berantai sederhana** (misalnya, update tabel ringkasan saat data detail berubah).

**Pertimbangkan Alternatif / Hati-hati Menggunakan Trigger Ketika:**

*   **Logika Kompleks:** Trigger yang terlalu rumit bisa sulit dipahami, di-debug, dan dipelihara. Terkadang lebih baik menempatkan logika bisnis yang kompleks di lapisan aplikasi.
*   **Kinerja:** Trigger menambahkan *overhead* pada setiap operasi DML yang memicunya. Trigger yang tidak efisien atau terlalu banyak trigger pada satu tabel dapat memperlambat kinerja database secara signifikan.
*   **Debugging:** Error dalam trigger bisa lebih sulit dilacak dibandingkan error di kode aplikasi.
*   **Ketergantungan Tersembunyi:** Trigger menciptakan perilaku "tersembunyi". Developer mungkin tidak sadar ada trigger yang berjalan di balik layar, yang bisa menyebabkan efek samping tak terduga.
*   **Alternatif:** Pertimbangkan apakah tugas tersebut bisa dicapai dengan *constraints* (`CHECK`, `FOREIGN KEY`), *stored procedure* yang dipanggil secara eksplisit oleh aplikasi, atau logika di lapisan aplikasi.

## 3. View

### 3.1 Pengenalan View

**Apa itu View?**

View adalah sebuah **tabel virtual** yang isinya didefinisikan oleh sebuah query SQL. View tidak menyimpan data secara fisik (kecuali pada *materialized view*, konsep yang lebih advance). Sebaliknya, view menyimpan query `SELECT` itu sendiri. Ketika Anda melakukan query terhadap view, database akan menjalankan query `SELECT` yang tersimpan tersebut dan menampilkan hasilnya seolah-olah berasal dari tabel biasa.

View memudahkan siapa saja mengakses informasi yang dibutuhkan tanpa harus terjebak dalam query yang rumit, serta berperan sebagai penjaga keamanan dengan menjaga informasi sensitif tetap tersembunyi, sambil tetap memungkinkan akses ke data yang diperlukan.

**Analogi Sederhana:**

Bayangkan kita seorang pemilik cafe dimana sering membutuhkan laporan yang menggabungkan data dari tabel `Pelanggan` dan `Pesanan` untuk melihat pelanggan mana saja yang memesan dalam 30 hari terakhir. Menulis query `JOIN` yang sama berulang kali tentu merepotkan. Dengan View, kita bisa "menyimpan" query `JOIN` tersebut dengan sebuah nama (misalnya `PelangganAktifTerakhir`). Selanjutnya, Anda cukup melakukan `SELECT * FROM PelangganAktifTerakhir;` untuk mendapatkan hasilnya, tanpa perlu menulis ulang query `JOIN` yang panjang. View ini seperti *shortcut* atau *preset* untuk query Anda.

**Mengapa View Penting?**

*   **Penyederhanaan Query:** Menyembunyikan kompleksitas query `JOIN`, `WHERE`, atau fungsi agregasi. Pengguna cukup melakukan query sederhana terhadap view.
*   **Keamanan Data:** Membatasi akses pengguna ke data tertentu. Anda bisa membuat view yang hanya menampilkan kolom-kolom yang relevan bagi pengguna tersebut, menyembunyikan kolom sensitif yang tidak bisa dijadikan *open-source* ~~layaknya database di pemerintahan Konoha~~ (seperti gaji, data pribadi).
*   **Konsistensi Data:** Menyajikan data dalam format yang konsisten kepada pengguna atau aplikasi, meskipun struktur tabel dasarnya berubah (selama view masih valid).
*   **Enkapsulasi Logika:** Menyimpan logika query yang sering digunakan di satu tempat (definisi view), sehingga lebih mudah dikelola dan diubah jika diperlukan.

### 3.2 Sintaks View

Sintaks dasar untuk membuat view cukup standar di berbagai DBMS:

```sql
CREATE VIEW nama_view AS
SELECT kolom1, kolom2, ...
FROM nama_tabel
WHERE kondisi
-- Bisa juga melibatkan JOIN, GROUP BY, HAVING, dll.
;
```
**Penjelasan Komponen Sintaks View:**

*   `CREATE VIEW nama_view`: Memberi nama pada view.
*   `AS`: Keyword yang menandakan dimulainya query `SELECT` yang mendefinisikan view.
*   `SELECT kolom1, kolom2, ... FROM ... WHERE ...`: Query SQL standar yang menentukan data apa saja yang akan ditampilkan oleh view. Query ini bisa sesederhana memilih beberapa kolom dari satu tabel, hingga query kompleks yang melibatkan banyak tabel, fungsi, dan klausa.

**Opsional:**

*   `CREATE OR REPLACE VIEW`: Jika view dengan nama tersebut sudah ada, ia akan diganti dengan definisi baru. Jika belum ada, ia akan dibuat. Ini berguna saat memperbarui definisi view.

### 3.3 Contoh Implementasi View

**Studi Kasus: Membuat Tampilan Sederhana Data Karyawan untuk Tim HR**

Tim HR seringkali hanya perlu melihat informasi dasar karyawan seperti nama, departemen, dan jabatan, tanpa perlu melihat informasi teknis atau data sensitif lainnya yang mungkin ada di tabel utama `Karyawan`.

**Langkah-langkah:**

1.  **Asumsikan Tabel Karyawan dan Departemen:**
    ```sql
    -- Tabel Karyawan 
    CREATE TABLE Karyawan (
        IDKaryawan INT PRIMARY KEY AUTO_INCREMENT,
        NamaLengkap VARCHAR(100) NOT NULL,
        Email VARCHAR(100) UNIQUE,
        TanggalLahir DATE,
        IDDepartemen INT,
        Jabatan VARCHAR(50),
        Gaji DECIMAL(22, 2), 
        NomorTelepon VARCHAR(20)
        -- FOREIGN KEY (IDDepartemen) REFERENCES Departemen(IDDepartemen)
    );

    -- Tabel Departemen
    CREATE TABLE Departemen (
        IDDepartemen INT PRIMARY KEY AUTO_INCREMENT,
        NamaDepartemen VARCHAR(50) NOT NULL
    );
    ```

2.  **Masukkan Data Contoh:**
    ```sql
    INSERT INTO Departemen (NamaDepartemen) VALUES ('Teknologi Informasi'), ('Sumber Daya Manusia'), ('Pemasaran');

    INSERT INTO Karyawan (NamaLengkap, Email, IDDepartemen, Jabatan, Gaji, NomorTelepon) VALUES
    ('Budi Speed', 'budi.s@example.com', 1, 'Software Engineer', 15000000.00, '082234567890'),
    ('Carlo Ancelotti', 'doncarlo@example.com', 2, 'HR Manager', 18000000.00, '082222222222'),
    ('Sergio Ramos', 'sr4@example.com', 2, 'HR Staff', 8000000.00, '082233334444'),
    ('Christiano Messi', 'siuuu@example.com', 1, 'System Analyst', 14000000.00, '082255556666');
    ```

3.  **Buat View:**
    View ini akan menggabungkan data dari `Karyawan` dan `Departemen`, tetapi hanya menampilkan kolom yang relevan untuk HR dan menyembunyikan gaji.
    ```sql
    CREATE VIEW InfoKaryawanHR AS
    SELECT
        k.IDKaryawan,
        k.NamaLengkap,
        k.Email,
        d.NamaDepartemen,
        k.Jabatan,
        k.NomorTelepon
    FROM
        Karyawan k
    JOIN
        Departemen d ON k.IDDepartemen = d.IDDepartemen;
    ```

**Penjelasan View:**

*   View bernama `InfoKaryawanHR`.
*   Query `SELECT`-nya memilih `IDKaryawan`, `NamaLengkap`, `Email`, `NamaDepartemen` (dari tabel `Departemen`), `Jabatan`, dan `NomorTelepon`.
*   Menggunakan `JOIN` untuk menghubungkan tabel `Karyawan` (`k`) dan `Departemen` (`d`) berdasarkan `IDDepartemen`.
*   Kolom `Gaji` dan kolom lain yang tidak relevan (misal: `TanggalLahir`) tidak dimasukkan dalam `SELECT`, sehingga tersembunyi dari pengguna view ini.

### 3.4 Pengujian View

Menguji view sangat mudah. kita cukup melakukan query `SELECT` terhadap view tersebut seolah-olah ia adalah tabel biasa.

1.  **Tampilkan Semua Data dari View:**
    ```sql
    SELECT * FROM InfoKaryawanHR;
    ```
    *Hasilnya akan menampilkan data gabungan dari karyawan dan departemen mereka, sesuai kolom yang didefinisikan di view (tanpa Gaji).*

2.  **Filter Data dari View:**
    Anda bisa menggunakan klausa `WHERE` pada view seperti pada tabel biasa.
    ```sql
    -- Tampilkan karyawan dari departemen Sumber Daya Manusia
    SELECT NamaLengkap, Jabatan
    FROM InfoKaryawanHR
    WHERE NamaDepartemen = 'Sumber Daya Manusia';
    ```
    *Hasilnya akan menampilkan 'Carlo Ancelotti' dan 'Sergio Ramos' beserta jabatan mereka.*

### 3.5 Keuntungan dan Keterbatasan View

**Keuntungan View:**

*   **Penyederhanaan:** Menyembunyikan kompleksitas query.
*   **Keamanan:** Membatasi akses data pada level kolom atau baris.
*   **Konsistensi:** Memberikan antarmuka data yang stabil.
*   **Reusable:** Query yang disimpan bisa digunakan berkali-kali.

**Keterbatasan View:**

*   **Kinerja:** View yang didasarkan pada query yang sangat kompleks atau tidak efisien bisa jadi lambat karena database tetap harus menjalankan query dasar setiap kali view diakses.
*   **Updatability:** Tidak semua view bisa di-`UPDATE`, `INSERT`, atau `DELETE`. Secara umum, view bisa diupdate jika:
    *   Berasal dari satu tabel dasar saja.
    *   Tidak menggunakan fungsi agregasi (`COUNT`, `SUM`, `AVG`, dll.).
    *   Tidak menggunakan `GROUP BY` atau `HAVING`.
    *   Tidak menggunakan `DISTINCT`.
    *   Aturan spesifik bisa berbeda antar DBMS. Mencoba melakukan DML pada view yang tidak dapat diupdate akan menghasilkan error.
*   **Ketergantungan:** Jika struktur tabel dasar yang digunakan oleh view berubah (misalnya kolom dihapus atau diganti namanya), view tersebut bisa menjadi tidak valid dan perlu dibuat ulang atau diubah.

---

## Sumber

- https://www.nesabamedia.com/pengertian-trigger/

- https://www.geeksforgeeks.org/different-types-of-mysql-triggers-with-examples/

- https://www.tutorialspoint.com/mysql/mysql-triggers.htm

- [SQL Views (geeksforgeeks.org)](https://www.geeksforgeeks.org/sql-views/?ref=gcse)

- [SQL Tutorial - GeeksforGeeks](https://www.geeksforgeeks.org/sql-tutorial/?ref=header_search)

- [SQL: Triggers and Views](https://dev.to/spo0q/sql-triggers-views-2abk)

- [SQL Triggers: A Guide for Developers](https://www.datacamp.com/tutorial/sql-triggers)
