# Modul 5: Trigger, Procedure, dan Function

## Daftar Isi

- [Modul 5: Trigger, Procedure, dan Function](#modul-7-procedure-dan-function)
  - [Daftar Isi](#daftar-isi)
  - [1. Trigger dalam SQL](#1-trigger-dalam-sql)
    - [1.1 Pengenalan Trigger](#11-pengenalan-trigger)
    - [1.2 Sintaks Trigger](#12-sintaks-trigger)
    - [1.3 Contoh Implementasi Trigger](#13-contoh-implementasi-trigger)
    - [1.4 Pengujian Trigger](#14-pengujian-trigger)
    - [1.5 Kapan Menggunakan Trigger (dan Kapan Tidak)](#15-kapan-menggunakan-trigger-dan-kapan-tidak)
  - [2. Pengenalan Procedure dan Function dalam SQL](#1-pengenalan-procedure-dan-function-dalam-sql)
    - [2.1 Jenis-jenis Procedure dan Function di MySQL](#11-jenis-jenis-procedure-dan-function-di-mysql)
    - [2.2 Mengapa Procedure dan Function Penting?](#12-mengapa-procedure-dan-function-penting)
  - [3. Perbedaan Procedure dan Function](#2-perbedaan-procedure-dan-function)
  - [4. Terminologi](#3-terminologi)
    - [4.1 Parameter](#31-parameter)
    - [4.2 Return](#32-return)
  - [5. Syntax](#4-syntax)
    - [5.1 Untuk Membuat Procedure atau Function](#41-untuk-membuat-procedure-atau-function)
      - [**Procedure**](#procedure)
      - [**Function**](#function)
    - [5.2 Untuk Menghapus Procedure atau Function](#42-untuk-menghapus-procedure-atau-function)
    - [5.3 Untuk Memanggil Procedure atau Function](#43-untuk-memanggil-procedure-atau-function)
    - [5.4 Untuk Melihat List Procedure atau Function](#44-untuk-melihat-list-procedure-atau-function)
  - [6. Demo](#5-demo)
    - [6.1 Uji Coba Procedure](#51-uji-coba-procedure)
      - [6.1.1 Procedure Parameter IN](#511-procedure-parameter-in)
      - [6.1.2 Procedure Parameter OUT](#512-procedure-parameter-out)
      - [6.1.3 Procedure Parameter INOUT](#513-procedure-parameter-inout)
      - [6.1.4 Procedure Parameter IN dan OUT](#514-procedure-parameter-in-dan-out)
      - [6.1.5 Procedure Compound Statement](#515-procedure-compound-statement)
    - [6.2 Uji Coba Function](#52-uji-coba-function)

---

## 1. Trigger dalam SQL

### 1.1 Pengenalan Trigger

**Apa itu Trigger?**

Trigger adalah sebuah prosedur tersimpan (stored procedure) dalam database yang **otomatis berjalan (_auto execute_)** ketika terjadi suatu _event_ (kejadian) tertentu pada sebuah tabel. Event ini biasanya adalah operasi Data Manipulation Language (DML) seperti `INSERT`, `UPDATE`, atau `DELETE`.

**Analogi Sederhana:**

Bayangkan ketika kita memiliki sistem alarm bel pada pintu. Ketika seseorang menekan tombol pada bel (`event` = menekan tombol), maka bel akan berbunyi (`action` = bunyi bel). Trigger pada database memiliki cara kerja yang mirip dengan seperti itu. Ketika dilakukan _insert/update/delete_ pada sebuah tabel tertentu (`event`), maka trigger akan menjalankan serangkaian perintah SQL (`action`) secara otomatis.

**Mengapa Trigger Penting?**

1.  **Otomatisasi:** Menjalankan tugas-tugas secara otomatis tanpa perlu intervensi manual atau kode aplikasi tambahan.
2.  **Integritas Data:** Memastikan aturan bisnis yang kompleks atau validasi data tetap terjaga di level database secara otomatis. Contoh: Stok parfum pada toko GPI tidak akan memiliki nilai negatif.
3.  **Auditing/Logging:** Mencatat perubahan data secara otomatis ke tabel log. Ini sangat bermanfaat untuk mengetahui catatan akses seseorang pada sebuah database.
4.  **Sinkronisasi Data:** Memperbarui data di tabel lain secara otomatis ketika data di satu tabel berubah.

### 1.2 Sintaks Trigger

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

| No  | Waktu TRIGGER | Keterangan TRIGGER                                            |
| --- | ------------- | ------------------------------------------------------------- |
| 1   | BEFORE INSERT | TRIGGER yang dijalankan sebelum record dimasukkan ke database |
| 2   | AFTER INSERT  | TRIGGER yang dijalankan sesudah record dimasukkan ke database |
| 3   | BEFORE UPDATE | TRIGGER yang dijalankan sebelum record diubah di database     |
| 4   | AFTER UPDATE  | TRIGGER yang dijalankan sesudah record diubah database        |
| 5   | BEFORE DELETE | TRIGGER yang dijalankan sebelum record dihapus di database    |
| 6   | AFTER DELETE  | TRIGGER yang dijalankan sesudah record dihapus di database    |

## 1.3 Contoh Implementasi Trigger

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
    Trigger ini akan berjalan _setelah_ ada `UPDATE` pada tabel `Produk` dan hanya jika kolom `Harga` benar-benar berubah.
    _(Contoh sintaks MySQL)_

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

- Trigger bernama `trg_after_update_harga_produk`.
- Berjalan `AFTER UPDATE` pada tabel `Produk`.
- `FOR EACH ROW` agar trigger dieksekusi untuk setiap baris produk yang diupdate.
- `IF OLD.Harga <> NEW.Harga THEN ... END IF;`: Ini adalah bagian penting. Trigger hanya akan memasukkan data ke `LogPerubahanHarga` jika nilai `Harga` yang lama (`OLD.Harga`) tidak sama dengan nilai `Harga` yang baru (`NEW.Harga`). Ini mencegah logging jika ada `UPDATE` pada baris tapi kolom harga tidak ikut diubah.
- `INSERT INTO LogPerubahanHarga ...`: Memasukkan data ID produk, harga lama, harga baru, dan user database saat ini ke tabel log.

### 1.4 Pengujian Trigger

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

- Anda akan melihat dua baris di `LogPerubahanHarga`:
  - Satu untuk perubahan harga `ASUS TUF F15 FX5027ZM` (dari 19000000.00 ke 19500000.00).
  - Satu lagi untuk perubahan harga `Noir M1 Nex` (dari 350000.00 ke 375000.00).
- Tidak ada log untuk `UPDATE` kedua yang hanya mengubah `Stok`, karena kondisi `IF OLD.Harga <> NEW.Harga` tidak terpenuhi.

### 1.5 Kapan Menggunakan Trigger (dan Kapan Tidak)

**Gunakan Trigger Ketika:**

- Perlu **otomatisasi** logika yang _harus_ terjadi setiap kali data dimanipulasi (`INSERT`/`UPDATE`/`DELETE`).
- Ingin memastikan **konsistensi data** atau aturan bisnis yang kompleks di level database, terlepas dari aplikasi mana yang mengaksesnya.
- Membutuhkan **auditing/logging** perubahan data yang andal.
- Perlu melakukan **aksi berantai sederhana** (misalnya, update tabel ringkasan saat data detail berubah).

**Pertimbangkan Alternatif / Hati-hati Menggunakan Trigger Ketika:**

- **Logika Kompleks:** Trigger yang terlalu rumit bisa sulit dipahami, di-debug, dan dipelihara. Terkadang lebih baik menempatkan logika bisnis yang kompleks di lapisan aplikasi.
- **Kinerja:** Trigger menambahkan _overhead_ pada setiap operasi DML yang memicunya. Trigger yang tidak efisien atau terlalu banyak trigger pada satu tabel dapat memperlambat kinerja database secara signifikan.
- **Debugging:** Error dalam trigger bisa lebih sulit dilacak dibandingkan error di kode aplikasi.
- **Ketergantungan Tersembunyi:** Trigger menciptakan perilaku "tersembunyi". Developer mungkin tidak sadar ada trigger yang berjalan di balik layar, yang bisa menyebabkan efek samping tak terduga.
- **Alternatif:** Pertimbangkan apakah tugas tersebut bisa dicapai dengan _constraints_ (`CHECK`, `FOREIGN KEY`), _stored procedure_ yang dipanggil secara eksplisit oleh aplikasi, atau logika di lapisan aplikasi.

## 2. Pengenalan Procedure dan Function dalam SQL

**Procedure** dan **function** adalah **subprogram** yang dapat dipanggil secara berulang dan disimpan dalam **MySQL**. Penggunaan dari **procedure** dan **function** sangat membantu untuk memangkas waktu ketika menulis sebuah query di MySQL, karena logika program bisa disimpan dan digunakan kembali.

Mirip dengan membuat fungsi dalam bahasa pemrograman lain, **procedure** dapat menerima **input (parameter)** dari pengguna dan kemudian beroperasi sesuai dengan parameter tersebut.

### 2.1 Jenis-jenis Procedure dan Function di MySQL

1. **Built-in Function** \
   Yaitu fungsi yang **secara otomatis tersedia** di MySQL dan bisa langsung digunakan tanpa konfigurasi tambahan. \
    Contoh:

   ```SQL
   Count(), Sum(), AVG()
   ```

2. **User-made Function** \
    Merupakan fungsi yang **dibuat oleh pengguna** untuk menjalankan tugas tertentu sesuai kebutuhan program.
   Contoh: **Procedure** dan **Function.**

### 2.2 Mengapa Procedure dan Function Penting?

Penggunaan **procedure** dan **function** di MySQL penting karena meningkatkan efisiensi dan konsistensi dalam pengelolaan database. Dengan menyimpan logika dalam satu tempat, kita tidak perlu menulis ulang query yang sama, sehingga lebih hemat waktu dan mudah dikelola. Selain itu, struktur kode menjadi lebih rapi dan terorganisir, lebih aman karena bisa membatasi akses langsung ke data, serta memastikan hasil yang seragam di berbagai bagian sistem.

---

## 3. Perbedaan Procedure dan Function

| PROCEDURE                                                             | FUNCTION                                                                        |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Digunakan terutama untuk menjalankan proses tertentu                  | Digunakan terutama untuk melakukan beberapa perhitungan                         |
| Tidak dapat dipanggil dalam pernyataan SELECT                         | Fungsi yang tidak berisi pernyataan DML dapat dipanggil dalam pernyataan SELECT |
| Gunakan parameter OUT untuk mengembalikan nilai                       | Gunakan RETURN untuk mengembalikan nilai                                        |
| Tidak wajib mengembalikan nilainya                                    | Wajib mengembalikan nilainya                                                    |
| RETURN hanya akan keluar dari kontrol dari subprogram                 | RETURN akan keluar dari kontrol dari subprogram dan juga mengembalikan value    |
| Tipe data yang dikembalikan tidak akan ditentukan pada saat pembuatan | Mengembalikan tipe data wajib pada saat pembuatan                               |

---

## 4. Terminologi

### 4.1 Parameter

Parameter merupakan variabel memori yang digunakan untuk menerima suatu nilai dari suatu fungsi yang memanggil parameter tadi.

Terdapat 3 mode parameter yaitu: **IN**, **OUT**, dan **INOUT**

- **IN**

Parameter ini digunakan untuk memberikan input ke subprogram. Nilainya konstan. Dalam pernyataan pemanggilan, parameter ini dapat berupa variabel atau nilai literal atau ekspresi. Secara default, parameternya adalah tipe IN.

- **OUT**

Parameter ini digunakan untuk mengambil dan mengembalikan output dari subprogram. Nilainya bisa berubah. Dalam pernyataan pemanggilan, parameter ini harus berupa variabel.

- **INOUT**

Parameter ini merupakan gabungan dari IN dan OUT. Parameter ini dapat digunakan untuk memberikan input dan juga mengambil output.

### 4.2 Return

RETURN merupakan instruksi kompiler untuk mengembalikan nilai dan mengalihkan kontrol dari subprogram ke pernyataan panggilan.

---

## 5. Syntax

### 5.1 Untuk Membuat Procedure atau Function

#### **Procedure**

```sql
CREATE PROCEDURE sp_name ([proc_arameter [,…]])
routine_body
```

atau

```sql
CREATE PROCEDURE procedure_name
AS
{query}
GO;
```

atau

```sql
CREATE PROCEDURE procedure_name
AS
BEGIN
    -- SQL statements or business logic here
END;
```

contoh

```sql
CREATE PROCEDURE SelectAllCustomers
AS
SELECT * FROM Customers
GO;
```

#### **Function**

```sql
CREATE FUNCTION sp_name ([func_parameter [,…]])
RETURNS type
routine_body
```

atau

```sql
CREATE FUNCTION function_name (
   parameter1 datatype,
   parameter2 datatype,
   ...
)
RETURNS return_datatype
AS
BEGIN
    -- SQL statements to define the function logic

    RETURN expression;
    -- Return statement indicating the result of the function
END;
```

Contoh:

```sql
CREATE FUNCTION CalculateSquare (input_number INT)
RETURNS INT
AS
BEGIN
    DECLARE result INT;
    SET result = input_number * input_number;
    RETURN result;
END;
```

Keterangan:

- proc_arameter:

`[IN | OUT | INOUT] param_name type`

- func_parameter:

`param_name type`

- Type : Semua type data yang valid di SQL.
- Routine_body:

`Statement SQL procedure yang valid.`

Jika tidak ada parameter maka empty parameter digunakan dengan menggunakan (). Setiap parameter memiliki IN parameter sebagai default. IN, OUT, INOUT parameter hanya valid untuk procedure, sedangkan untuk function hanya IN parameter saja.

RETURN type hanya berlaku untuk function.

ROUTINE_BODY berisi statement SQL yang valid. Dapat berisi statement sederhana seperti SELECT atau INSERT atau berisi gabungan beberapa statement yang dapat ditulis dengan menggunakan BEGIN .. END. Compound statement dapat berisi deklarasi, loop dan struktur kontrol yang lain

### 5.2 Untuk Menghapus Procedure atau Function

```sql
DROP {PROCEDURE|FUNCTION} [IF EXIST] name
```

Contoh:

```sql
DROP PROCEDURE abcde;
```

### 5.3 Untuk Memanggil Procedure atau Function

```sql
CALL name
```

Contoh:

```sql
CALL abcde();
```

atau

```sql
EXEC abcde;
```

### 5.4 Untuk Melihat List Procedure atau Function

```sql
SHOW {PROCEDURE|FUNCTION} status;
```

Contoh:

```sql
SHOW PROCEDURE status;
```

atau

```sql
SHOW FUNCTION status;
```

## 6. Demo

Tabel dan record yang akan digunakan

```sql
CREATE TABLE `jurusan` (
  `id` int(11) NOT NULL,
  `kode_jurusan` varchar(2) NOT NULL,
  `nama_jurusan` varchar(50) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

INSERT INTO `jurusan` (`id`, `kode_jurusan`, `nama_jurusan`) VALUES
(1, '01', 'Informatika'),
(2, '02', 'Kimia'),
(3, '03', 'Biologi');

CREATE TABLE `mahasiswa` (
  `id` int(11) NOT NULL,
  `nama` varchar(30) NOT NULL,
  `kode_jurusan` varchar(2) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

INSERT INTO `mahasiswa` (`id`, `nama`, `kode_jurusan`) VALUES
(1, 'Andi', '01'),
(2, 'Bob', '01'),
(3, 'Caca', '02'),
(4, 'David', '02'),
(5, 'Erika', '03');
```

### 6.1 Uji Coba Procedure

#### 6.1.1 Procedure Parameter IN

```sql
CREATE PROCEDURE sp_jurusan_mhs(kdjurusan char(2))
SELECT m.nama, m.kode_jurusan, j.nama_jurusan
FROM mahasiswa m
LEFT JOIN jurusan j
ON j.kode_jurusan = m.kode_jurusan
WHERE m.kode_jurusan = kdjurusan;
```

Memanggil Procedure

```sql
CALL sp_jurusan_mhs('01')
```

#### 6.1.2 Procedure Parameter OUT

```sql
CREATE PROCEDURE sp_sum_mhs (OUT sum int(11))
SELECT count(*) INTO sum FROM mahasiswa;
```

Memanggil Procedure

```sql
CALL sp_sum_mhs(@n);
SELECT @n;
```

#### 6.1.3 Procedure Parameter INOUT

```sql
CREATE PROCEDURE sp_telp (INOUT telp varchar(20))
SELECT CONCAT("(",left(telp,3),") ",substring(telp,4,3),"-",substring(telp,7))
INTO telp
```

Memanggil Procedure:

```sql
SET @telp = '0211234567';
CALL sp_telp(@telp);
SELECT @telp;
```

#### 6.1.4 Procedure Parameter IN dan OUT

```sql
CREATE PROCEDURE sp_sum_jurusan(IN kdjurusan char(2), OUT sum int)
SELECT count(*) INTO sum FROM mahasiswa
WHERE kode_jurusan = kdjurusan;
```

Memanggil Procedure:

```sql
CALL sp_sum_jurusan('01', @n);
SELECT @n;
```

#### 6.1.5 Procedure Compound Statement

```sql
DELIMITER $$
CREATE PROCEDURE sp_ganti_jurusan (kd varchar(2), nm varchar(20))
BEGIN
    IF(EXISTS(SELECT kode_jurusan FROM jurusan WHERE kode_jurusan = kd))
    THEN
    	UPDATE jurusan SET nama_jurusan = nm WHERE kode_jurusan = kd;
    ELSE
    	INSERT INTO jurusan (kode_jurusan,nama_jurusan) VALUES (kd,nm);
    END IF;
END;
$$
DELIMITER ;
```

Memanggil Procedure:

```sql
CALL sp_ganti_jurusan('01', 'Statistika')
```

### 6.2 Uji Coba Function

```sql
DELIMITER $$
CREATE FUNCTION f_sum_mhs (kdjurusan char(2))
RETURNS int
BEGIN
    DECLARE sum int;
    SELECT COUNT(*) INTO sum FROM mahasiswa
    	where kode_jurusan = kdjurusan;
    RETURN sum;
END;
$$
DELIMITER ;
```

Memanggil Function:

```sql
SELECT kode_jurusan, nama_jurusan, f_sum_mhs(kode_jurusan) FROM jurusan
```
