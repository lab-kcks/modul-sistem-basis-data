## Daftar Isi
- [1. Pengurutan Data](#pengurutan-data)
    - [1.1 ASC (Ascending)](#1-asc)
    - [1.2 DESC (Descending)](#2-desc)
- [2. Fungsi Agregasi](#fungsi-agregasi)
    - [2.1 AVG](#1-avg-menghitung-rata-rata)
    - [2.2 COUNT](#2-count-menghitung-jumlah-baris)
    - [2.3 MAX](#3-max-nilai-maksimum-sebuah-kolom)
    - [2.4 MIN](#4-min-nilai-minimum-sebuah-kolom)
    - [2.5 SUM](#5-sum-total-nilai-nilai)
- [3. Operator Between, IN, LIKE](#operator-between-in-like)
    - [3.1 Between](#1-between)
    - [3.2 IN](#2-in)
    - [3.3 LIKE](#3-like)
- [4. Ekspresi Query](#ekspresi-query)
    - [4.1 Mengganti Nama Field Output](#1-mengganti-nama-field-output)
    - [4.2 Menambah Baris Teks Output](#2-menambah-baris-teks-output)
    - [4.3 Ekspresi Kondisi](#3-ekspresi-kondisi)
- [5. Fungsi Waktu](#fungsi-waktu)
    - [5.1 Current Time](#1-current-time)
    - [5.2 Timediff](#2-timediff)
    - [5.3 Year/ Month/ Day](#3-year-month-day)
- [6. Pengenalan JOIN dalam SQL](#1-pengenalan-join-dalam-sql)
  - [6.1 Self Join](#11-self-join)
  - [6.2 Outer Join pada Dua Tabel](#12-outer-join-pada-dua-tabel)
  - [6.3 Join Lebih dari Dua Tabel](#13-join-lebih-dari-dua-tabel)
- [7. View](#2-view)
  - [7.1 Pengenalan](#31-pengenalan-view)
  - [7.2 Sintaks](#32-sintaks-view)
  - [7.3 Contoh Implementasi](#33-contoh-implementasi-view)
  - [7.4 Pengujian View](#34-pengujian-view)
  - [7.5 Keuntungan dan Keterbatasan View](#35-keuntungan-dan-keterbatasan-view)
- [Sumber](#sumber)

## 1. Pengurutan Data
ORDER BY dalam _sql_ digunakan untuk mengurutkan hasil query pada sebuah kolom berdasarkan nilai terbesar atau terkecilnya. Untuk mengetahui diurutkan secara nilai terbesar atau terkecil, setelah klausa ORDER BY dapat ditambahkan ASCENDING ataupun DESCENDING. 
```
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC; 
```
### 1.1 ASC
Digunakan untuk mengurutkan data dari data bernilai kecil ke besar contohnya A ke Z

Contoh Penggunaan :
```
SELECT nama_produk, harga, stok
FROM produk 
ORDER BY harga ASC;
```
![image](https://github.com/user-attachments/assets/57227ad4-b0ac-4d11-96fa-6e5df6a788cb)

### 1.2 DESC
Digunakan untuk mengurutkan data dari data bernilai besar ke kecil contohnya Z ke A

Contoh Penggunaan :
```
SELECT nama_produk, kategori, stok
FROM produk 
ORDER BY stok DESC;
```
![image](https://github.com/user-attachments/assets/9cea82c6-dc3a-4a6e-a16b-e8d574ea5ed7)


## 2. Fungsi Agregasi
Fungsi Agregasi adalah sebuah fungsi yang akan menghasilkan sebuah value baru/ringkasan data dari hasil perhitungan beberapa kolom pada sebuah operasi query.
Fungsi agregasi SQL ini sebagian besar digunakan dengan **GROUP BY** dari pernyataan **SELECT**.

|Fungsi Agregasi|Deskripi Fungsi|
|:---------------|:--------|
|AVG |Menghitung nilai rata-rata|
|COUNT |Menghitung jumlah baris|
|MAX |Mengambil nilai data yang maksimum dari sebuah kolom|
|MIN |Mengambil nilai data yang minimum dari sebuah kolom|
|SUM |Menghitung jumlah total dari nilai-nilai yang ada dalam sebuah kolom numerik (angka)|

```
SELECT COUNT|SUM|AVG|MAX|MIN(column_name)
FROM table_name
WHERE condition;
```

### 2.1 AVG (menghitung rata-rata)
```
SELECT AVG(harga) AS rata_harga_makanan
FROM produk
WHERE kategori = 'makanan';
```
![image](https://github.com/user-attachments/assets/5ee73a17-12a0-4991-87f6-8c6380b21f15)

### 2.2 COUNT (menghitung jumlah baris)
```
SELECT kota, COUNT(*) AS jumlah_pelanggan
FROM pelanggan
GROUP BY kota;
```
![image](https://github.com/user-attachments/assets/b2039734-04ee-4e68-9666-761f0186394e)

### 2.3 MAX (nilai maksimum sebuah kolom)
```
SELECT MAX(harga)
FROM produk;
```
![image](https://github.com/user-attachments/assets/cf6b928c-fb0b-4fa6-936c-b6d948565df0)

### 2.4 MIN (nilai minimum sebuah kolom)
```
SELECT MIN(harga)
FROM produk;
```
![image](https://github.com/user-attachments/assets/68a02cff-1494-425c-a03e-5281f41ca207)

### 2.5 SUM (total nilai-nilai)
```
SELECT SUM(subtotal) AS total_penjualan FROM detail_transaksi;
```
![image](https://github.com/user-attachments/assets/497d54a4-7f1e-4253-9330-4297d596c231)

## 3. Operator Between, IN, LIKE
### 3.1 Between
Operator SQL BETWEEN digunakan untuk menguji apakah suatu nilai berada dalam rentang nilai tertentu berupa teks, tanggal, ataupun angka.
```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
Contoh Penggunaan :
```
SELECT * FROM produk
WHERE harga BETWEEN 20000 AND 40000;
```
![image](https://github.com/user-attachments/assets/41d519e1-d583-4e6c-a4e8-abba1c73eb59)

### 3.2 IN
Operator IN digunakan untuk memeriksa apakah suatu nilai terdapat di dalam daftar nilai yang telah ditentukan.
```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);
```
Contoh Penggunaan :
```
SELECT * FROM pelanggan
WHERE kota IN ('Surabaya', 'Jakarta');
```
![image](https://github.com/user-attachments/assets/3a305fc5-858a-40e6-af8d-41bb9be9900f)


### 3.3 LIKE
Operator LIKE merupakan salah satu operator yang dimanfaatkan untuk mencari sebuah data di dalam tabel, seperti fungsi “Search” yang berjalan dengan mencari kolom dengan pola yang spesifik. Dalam operator LIKE ini biasanya menggunakan 2 bentuk simbol yaitu **%** dan **_** . 
```
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;
```
Contoh Penggunaan:
- Simbol % digunakan sebagai wildcard dalam pencarian data yang artinya bisa mewakili tidak ada karakter, satu karakter, atau banyak karakter dalam sebuah string.
    ```
    SELECT * FROM produk
    WHERE nama_produk LIKE '%myeon%';
    ```
    ![image](https://github.com/user-attachments/assets/dc19b14f-a2ba-4476-bda0-5d30b4a5a1d5)

- Simbol _ digunakan sebagai wildcard yang mewakili tepat satu karakter apa pun dalam sebuah string
  ```
  SELECT * FROM pelanggan
  WHERE kota LIKE '____baya';
  ```
  ![image](https://github.com/user-attachments/assets/fdf949b3-1d98-47af-85a0-2e4eea6a9779)
  
## 4. Ekspresi Query
Merupakan kombinasi dari satu atau beberapa nilai, operator, dan fungsi SQL yang semuanya dievaluasi menjadi suatu nilai. 
### 4.1 Mengganti Nama Field Output
Digunakan ketika ingin menampilkan data dari sebuah tabel namun menggunakan nama (tabel) yang berbeda
```
SELECT column_name AS alias_name
FROM table_name;
```
Contoh Penggunaan:<br>
Pada kasus ini, tabel ```harga``` ingin ditampilkan namun menggunakan title ```Harga  (Rp)```
```
SELECT nama_produk AS 'Nama Produk', harga AS 'Harga (Rp)'
FROM produk;
```
![image](https://github.com/user-attachments/assets/a6a853f2-64b6-47ab-a4c9-554adb94a635)

### 4.2 Menambah Baris Teks Output
Digunakan ketika ingin menambahkan baris text tetap pada hasil query. Biasanya digunakan untuk membuat laporan atau menambahkan hasil deskripsi tetap di query.
```
SELECT 'Text' AS alias_name, column_name
FROM table_name;
```
Contoh Penggunaan:<br>
Pada kasus ini, hasil output akan menampilkan kolom keterangan berisi text tetap yaitu _'Produk ini tersedia'_
```
SELECT 
  'Produk ini tersedia' AS keterangan, 
  nama_produk 
FROM produk;
```
![image](https://github.com/user-attachments/assets/64e6d094-eacc-40ad-a0e3-82eff4a1d3bd)

### 4.3 Ekspresi Kondisi
**CASE** adalah ekspresi kondisional di SQL yang berfungsi seperti if-else. Umumnya digunakan untuk mengganti nilai berdasarkan kondisi tertentu.
```
Select Nama_Field_1 Case Nama_Field_2 When 'Nilai_field_2'
Then 'Keterangan_1' Else 'Keterangan_2'
End As Nilai_field_2 From Nama_Table;
```
Contoh Penggunaan:<br>
Dalam kasus ini, akan menampilkan sapaan 'Mas' jika jenis kelaminnya laki-laki dan 'Mbak' jika jenis kelaminnya perempuan.
```
SELECT 
  nama_pelanggan,
  CASE jenis_kelamin
    WHEN 'Laki-laki' THEN 'Mas'
    ELSE 'Mbak'
  END AS sapaan
FROM pelanggan;
```
![image](https://github.com/user-attachments/assets/6de128d5-d59d-439d-be65-2973cf795269)

## 5. Fungsi Waktu
### 5.1 Current Time
Digunakan untuk menampilkan tanggal, waktu, maupun keduanya saat ini. 
- ```NOW()``` : Mengembalikan tanggal dan waktu saat ini.
- ```CURDATE()``` : Mengembalikan tanggal saja (tanpa jam).
- ```CURTIME()``` : Mengembalikan waktu saja (tanpa tanggal).
```
SELECT CURTIME()| CURDATE()| NOW();
```
Contoh Penggunaan:
```
SELECT 
  NOW() AS 'Waktu Lengkap',
  CURDATE() AS 'Tanggal Hari Ini',
  CURTIME() AS 'Jam Saat Ini';
```
![image](https://github.com/user-attachments/assets/7283a53a-0ea3-4558-96eb-6f12b0c5c033)

### 5.2 Timediff
Digunakan untuk mmenghitung **selisih** nilai waktu antara 2 nilai waktu.<br>
Contoh Penggunaan:
```
SELECT 
  id_transaksi, 
  tanggal,
  TIMEDIFF(NOW(), tanggal) AS 'Selisih Waktu Sekarang'
FROM transaksi;
```
![image](https://github.com/user-attachments/assets/5b367fa3-af30-41ad-b710-f6a7556938a6)

### 5.3 Year/ Month/ Day
Digunakan untuk mengambil bagian dari sebuah waktu tertentu. Contoh: YEAR, MONTH, DAY.
```
SELECT YEAR(CURRENT_TIME());
```
![image](https://github.com/user-attachments/assets/de28e7e7-da31-46cd-81b2-3c7b835cd87f)

Fungsi terkait lainnya:
```
MONTH(CURRENT_DATE())
DAY(CURRENT_DATE())
```

## 6. Pengenalan JOIN dalam SQL

![image](https://github.com/user-attachments/assets/12c166d6-6c5c-4290-bd7f-eee39ae8127e)

`JOIN` merupakan salah satu perintah penting dalam bahasa SQL (Structured Query Language) yang digunakan untuk menggabungkan data dari dua tabel atau lebih dengan memanfaatkan kolom yang memiliki hubungan logis di antara tabel-tabel tersebut. Dengan menggunakan perintah `JOIN`, kita dapat menyatukan data yang sebelumnya tersebar di berbagai tabel menjadi satu tampilan yang utuh, sehingga mempermudah dalam melakukan analisis atau pelaporan yang membutuhkan data dari banyak sumber.

#### Lalu mengapa JOIN ini diperlukan? 
Melalui mekanisme ini, kita dapat mengakses informasi yang lebih lengkap dan bermakna karena hubungan antar data menjadi lebih terlihat. `JOIN` sangat berguna dalam konteks sistem basis data relasional, di mana data biasanya disimpan dalam bentuk tabel-tabel terpisah yang saling terhubung melalui relasi tertentu seperti foreign key.

Secara umum, JOIN dalam SQL dibagi menjadi dua kelompok besar berdasarkan sifat penggabungannya:

- Self Join: Merupakan jenis join yang dilakukan pada tabel itu sendiri, yaitu ketika sebuah tabel digabungkan dengan salinan dirinya sendiri. Ini biasanya digunakan untuk membandingkan baris antar entri dalam satu tabel, misalnya untuk menemukan data yang memiliki atribut serupa.

- Outer Join: Digunakan untuk menggabungkan dua atau lebih tabel yang berbeda, termasuk baris-baris data yang mungkin tidak memiliki pasangan yang cocok di tabel lainnya. Outer Join sendiri terdiri dari beberapa variasi seperti Left Join, Right Join, dan Full Outer Join, yang masing-masing memiliki cara kerja dan hasil penggabungan yang berbeda tergantung kebutuhan.

---

### 6.1 Self Join

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

### 6.2 Outer Join pada Dua Tabel

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

#### 6.2.1 Inner Join

```sql
SELECT *
FROM mahasiswa
INNER JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Inner JOIN Menampilkan data mahasiswa yang memiliki relasi transaksi, dan hanya baris yang cocok pada kedua tabel yang akan ditampilkan. In kalkulus terms, intinya Inner JOIN itu menampilkan irisan dari kedua tabel tersebut. 

![image](https://github.com/user-attachments/assets/ededd437-1980-42d6-b6fa-1d945c4da599)

---

#### 6.2.2 Left Join

```sql
SELECT *
FROM mahasiswa
LEFT JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Menampilkan semua mahasiswa beserta transaksinya, jika ada. Jika tidak ada, maka kolom dari tabel `transaksi` akan berisi `NULL`.

![image](https://github.com/user-attachments/assets/895393af-7e96-4eee-a283-54eb3a1ca804)

---

#### 6.2.3 Right Join

```sql
SELECT *
FROM mahasiswa
RIGHT JOIN transaksi
ON mahasiswa.nim = transaksi.nim;
```

Menampilkan semua data dari `transaksi`, dan akan melampirkan data mahasiswa jika ditemukan pasangan yang cocok.

![image](https://github.com/user-attachments/assets/3f148eef-bb9f-456c-8951-e946fba6c307)

---

#### 6.2.4 Full Outer Join

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

### 6.3 Join Lebih dari Dua Tabel

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

## 7. View

### 7.1 Pengenalan View

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

### 7.2 Sintaks View

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

### 7.3 Contoh Implementasi View

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

### 7.4 Pengujian View

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

### 7.5 Keuntungan dan Keterbatasan View

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

## Sumber
- https://www.geeksforgeeks.org/sql-order-by/?ref=gcse
- https://www.geeksforgeeks.org/sql-logical-operators/?ref=gcse
- https://www.geeksforgeeks.org/aggregate-functions-in-sql/?ref=gcse
- https://www.w3schools.com/sql/func_mysql_current_time.asp
- https://www.w3schools.com/sql/sql_case.asp
- https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_timediff
- https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_year
- https://www.nesabamedia.com/pengertian-trigger/
- https://www.geeksforgeeks.org/different-types-of-mysql-triggers-with-examples/
- https://www.tutorialspoint.com/mysql/mysql-triggers.htm
- [SQL Views (geeksforgeeks.org)](https://www.geeksforgeeks.org/sql-views/?ref=gcse)
- [SQL Tutorial - GeeksforGeeks](https://www.geeksforgeeks.org/sql-tutorial/?ref=header_search)
- [SQL: Triggers and Views](https://dev.to/spo0q/sql-triggers-views-2abk)
- [SQL Triggers: A Guide for Developers](https://www.datacamp.com/tutorial/sql-triggers)
