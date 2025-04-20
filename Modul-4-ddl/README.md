# Module 4
## Daftar Isi

## Introduction
![image](https://github.com/user-attachments/assets/10eb221c-3239-46e3-b52b-fef60aa8d7a4)


## ALTER TABLE (DDL)
_Statement_ ```ALTER TABLE``` digunakan untuk menambah, menghapus, dan memodifikasi kolom yang ada pada sebuah tabel.
```
ALTER TABLE table_name
{ADD|MODIFY|DROP|CHANGE} (column/data_type);
```
### Menambah Kolom (ADD)
```
ALTER TABLE table_name
ADD column data_type;
```

```
ALTER TABLE Customers
ADD Email varchar(255);
```

### Mengubah Tipe Data Kolom (MODIFY)
```
ALTER TABLE table_name
MODIFY COLUMN column new_data_type;
```
```
ALTER TABLE Customers
MODIFY COLUMN username char;
```
### Menghapus Kolom (DROP)
```
ALTER TABLE table_name
DROP COLUMN column;
```
```
ALTER TABLE Customers
DROP COLUMN Email;
```
### Mengubah Nama Kolom (CHANGE)
```
ALTER TABLE table_name
CHANGE COLUMN table_name new_table_name definition/data_type
```

```
ALTER TABLE Customers
CHANGE COLUMN email cust_email VARCHAR(255)
```

## DML
### Menambah Data Baru (INSERT)
### Modifikasi Data (UPDATE)
### Menghapus Data (DELETE)
### Menampilkan Data (SELECT)
