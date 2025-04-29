# Module 4
## Daftar Isi
- [DDL](#ddl)
- [DML](#dml-data-manipulation-language)
  - [INSERT](#1-menambah-data-baru-insert)
  - [UPDATE](#2-modifikasi-data-update)
  - [DELETE](#3-menghapus-data-delete)
  - [SELECT](#4-menampilkan-data-select)

## Introduction
![image](https://github.com/user-attachments/assets/10eb221c-3239-46e3-b52b-fef60aa8d7a4)

## DDL (Data Definition Language)
## ALTER TABLE
_Statement_ ```ALTER TABLE``` digunakan untuk menambah, menghapus, dan memodifikasi kolom yang ada pada sebuah tabel.
```sql
ALTER TABLE table_name
{ADD|MODIFY|DROP|CHANGE} (column/data_type);
```
### A. Menambah Kolom (ADD)
Command ini digunakan ketika ingin menambahkan kolom baru pada sebuah tabel yang sudah tersedia.
```sql
ALTER TABLE table_name
ADD column data_type;
```

```sql
ALTER TABLE Customers
ADD Email varchar(255);
```

### B. Mengubah Tipe Data Kolom (MODIFY)
MODIFY digunakan ketika ingin mengubah tipe data dari sebuah kolom yang sudah tersedia.
```sql
ALTER TABLE table_name
MODIFY COLUMN column new_data_type;
```
```sql
ALTER TABLE Customers
MODIFY COLUMN username char;
```
### C. Menghapus Kolom (DROP)
Command ini digunakan ketika ingin menghapus sebuah kolom dalam tabel yang sudah tersedia.
```sql
ALTER TABLE table_name
DROP COLUMN column;
```
```sql
ALTER TABLE Customers
DROP COLUMN Email;
```
### D. Menghapus Tabel (DROP)
Command DROP juga dapat digunakan jika ingin menghapus sebuah tabel. <br>
```sql
DROP TABLE table_name;
```
```sql
DROP TABLE customers;
```
![image](https://github.com/user-attachments/assets/c9f1cf47-ea19-470e-9456-830cee4a1e53)

### E. Mengubah Nama Kolom (CHANGE)
Command ini digunakan untuk mengubah atau rename nama kolom yang sudah tersedia.
```sql
ALTER TABLE table_name
CHANGE COLUMN column_name new_column_name definition/data_type
```

```sql
ALTER TABLE Customers
CHANGE COLUMN email cust_email VARCHAR(255)
```

### F. Mengubah Nama Tabel (RENAME)
Command ini digunakan ketika ingin mengubah nama dari sebuah tabel.
```sql
ALTER TABLE table_name RENAME TO new_table_name;
```
```sql
ALTER TABLE customers RENAME TO pelanggan;
```

## TRUNCATE
TRUNCATE digunakan ketika ingin menghapus semua data yang ada di tabel tertentu tanpa menghapus tabel nya itu sendiri.
```sql
TRUNCATE TABLE table_name
```
```sql
TRUNCATE TABLE Customers
```


## DML (Data Manipulation Language)
### 1. Menambah Data Baru (INSERT)
INSERT digunakan untuk menyisipkan (insert) data baru ke dalam tabel. Kalian dapat menentukan nilai-nilai yang akan dimasukkan ke dalam kolom-kolom yang sesuai.

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

Contoh Penggunaan :
```sql
INSERT INTO anggota (ID_Anggota, Nama_Anggota, No_Hp, Email)
VALUES ('1', 'Haechan', '083134540788', 'aechaniee@gmail.com');
```
![INSERT 1](https://github.com/user-attachments/assets/c7b2be9c-b4d0-487c-9ec0-6072cf8a8cc1)

Ketika kalian ingin memasukkan semua data pada kolom tanpa terkecuali, langsung saja tidak perlu di define kolomnya tidak apa asalkan data yang kamu masukkan juga urut dengan urutan kolom

```sql
INSERT INTO anggota
VALUES ('2', 'Jeno', '083154641190', 'nonoo@gmail.com');
```
![INSERT 2](https://github.com/user-attachments/assets/ff0bc3d2-962b-4743-8669-2cfb608c97ba)

### 2. Modifikasi Data (UPDATE)
UPDATE digunakan untuk memperbarui data yang ada dalam tabel. Kalian dapat mengubah nilai-nilai kolom yang ada berdasarkan kondisi yang ditentukan.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

Contoh Penggunaan :
```sql
UPDATE anggota
SET Nama_Anggota = 'Jaemin', Email = 'nana@gmail.com'
WHERE ID_Anggota = 2;
```
![UPDATE 1](https://github.com/user-attachments/assets/759b99a5-9b44-48c1-9f37-86ddb8baa2da)

⚠️ Jika kalian menggunakan syntax UPDATE jangan lupa menambahkan **WHERE**, karena jika kalian lupa maka data akan terupdate di semua baris 

### 3. Menghapus Data (DELETE)
DELETE digunakan untuk menghapus data dari tabel. Kalian dapat menentukan kondisi untuk membatasi data mana yang akan dihapus.

```sql
DELETE FROM table_name  
WHERE condition;
```

Contoh Penggunaan :
- Menghapus baris tertentu sesuai kondisi yang ditetapkan
  ```sql
  DELETE FROM anggota
  WHERE ID_Anggota = 2;
  ```
  ![DELETE 1](https://github.com/user-attachments/assets/51de7755-0fee-4756-9810-3790d8c59bb5)

- Menghapus semua baris pada tabel
  ```sql
  DELETE FROM anggota;
  ```
  ![DELETE 2](https://github.com/user-attachments/assets/a52a9789-175b-44b2-8cb5-5992f71d4545)

### 4. Menampilkan Data (SELECT)
SELECT digunakan untuk mengambil data dari satu atau lebih tabel di dalam database. Kalian dapat menggunakan SELECT untuk menentukan kolom yang ingin ditampilkan.

- Menampilkan seluruh data di semua kolom tabel
  ```sql
  SELECT * FROM table_name; 
  ```
  Contoh Penggunaan :
  ```sql
  SELECT * FROM anggota;
  ```
  ![SELECT 1](https://github.com/user-attachments/assets/ba9a96f7-84b5-483d-9b0b-9e709f0102e3)

- Menampilkan kolom-kolom tertentu
  ```sql
  SELECT column1, column2, ... 
  FROM table_name;
  ```
  Contoh Penggunaan :
  ```sql
  SELECT Nama_Anggota, No_Hp
  FROM anggota;
  ```
  ![SELECT 2](https://github.com/user-attachments/assets/73df9109-7c0c-4ebc-9815-413c76f2b1ad)

- Menampilkan data atau baris-baris tertentu
  ```sql
  SELECT * FROM table_name WHERE condition; 
  ```
  Contoh Penggunaan :
  ```  sql
  SELECT * FROM anggota WHERE ID_Anggota = '1'; 
  ```
  ![SELECT 3](https://github.com/user-attachments/assets/6451f136-47ad-4ba9-9242-13bc61e46ec1)

  ## Sumber 
- https://www.w3schools.com/sql/
- https://www.w3schools.com/sql/sql_alter.asp
- https://www.w3schools.com/sql/sql_insert.asp
- https://www.w3schools.com/sql/sql_update.asp
- https://www.w3schools.com/sql/sql_delete.asp
- https://www.w3schools.com/sql/sql_select.asp
- https://www.geeksforgeeks.org/sql-tutorial/
- https://www.sqltutorial.org/
- https://www.postgresql.org/docs/
- https://dev.mysql.com/doc/





