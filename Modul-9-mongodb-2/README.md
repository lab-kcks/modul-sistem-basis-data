
## Update & Delete dalam MongoDB
### Operasi Update

Operasi update digunakan untuk memodifikasi dokumen yang sudah ada dalam sebuah *collection*. MongoDB menyediakan beberapa metode untuk melakukan pembaruan data, yang memungkinkan pembaruan pada satu dokumen maupun banyak dokumen sekaligus, serta pembaruan field spesifik atau penggantian keseluruhan dokumen.

1.  **Metode `updateOne()` di MongoDB**

    Metode `updateOne()` digunakan untuk memperbarui **satu dokumen pertama** yang cocok dengan kriteria filter yang diberikan.

    **Sintaks Dasar:**
    ```javascript
    db.nama_koleksi_anda.updateOne(
       <filter_kueri>,       // Kriteria untuk memilih dokumen yang akan diupdate
       <dokumen_update>,     // Spesifikasi pembaruan yang akan diterapkan
       {
         upsert: <boolean>    // Opsional: Jika true, buat dokumen baru jika tidak ada yang cocok (default: false)
         // ... opsi lainnya
       }
    );
    ```

    -   `<filter_kueri>`: Sama seperti kriteria pada `find()`, menentukan dokumen mana yang akan diupdate.
    -   `<dokumen_update>`: Menggunakan operator update (misalnya `$set`, `$inc`, `$unset`) untuk mendefinisikan perubahan.
        -   `$set`: Mengatur nilai sebuah field. Jika field tidak ada, akan dibuat.
        -   `$inc`: Menambah atau mengurangi nilai sebuah field numerik.
        -   `$unset`: Menghapus field tertentu dari dokumen.

    **Contoh Skenario:**
    Misalkan kita memiliki koleksi `karyawan` dan ingin mengupdate gaji karyawan dengan `nama: "Budi"` menjadi `5500000`.

    -   **Data Input (Contoh di koleksi `karyawan` sebelum update):**
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5000000 },
          { "_id": 2, "nama": "Ani", "departemen": "HR", "gaji": 6000000 }
        ]
        ```

    -   **Perintah Update:**
        ```javascript
        db.karyawan.updateOne(
          { nama: "Budi" },
          { $set: { gaji: 5500000, status: "Aktif" } } // Mengubah gaji dan menambah field status
        );
        ```

    -   **Hasil (Dokumen Budi akan terupdate):**
        Koleksi `karyawan` akan menjadi:
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5500000, "status": "Aktif" },
          { "_id": 2, "nama": "Ani", "departemen": "HR", "gaji": 6000000 }
        ]
        ```
        Output dari perintah `updateOne` biasanya berisi informasi seperti `matchedCount` dan `modifiedCount`.

    -   **Contoh dengan `upsert`:**
        Jika kita ingin mengupdate karyawan bernama "Citra" dan jika tidak ada, maka buat dokumen baru:
        ```javascript
        db.karyawan.updateOne(
          { nama: "Citra" },
          { $set: { departemen: "Marketing", gaji: 4000000 } },
          { upsert: true }
        );
        ```
        Jika "Citra" tidak ditemukan, dokumen baru akan ditambahkan.

2.  **Metode `updateMany()` di MongoDB**

    Metode `updateMany()` digunakan untuk memperbarui **semua dokumen** yang cocok dengan kriteria filter yang diberikan.

    **Sintaks Dasar:**
    ```javascript
    db.nama_koleksi_anda.updateMany(
       <filter_kueri>,
       <dokumen_update>,
       {
         upsert: <boolean> // Opsional
         // ... opsi lainnya
       }
    );
    ```

    **Contoh Skenario:**
    Menaikkan gaji semua karyawan di departemen "IT" sebesar 10%.

    -   **Data Input (Contoh di koleksi `karyawan` sebelum update):**
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5000000 },
          { "_id": 2, "nama": "Ani", "departemen": "HR", "gaji": 6000000 },
          { "_id": 3, "nama": "Dedi", "departemen": "IT", "gaji": 4500000 }
        ]
        ```

    -   **Perintah Update:**
        ```javascript
        db.karyawan.updateMany(
          { departemen: "IT" },
          { $mul: { gaji: 1.10 } } // Menggunakan $mul untuk perkalian (menaikkan 10%)
        );
        ```
        Operator `$mul` mengalikan nilai field dengan angka yang diberikan.

    -   **Hasil (Dokumen Budi dan Dedi akan terupdate):**
        Koleksi `karyawan` akan menjadi:
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5500000 },
          { "_id": 2, "nama": "Ani", "departemen": "HR", "gaji": 6000000 },
          { "_id": 3, "nama": "Dedi", "departemen": "IT", "gaji": 4950000 }
        ]
        ```

3.  **Metode `replaceOne()` di MongoDB**

    Metode `replaceOne()` digunakan untuk **mengganti keseluruhan satu dokumen pertama** yang cocok dengan kriteria filter, kecuali untuk field `_id` (yang tidak dapat diubah). Jika dokumen pengganti tidak mengandung field `_id` dari dokumen asli, maka `_id` akan tetap dipertahankan.

    **Sintaks Dasar:**
    ```javascript
    db.nama_koleksi_anda.replaceOne(
       <filter_kueri>,
       <dokumen_pengganti>,
       {
         upsert: <boolean> // Opsional
         // ... opsi lainnya
       }
    );
    ```
    Penting untuk dicatat bahwa `<dokumen_pengganti>` **tidak boleh** mengandung operator update (seperti `$set`). Ini adalah dokumen pengganti secara utuh.

    **Contoh Skenario:**
    Mengganti data karyawan dengan `nama: "Ani"` dengan data yang sepenuhnya baru.

    -   **Data Input (Contoh di koleksi `karyawan` sebelum replace):**
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5500000 },
          { "_id": 2, "nama": "Ani", "departemen": "HR", "gaji": 6000000, "alamat": "Jl. Merdeka 10" }
        ]
        ```

    -   **Perintah Replace:**
        ```javascript
        db.karyawan.replaceOne(
          { nama: "Ani" },
          { nama: "Ani Suryani", departemen: "Human Resources", gaji: 6200000, kontak: "0812345678" }
        );
        ```

    -   **Hasil (Dokumen Ani akan tergantikan):**
        Koleksi `karyawan` akan menjadi:
        ```json
        [
          { "_id": 1, "nama": "Budi", "departemen": "IT", "gaji": 5500000 },
          { "_id": 2, "nama": "Ani Suryani", "departemen": "Human Resources", "gaji": 6200000, "kontak": "0812345678" } // Field alamat hilang
        ]
        ```
        Field `_id: 2` akan tetap ada, tetapi field `alamat` yang ada sebelumnya akan hilang karena tidak disertakan dalam dokumen pengganti.

**Operator Update yang Umum Digunakan:**

Selain `$set`, `$inc`, `$mul`, dan `$unset` yang telah disebutkan, beberapa operator update lain yang sering digunakan meliputi:

-   `$rename`: Mengubah nama sebuah field.
-   `$min`: Memperbarui field hanya jika nilai yang diberikan lebih kecil dari nilai saat ini.
-   `$max`: Memperbarui field hanya jika nilai yang diberikan lebih besar dari nilai saat ini.
-   `$currentDate`: Mengatur nilai field ke tanggal saat ini (sebagai Date atau Timestamp).
-   Untuk Array: `$push` (menambah elemen), `$pop` (menghapus elemen pertama/terakhir), `$pull` (menghapus elemen berdasarkan kondisi), `$addToSet` (menambah elemen jika belum ada).

### Operasi Delete

Operasi delete digunakan untuk menghapus dokumen dari sebuah *collection*. MongoDB menyediakan metode untuk menghapus satu dokumen atau banyak dokumen sekaligus berdasarkan kriteria filter.

**Peringatan:** Operasi delete bersifat permanen. Setelah dokumen dihapus, data tersebut tidak dapat dipulihkan kecuali jika ada mekanisme backup sebelumnya. Selalu pastikan kriteria filter sudah benar sebelum menjalankan perintah delete.

1.  **Metode `deleteOne()` di MongoDB**

    Metode `deleteOne()` digunakan untuk menghapus **satu dokumen pertama** yang cocok dengan kriteria filter yang diberikan.

    **Sintaks Dasar:**
    ```javascript
    db.nama_koleksi_anda.deleteOne(<filter_kueri>);
    ```

    **Contoh Skenario:**
    Menghapus produk dengan `nama: "Produk C"` dari koleksi `inventaris`.

    -   **Data Input (Contoh di koleksi `inventaris` sebelum delete):**
        ```json
        [
          { "_id": "P001", "nama": "Produk A", "jumlah": 100 },
          { "_id": "P002", "nama": "Produk B", "jumlah": 150 },
          { "_id": "P003", "nama": "Produk C", "jumlah": 0 }
        ]
        ```

    -   **Perintah Delete:**
        ```javascript
        db.inventaris.deleteOne({ nama: "Produk C" });
        ```

    -   **Hasil (Dokumen "Produk C" akan terhapus):**
        Koleksi `inventaris` akan menjadi:
        ```json
        [
          { "_id": "P001", "nama": "Produk A", "jumlah": 100 },
          { "_id": "P002", "nama": "Produk B", "jumlah": 150 }
        ]
        ```
        Output dari perintah `deleteOne` biasanya berisi informasi seperti `deletedCount`.

2.  **Metode `deleteMany()` di MongoDB**

    Metode `deleteMany()` digunakan untuk menghapus **semua dokumen** yang cocok dengan kriteria filter yang diberikan.

    **Sintaks Dasar:**
    ```javascript
    db.nama_koleksi_anda.deleteMany(<filter_kueri>);
    ```

    **Contoh Skenario:**
    Menghapus semua produk dari koleksi `inventaris` yang `jumlah`-nya adalah `0`.

    -   **Data Input (Contoh di koleksi `inventaris` sebelum delete):**
        ```json
        [
          { "_id": "P001", "nama": "Produk A", "jumlah": 100 },
          { "_id": "P002", "nama": "Produk B", "jumlah": 0 },
          { "_id": "P003", "nama": "Produk C", "jumlah": 0 },
          { "_id": "P004", "nama": "Produk D", "jumlah": 50 }
        ]
        ```

    -   **Perintah Delete:**
        ```javascript
        db.inventaris.deleteMany({ jumlah: 0 });
        ```

    -   **Hasil (Dokumen "Produk B" dan "Produk C" akan terhapus):**
        Koleksi `inventaris` akan menjadi:
        ```json
        [
          { "_id": "P001", "nama": "Produk A", "jumlah": 100 },
          { "_id": "P004", "nama": "Produk D", "jumlah": 50 }
        ]
        ```

    Untuk menghapus semua dokumen dalam sebuah koleksi, kalian dapat memberikan filter kosong `{}` pada `deleteMany()`:
    ```javascript
    db.nama_koleksi_anda.deleteMany({}); // Akan menghapus semua dokumen di koleksi
    ```
    Berhati-hatilah saat menggunakan perintah ini.

---

## Agregasi 
Agregasi dalam MongoDB dapat dianalogikan sebagai sebuah alur pemrosesan data. Data mentah dari koleksi kalian dimasukkan ke dalam alur tersebut, kemudian diproses melalui serangkaian tahapan (stages), di mana setiap tahapan memiliki fungsi spesifik. Hasilnya adalah data yang telah diringkas, dikelompokkan, dihitung, atau ditransformasi bentuknya sesuai dengan kebutuhan analisis. Fokus utama tetap pada aspek membaca dan menganalisis data, meskipun beberapa tahapan dapat menghasilkan data baru.

### Konsep Umum Agregasi MongoDB

**Penjelasan:**
Agregasi bekerja menggunakan konsep _pipeline_ (saluran pipa). Dokumen-dokumen dari koleksi kalian akan memasuki salah satu ujung pipa, kemudian di dalam pipa tersebut terdapat beberapa stasiun pemrosesan (tahapan/stages). Setiap dokumen akan melewati stasiun-stasiun ini secara berurutan. Pada setiap stasiun, dokumen dapat diubah, difilter, dikelompokkan, atau dihitung. Output dari satu stasiun menjadi input untuk stasiun berikutnya. Pada akhirnya, di ujung pipa lainnya, akan dihasilkan data yang telah terolah.

**Cara Menjalankan Pipeline Agregasi:**
Pipeline agregasi dijalankan menggunakan perintah `aggregate()` pada sebuah koleksi. Perintah ini menerima sebuah array yang berisi definisi dari tahapan-tahapan pipeline.

**Sintaks Dasar:**

```javascript
db.nama_koleksi_anda.aggregate([
  {
    $tahapPertama: {
      /* parameter tahap pertama */
    }
  },
  {
    $tahapKedua: {
      /* parameter tahap kedua */
    }
  }
  // ... dan seterusnya
]);
```

---

### Tahapan (Stages) dalam Pipeline Agregasi

**Penjelasan:**
Ini adalah unit-unit atau stasiun pemrosesan dalam pipeline kalian. Terdapat beragam tahapan yang dapat digunakan, beberapa yang paling umum meliputi:

-   **`$match`**: Berfungsi sebagai filter. Hanya dokumen yang memenuhi kriteria yang ditentukan yang akan diteruskan ke tahap berikutnya. Serupa dengan operasi `find()`.
-   **`$group`**: Mengelompokkan dokumen berdasarkan nilai field tertentu. Pada tahap ini, dapat dilakukan kalkulasi seperti total, rata-rata, nilai maksimum/minimum, dan lainnya untuk setiap grup.
-   **`$project`**: Mengubah struktur dokumen. Dapat digunakan untuk memilih field yang akan ditampilkan, menambah field baru, atau mengubah nama field yang sudah ada.
-   **`$sort`**: Mengurutkan dokumen berdasarkan field tertentu, baik secara menaik (ascending) maupun menurun (descending).
-   **`$limit`**: Membatasi jumlah dokumen yang diteruskan ke tahap selanjutnya sebanyak N dokumen pertama.
-   **`$skip`**: Melewatkan N dokumen pertama; pemrosesan dilanjutkan mulai dari dokumen ke-N+1.
-   **`$unwind`**: Memecah dokumen yang memiliki field berupa array menjadi beberapa dokumen, satu dokumen untuk setiap elemen dalam array tersebut.
-   **`$lookup`**: Melakukan penggabungan data dari koleksi lain, serupa dengan operasi `JOIN` pada SQL.
-   **`$count`**: Menghitung jumlah dokumen yang masuk ke tahap ini dan menghasilkan satu dokumen output yang berisi field dengan nilai jumlah tersebut.
-   **`$out`**: Menyimpan hasil akhir pipeline ke dalam sebuah koleksi baru.

**Contoh Skenario Singkat (Menggabungkan beberapa tahap):**
Misalkan terdapat koleksi `produk` dan akan dicari produk dengan kategori "Buah" yang harganya di atas 3000, diurutkan dari yang paling mahal, dan hanya menampilkan nama serta harganya.

-   **Data Input (Contoh pada koleksi `produk`):**

```json
[
  { "nama": "Apel Fuji", "kategori": "Buah", "harga": 5000, "stok": 50 },
  { "nama": "Mangga Harum Manis", "kategori": "Buah", "harga": 7000, "stok": 30 },
  { "nama": "Bayam Segar", "kategori": "Sayur", "harga": 2000, "stok": 100 },
  { "nama": "Jeruk Sunkist", "kategori": "Buah", "harga": 6000, "stok": 40 }
]
```

-   **Perintah Agregasi:**

```javascript
db.produk.aggregate([
  { $match: { kategori: "Buah", harga: { $gt: 3000 } } }, // Filter produk: kategori "Buah" dan harga > 3000
  { $sort: { harga: -1 } }, // Urutkan berdasarkan harga secara descending
  { $project: { _id: 0, namaProduk: "$nama", hargaProduk: "$harga" } } // Pilih dan ubah nama field
]);
```

-   **Hasil Output:**

```json
[
  { "namaProduk": "Mangga Harum Manis", "hargaProduk": 7000 },
  { "namaProduk": "Jeruk Sunkist", "hargaProduk": 6000 },
  { "namaProduk": "Apel Fuji", "hargaProduk": 5000 }
]
```

---

### Optimasi Pipeline Agregasi

**Penjelasan:**
Untuk memastikan pipeline agregasi berjalan secara optimal dan efisien dalam penggunaan sumber daya, beberapa praktik terbaik dapat diterapkan:

1.  **Filter di Awal**: Gunakan `$match` pada tahap sedini mungkin dalam pipeline. Semakin sedikit dokumen yang diproses pada tahap-tahap selanjutnya, semakin cepat eksekusi pipeline.
2.  **Pemanfaatan Indeks**: Jika `$match` atau `$sort` menggunakan field yang memiliki indeks, MongoDB dapat memanfaatkan indeks tersebut untuk mempercepat proses pencarian dan pengurutan.
3.  **Proyeksi Dini**: Gunakan `$project` untuk menghilangkan field-field yang tidak diperlukan sesegera mungkin. Hal ini mengurangi ukuran dokumen yang harus diolah pada tahap berikutnya.
4.  **Urutan Tahap**: Pertimbangkan urutan tahapan yang paling efisien. Sebagai contoh, mungkin lebih baik melakukan `$project` sebelum `$unwind` jika field array berukuran sangat besar namun hanya beberapa elemen yang dibutuhkan.
5.  **Opsi `allowDiskUse`**: Jika operasi agregasi memerlukan memori lebih dari batas default (100MB), MongoDB akan menghasilkan galat. Opsi `allowDiskUse: true` dapat disertakan dalam perintah `aggregate()` untuk memungkinkan MongoDB menggunakan file temporer di disk. Namun, ini berpotensi memperlambat kinerja.

---

### Batasan Pipeline Agregasi

**Penjelasan:**
Meskipun merupakan fitur yang sangat berguna, pipeline agregasi memiliki beberapa batasan:

1.  **Ukuran Dokumen BSON**: Hasil dari setiap tahap (dan dokumen input/output) tidak boleh melebihi batas ukuran dokumen BSON, yaitu 16MB.
2.  **Memori per Tahap**: Secara default, setiap tahap agregasi dibatasi penggunaan memorinya hingga 100MB. Jika melebihi batas ini, diperlukan opsi `allowDiskUse: true`.
3.  **Jumlah Tahapan**: Tidak ada batasan keras mengenai jumlah tahapan, namun pipeline yang terlalu panjang dan kompleks dapat menyebabkan penurunan performa.
4.  **Batasan Operator Tertentu**: Beberapa operator agregasi mungkin memiliki batasan spesifik dalam penggunaannya. Selalu rujuk pada dokumentasi resmi MongoDB untuk detail.

---

### MongoDB Agregasi `$match`

**Penjelasan:**
Tahap `$match` berfungsi untuk menyaring dokumen. Hanya dokumen yang memenuhi kriteria yang ditentukan yang akan diteruskan ke tahap selanjutnya dalam pipeline. Sintaksnya serupa dengan kriteria kueri pada operasi `find()`.

**Sintaks Dasar:**

```javascript
{ $match: { <kriteria_kueri> } }
```

**Contoh Skenario:**
Akan dicari data penjualan dengan jumlah item lebih dari 5.

-   **Data Input (Contoh pada koleksi `penjualan`):**

```json
[
  { "produk_id": 101, "jumlah": 10, "tanggal": ISODate("2023-03-15") },
  { "produk_id": 102, "jumlah": 3, "tanggal": ISODate("2023-03-15") },
  { "produk_id": 103, "jumlah": 7, "tanggal": ISODate("2023-03-16") }
]
```

-   **Perintah Agregasi:**

```javascript
db.penjualan.aggregate([{ $match: { jumlah: { $gt: 5 } } }]);
```

-   **Hasil Output:**

```json
[
  { "_id": ObjectId("..."), "produk_id": 101, "jumlah": 10, "tanggal": ISODate("2023-03-15T00:00:00Z") },
  { "_id": ObjectId("..."), "produk_id": 103, "jumlah": 7, "tanggal": ISODate("2023-03-16T00:00:00Z") }
]
```

---

### MongoDB Agregasi `$group`

**Penjelasan:**
Tahap `$group` mengelompokkan dokumen input berdasarkan ekspresi `_id` yang ditentukan. Untuk setiap grup unik yang terbentuk, `$group` akan menghasilkan satu dokumen output. _Accumulator expressions_ (seperti `$sum`, `$avg`, `$min`, `$max`, `$push`, `$addToSet`, `$first`, `$last`) dapat digunakan untuk melakukan perhitungan pada field dari dokumen-dokumen dalam satu grup.

**Sintaks Dasar:**

```javascript
{
  $group: {
    _id: <ekspresi_untuk_pengelompokan>, // Dapat berupa nama field "$namaField" atau ekspresi kompleks
    namaFieldHasil1: { <accumulator1>: <ekspresi_atau_field_untuk_dihitung1> },
    namaFieldHasil2: { <accumulator2>: <ekspresi_atau_field_untuk_dihitung2> },
    // ...dan seterusnya
  }
}
```

**Contoh Skenario:**
Akan dihitung total jumlah produk terjual dan rata-rata jumlah per transaksi untuk setiap `produk_id`.

-   **Data Input (Contoh pada koleksi `penjualan`):**

```json
[
  { "produk_id": 101, "jumlah": 10, "pelanggan": "A" },
  { "produk_id": 102, "jumlah": 5, "pelanggan": "B" },
  { "produk_id": 101, "jumlah": 12, "pelanggan": "C" },
  { "produk_id": 103, "jumlah": 7, "pelanggan": "A" },
  { "produk_id": 102, "jumlah": 8, "pelanggan": "D" }
]
```

-   **Perintah Agregasi:**

```javascript
db.penjualan.aggregate([
  {
    $group: {
      _id: "$produk_id", // Kelompokkan berdasarkan produk_id
      totalTerjual: { $sum: "$jumlah" }, // Jumlahkan nilai field 'jumlah'
      rataRataPerTransaksi: { $avg: "$jumlah" }, // Hitung rata-rata nilai field 'jumlah'
      jumlahTransaksi: { $sum: 1 } // Hitung jumlah dokumen dalam setiap grup
    }
  }
]);
```

-   **Hasil Output:**

```json
[
  { "_id": 103, "totalTerjual": 7, "rataRataPerTransaksi": 7, "jumlahTransaksi": 1 },
  { "_id": 102, "totalTerjual": 13, "rataRataPerTransaksi": 6.5, "jumlahTransaksi": 2 },
  { "_id": 101, "totalTerjual": 22, "rataRataPerTransaksi": 11, "jumlahTransaksi": 2 }
]
```

---

### MongoDB Agregasi `$project`

**Penjelasan:**
Tahap `$project` digunakan untuk memilih field mana saja yang akan diteruskan ke tahap selanjutnya, mengganti nama field, atau bahkan membuat field baru berdasarkan ekspresi. Serupa dengan klausa `SELECT` pada SQL namun dengan fleksibilitas yang lebih tinggi.

**Sintaks Dasar:**

-   Menampilkan field: `{ $project: { namaField: 1, fieldLain: 1, _id: 0 } }` (nilai `1` untuk menampilkan, `0` untuk menyembunyikan `_id` atau field lainnya).
-   Mengganti nama field: `{ $project: { namaBaru: "$namaFieldLama" } }`.
-   Membuat field baru dengan ekspresi: `{ $project: { fieldBaru: { $add: ["$field1", "$field2"] } } }`.

**Contoh Skenario:**
Dari data produk, akan ditampilkan nama produk (diubah menjadi `item_name`), kategori, dan harga dalam Dolar AS (dengan asumsi harga asli dalam Rupiah, dan 1 USD = 15000 IDR).

-   **Data Input (Contoh pada koleksi `produk`):**

```json
[{ "_id": 1, "nama": "Apel Fuji", "kategori": "Buah", "harga_rp": 45000, "stok": 50 }]
```

-   **Perintah Agregasi:**

```javascript
db.produk.aggregate([
  {
    $project: {
      _id: 0, // Sembunyikan field _id bawaan
      item_name: "$nama", // Ganti nama field 'nama' menjadi 'item_name'
      kategori_produk: "$kategori", // Tampilkan field 'kategori' dengan nama baru
      harga_usd: { $divide: ["$harga_rp", 15000] } // Buat field baru 'harga_usd' hasil pembagian
    }
  }
]);
```

-   **Hasil Output:**

```json
[{ "item_name": "Apel Fuji", "kategori_produk": "Buah", "harga_usd": 3 }]
```

---

### MongoDB Agregasi `$sort`

**Penjelasan:**
Tahap `$sort` mengurutkan dokumen yang masuk berdasarkan field yang ditentukan. Pengurutan dapat dilakukan secara menaik (ascending, dengan nilai `1`) atau menurun (descending, dengan nilai `-1`).

**Sintaks Dasar:**

```javascript
{ $sort: { fieldUntukSort1: <1_atau_-1>, fieldUntukSort2: <1_atau_-1>, ... } }
```

**Contoh Skenario:**
Akan diurutkan produk berdasarkan kategori (A-Z), kemudian berdasarkan harga (termurah ke termahal) di dalam setiap kategori.

-   **Data Input (Contoh pada koleksi `produk`):**

```json
[
  { "nama": "Bayam", "kategori": "Sayur", "harga": 2000 },
  { "nama": "Apel", "kategori": "Buah", "harga": 5000 },
  { "nama": "Mangga", "kategori": "Buah", "harga": 7000 },
  { "nama": "Kangkung", "kategori": "Sayur", "harga": 1500 }
]
```

-   **Perintah Agregasi:**

```javascript
db.produk.aggregate([
  { $sort: { kategori: 1, harga: 1 } } // Urutkan berdasarkan kategori (A-Z), lalu harga (termurah)
]);
```

-   **Hasil Output:**

```json
[
  { "_id": ObjectId("..."), "nama": "Apel", "kategori": "Buah", "harga": 5000 },
  { "_id": ObjectId("..."), "nama": "Mangga", "kategori": "Buah", "harga": 7000 },
  { "_id": ObjectId("..."), "nama": "Kangkung", "kategori": "Sayur", "harga": 1500 },
  { "_id": ObjectId("..."), "nama": "Bayam", "kategori": "Sayur", "harga": 2000 }
]
```

---

### MongoDB Agregasi `$count`

**Penjelasan:**
Tahap `$count` adalah metode sederhana untuk menghitung jumlah dokumen yang melewatinya dalam pipeline. Hasilnya adalah satu dokumen tunggal yang berisi sebuah field (dengan nama yang ditentukan pengguna) yang nilainya adalah jumlah dokumen tersebut.

**Sintaks Dasar:**

```javascript
{ $count: "nama_field_untuk_hasil_hitungan" }
```

**Contoh Skenario:**
Akan dihitung berapa banyak produk yang harganya di atas 4000.

-   **Data Input (Contoh pada koleksi `produk`):**

```json
[
  { "nama": "Apel", "kategori": "Buah", "harga": 5000 },
  { "nama": "Bayam", "kategori": "Sayur", "harga": 2000 },
  { "nama": "Mangga", "kategori": "Buah", "harga": 7000 }
]
```

-   **Perintah Agregasi:**

```javascript
db.produk.aggregate([
  { $match: { harga: { $gt: 4000 } } }, // Filter produk dengan harga > 4000
  { $count: "jumlahProdukMahal" }      // Hitung jumlah dokumen hasil filter
]);
```

-   **Hasil Output:**

```json
[{ "jumlahProdukMahal": 2 }]
```

---

### MongoDB Agregasi `$lookup` (Serupa Join)

**Penjelasan:**
Tahap `$lookup` memungkinkan dilakukannya operasi "left outer join" dengan koleksi lain dalam basis data yang sama. Dengan demikian, informasi dari dua koleksi dapat digabungkan. Tahap ini akan menambahkan field baru (berupa array) ke dokumen input, yang berisi dokumen-dokumen yang cocok dari koleksi "tujuan" (foreign collection).

**Sintaks Dasar:**

```javascript
{
  $lookup: {
    from: "nama_koleksi_tujuan_join", // Koleksi yang akan digabungkan
    localField: "field_di_koleksi_ini_untuk_join", // Field pada dokumen input sebagai kunci join
    foreignField: "field_di_koleksi_tujuan_untuk_join", // Field pada dokumen koleksi 'from' sebagai kunci join
    as: "nama_field_baru_hasil_join" // Nama field array output yang berisi dokumen hasil join
  }
}
```

**Contoh Skenario:**
Terdapat koleksi `pesanan` yang berisi `produk_id`, dan koleksi `produkInfo` yang berisi detail produk. Akan ditampilkan data pesanan lengkap dengan nama produknya.

-   **Data Input (Koleksi `pesanan`):**

```json
[
  { "_id": 1, "item_id": "A001", "jumlah": 2 },
  { "_id": 2, "item_id": "B002", "jumlah": 1 }
]
```

-   **Data Input (Koleksi `produkInfo`):**

```json
[
  { "_id": "A001", "nama_produk": "Laptop XYZ", "harga": 15000000 },
  { "_id": "B002", "nama_produk": "Mouse Gaming", "harga": 500000 },
  { "_id": "C003", "nama_produk": "Keyboard Mekanik", "harga": 1200000 }
]
```

-   **Perintah Agregasi (dijalankan pada koleksi `pesanan`):**

```javascript
db.pesanan.aggregate([
  {
    $lookup: {
      from: "produkInfo",
      localField: "item_id",
      foreignField: "_id",
      as: "detailProduk"
    }
  }
]);
```

-   **Hasil Output:**

```json
[
  {
    "_id": 1,
    "item_id": "A001",
    "jumlah": 2,
    "detailProduk": [{ "_id": "A001", "nama_produk": "Laptop XYZ", "harga": 15000000 }]
  },
  {
    "_id": 2,
    "item_id": "B002",
    "jumlah": 1,
    "detailProduk": [{ "_id": "B002", "nama_produk": "Mouse Gaming", "harga": 500000 }]
  }
]
```

**Tips:** Umumnya setelah `$lookup`, dapat digunakan tahap `$unwind` untuk memecah array hasil `lookup` (jika diasumsikan hanya ada satu dokumen yang cocok atau ingin diproses satu per satu) dan `$project` untuk menata ulang field sesuai kebutuhan.

---

### MongoDB Agregasi `$first` (dan `$last`)

**Penjelasan:**
Operator `$first` dan `$last` umumnya digunakan di dalam tahap `$group`. Operator `$first` akan mengambil nilai dari field tertentu dari dokumen _pertama_ yang masuk ke dalam grup tersebut. Sebaliknya, `$last` mengambil dari dokumen _terakhir_.
**Penting**: Jika urutan dokumen dalam grup signifikan untuk menentukan mana yang dianggap "pertama" atau "terakhir" (selain dari pengelompokan berdasarkan `_id` itu sendiri), **harus** digunakan tahap `$sort` _sebelum_ tahap `$group` agar hasilnya konsisten dan sesuai dengan ekspektasi.

**Sintaks Dasar (di dalam `$group`):**

```javascript
{
  $group: {
    _id: <ekspresi_pengelompokan>,
    fieldPertama: { $first: "$namaFieldSumber" },
    fieldTerakhir: { $last: "$namaFieldSumber" }
  }
}
```

**Contoh Skenario:**
Dari data penjualan, akan dicari tanggal transaksi pertama untuk setiap produk.

-   **Data Input (Contoh pada koleksi `penjualan`):**

```json
[
  { "produk_id": 101, "jumlah": 10, "tanggal": ISODate("2023-03-15") },
  { "produk_id": 102, "jumlah": 5, "tanggal": ISODate("2023-03-10") },
  { "produk_id": 101, "jumlah": 12, "tanggal": ISODate("2023-02-20") }, // Tanggal lebih awal dari 15 Maret
  { "produk_id": 102, "jumlah": 8, "tanggal": ISODate("2023-03-12") }
]
```

-   **Perintah Agregasi:**

```javascript
db.penjualan.aggregate([
  { $sort: { tanggal: 1 } }, // Urutkan berdasarkan tanggal (paling lama terlebih dahulu) - PENTING!
  {
    $group: {
      _id: "$produk_id",
      tanggalTransaksiPertama: { $first: "$tanggal" },
      tanggalTransaksiTerakhir: { $last: "$tanggal" } // Contoh penggunaan $last
    }
  }
]);
```

-   **Hasil Output:**

```json
[
  {
    "_id": 101,
    "tanggalTransaksiPertama": ISODate("2023-02-20T00:00:00Z"),
    "tanggalTransaksiTerakhir": ISODate("2023-03-15T00:00:00Z")
  },
  {
    "_id": 102,
    "tanggalTransaksiPertama": ISODate("2023-03-10T00:00:00Z"),
    "tanggalTransaksiTerakhir": ISODate("2023-03-12T00:00:00Z")
  }
]
```

---

### MongoDB Agregasi `$out`

**Penjelasan:**
Tahap `$out` harus menjadi tahap _terakhir_ dalam pipeline agregasi. Fungsinya adalah untuk mengambil hasil dari pipeline dan menuliskannya ke dalam sebuah koleksi baru. Jika koleksi dengan nama yang sama sudah ada, `$out` akan **menimpanya** (overwrite). Tahap ini berguna jika hasil agregasi akan sering dikueri atau digunakan kembali di masa mendatang.

**Sintaks Dasar:**

```javascript
{ $out: "nama_koleksi_output_baru" }
```

**Contoh Skenario:**
Akan dihitung total penjualan per kategori produk, dan hasilnya disimpan ke koleksi baru bernama `ringkasanPenjualanKategori`.

-   **Data Input (Contoh pada koleksi `produkDijual`):**

```json
[
  { "nama": "Apel", "kategori": "Buah", "terjual": 50, "total_harga": 250000 },
  { "nama": "Bayam", "kategori": "Sayur", "terjual": 100, "total_harga": 200000 },
  { "nama": "Mangga", "kategori": "Buah", "terjual": 30, "total_harga": 210000 },
  { "nama": "Kangkung", "kategori": "Sayur", "terjual": 120, "total_harga": 180000 }
]
```

-   **Perintah Agregasi:**

```javascript
db.produkDijual.aggregate([
  {
    $group: {
      _id: "$kategori",
      totalUnitTerjualSeluruhProduk: { $sum: "$terjual" },
      totalPendapatanPerKategori: { $sum: "$total_harga" }
    }
  },
  { $sort: { totalPendapatanPerKategori: -1 } },
  { $out: "ringkasanPenjualanKategori" } // Simpan hasilnya ke koleksi baru
]);
```

-   **Hasil Output (Tidak langsung tampil di konsol, tetapi tersimpan di koleksi `ringkasanPenjualanKategori`):**
    Setelah perintah di atas dijalankan, kalian dapat memeriksa koleksi `ringkasanPenjualanKategori`:
    ```javascript
    db.ringkasanPenjualanKategori.find().pretty();
    ```
    Akan menghasilkan:

```json
[
  {
    "_id": "Buah",
    "totalUnitTerjualSeluruhProduk": 80,
    "totalPendapatanPerKategori": 460000
  },
  {
    "_id": "Sayur",
    "totalUnitTerjualSeluruhProduk": 220,
    "totalPendapatanPerKategori": 380000
  }
]
```

