# Modul 8: Procedure dan Function

## Daftar Isi

- []()

- - -
## 1. Pengenalan Procedure dan Function dalam SQL

**Procedure** dan **function** adalah **subprogram** yang dapat dipanggil secara berulang dan disimpan dalam **MySQL**. Penggunaan dari **procedure** dan **function** sangat membantu untuk memangkas waktu ketika menulis sebuah query di MySQL, karena logika program bisa disimpan dan digunakan kembali.

Mirip dengan membuat fungsi dalam bahasa pemrograman lain, **procedure** dapat menerima **input (parameter)** dari pengguna dan kemudian beroperasi sesuai dengan parameter tersebut.

### 1.1 Jenis-jenis Procedure dan Function di MySQL

1. **Built-in Function** \
   Yaitu fungsi yang **secara otomatis tersedia** di MySQL dan bisa langsung digunakan tanpa konfigurasi tambahan. \
    Contoh:
   
   ```SQL
   Count(), Sum(), AVG()
   ```

2. **User-made Function** \
   Merupakan fungsi yang **dibuat oleh pengguna** untuk menjalankan tugas tertentu sesuai kebutuhan program.
Contoh: **Procedure** dan **Function.**

### 1.2 Mengapa Procedure dan Function Penting?

Penggunaan **procedure** dan **function** di MySQL penting karena meningkatkan efisiensi dan konsistensi dalam pengelolaan database. Dengan menyimpan logika dalam satu tempat, kita tidak perlu menulis ulang query yang sama, sehingga lebih hemat waktu dan mudah dikelola. Selain itu, struktur kode menjadi lebih rapi dan terorganisir, lebih aman karena bisa membatasi akses langsung ke data, serta memastikan hasil yang seragam di berbagai bagian sistem.

---
## 2. Perbedaan Procedure dan Function

| PROCEDURE                                                             | FUNCTION                                                                        |
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Digunakan terutama untuk menjalankan proses tertentu                  | Digunakan terutama untuk melakukan beberapa perhitungan                         |
| Tidak dapat dipanggil dalam pernyataan SELECT                         | Fungsi yang tidak berisi pernyataan DML dapat dipanggil dalam pernyataan SELECT |
| Gunakan parameter OUT untuk mengembalikan nilai                       | Gunakan RETURN untuk mengembalikan nilai                                        |
| Tidak wajib mengembalikan nilainya                                    | Wajib mengembalikan nilainya                                                    |
| RETURN hanya akan keluar dari kontrol dari subprogram                 | RETURN akan keluar dari kontrol dari subprogram dan juga mengembalikan value    |
| Tipe data yang dikembalikan tidak akan ditentukan pada saat pembuatan | Mengembalikan tipe data wajib pada saat pembuatan                               |

---
## 3. Terminologi

### 3.1 Parameter

Parameter merupakan variabel memori yang digunakan untuk menerima suatu nilai dari suatu fungsi yang memanggil parameter tadi.

Terdapat 3 mode parameter yaitu: **IN**, **OUT**, dan **INOUT**

- **IN**

Parameter ini digunakan untuk memberikan input ke subprogram. Nilainya konstan. Dalam pernyataan pemanggilan, parameter ini dapat berupa variabel atau nilai literal atau ekspresi. Secara default, parameternya adalah tipe IN.

- **OUT**

Parameter ini digunakan untuk mengambil dan mengembalikan output dari subprogram. Nilainya bisa berubah. Dalam pernyataan pemanggilan, parameter ini harus berupa variabel.

- **INOUT**

Parameter ini merupakan gabungan dari IN dan OUT. Parameter ini dapat digunakan untuk memberikan input dan juga mengambil output.

### 3.2 Return

RETURN merupakan instruksi kompiler untuk mengembalikan nilai dan mengalihkan kontrol dari subprogram ke pernyataan panggilan.

---
## 4. Syntax

### 4.1 Untuk Membuat Procedure atau Function

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

### 4.2 Untuk Menghapus Procedure atau Function

```sql
DROP {PROCEDURE|FUNCTION} [IF EXIST] name
```

Contoh:

```sql
DROP PROCEDURE abcde;
```

### 4.3 Untuk Memanggil Procedure atau Function

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

### 4.4 Untuk Melihat List Procedure atau Function

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

## 5. Demo

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

### 5.1 Uji Coba Procedure

#### 4.1.1 Procedure Parameter IN

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

#### 4.1.2 Procedure Parameter OUT

```sql
CREATE PROCEDURE sp_sum_mhs (OUT sum int(11))
SELECT count(*) INTO sum FROM mahasiswa; 
```

Memanggil Procedure

```sql
CALL sp_sum_mhs(@n);
SELECT @n; 
```

#### 4.1.3 Procedure Parameter INOUT

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

#### 4.1.4 Procedure Parameter IN dan OUT

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

#### 4.1.5 Procedure Compound Statement

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

### 4.2 Uji Coba Function

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
