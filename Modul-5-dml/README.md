## Daftar Isi
- [Pengurutan Data](#pengurutan-data)
    - [ASC (Ascending)](#asc)
    - [DESC (Descending)](#desc)
- [Fungsi Agregasi](#fungsi-agregasi)
    - [AVG](#avg-menghitung-rata-rata)
    - [COUNT](#count-menghitung-jumlah-baris)
    - [MAX](#max-nilai-maksimum-sebuah-kolom)
    - [MIN](#min-nilai-minimum-sebuah-kolom)
    - [SUM](#sum-total-nilai-nilai)
- [Operator Between, IN, LIKE](#operator-between-in-like)
    - [Operator Between](#operator-between)
    - [Operator IN](#operator-in)
    - [Operator LIKE](#operator-like)
- [Ekspresi Query](#ekspresi-query)
- [Fungsi Waktu](#fungsi-waktu)
- [Referensi](#referensi)

## Pengurutan Data
ORDER BY dalam _sql_ digunakan untuk mengurutkan hasil query pada sebuah kolom berdasarkan nilai terbesar atau terkecilnya. Untuk mengetahui diurutkan secara nilai terbesar atau terkecil, setelah klausa ORDER BY dapat ditambahkan ASCENDING ataupun DESCENDING. 
```
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC; 
```
### ASC
Digunakan untuk mengurutkan data dari data bernilai kecil ke besar contohnya A ke Z

Contoh Penggunaan :
```
SELECT nama_produk, harga, stok
FROM produk 
ORDER BY harga ASC;
```
![image](https://github.com/user-attachments/assets/57227ad4-b0ac-4d11-96fa-6e5df6a788cb)

### DESC
Digunakan untuk mengurutkan data dari data bernilai besar ke kecil contohnya Z ke A

Contoh Penggunaan :
```
SELECT nama_produk, kategori, stok
FROM produk 
ORDER BY stok DESC;
```
![image](https://github.com/user-attachments/assets/9cea82c6-dc3a-4a6e-a16b-e8d574ea5ed7)


## Fungsi Agregasi
Fungsi Agregasi adalah sebuah fungsi yang akan menghasilkan sebuah value baru/ringkasang data dari hasil perhitungan beberapa kolom pada sebuah operasi query.
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

### AVG (menghitung rata-rata)
```
SELECT AVG(harga) AS rata_harga_makanan
FROM produk
WHERE kategori = 'makanan';
```
![image](https://github.com/user-attachments/assets/5ee73a17-12a0-4991-87f6-8c6380b21f15)

### COUNT (menghitung jumlah baris)
```
SELECT kota, COUNT(*) AS jumlah_pelanggan
FROM pelanggan
GROUP BY kota;
```
![image](https://github.com/user-attachments/assets/b2039734-04ee-4e68-9666-761f0186394e)

### MAX (nilai maksimum sebuah kolom)
```
SELECT MAX(harga)
FROM produk;
```
![image](https://github.com/user-attachments/assets/cf6b928c-fb0b-4fa6-936c-b6d948565df0)

### MIN (nilai minimum sebuah kolom)
```
SELECT MIN(harga)
FROM produk;
```
![image](https://github.com/user-attachments/assets/68a02cff-1494-425c-a03e-5281f41ca207)

### SUM (total nilai-nilai)
```
SELECT SUM(subtotal) AS total_penjualan FROM detail_transaksi;
```
![image](https://github.com/user-attachments/assets/497d54a4-7f1e-4253-9330-4297d596c231)

## Operator Between, IN, LIKE
### Operator Between
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

### Operator IN
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
![image](https://github.com/user-attachments/assets/f80414f6-5dff-4991-8555-c643a282447d)

### Operator LIKE
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
  
## Ekspresi Query

## Fungsi Waktu

### Referensi
- https://www.revou.co/panduan-teknis/sql-sum
- 
