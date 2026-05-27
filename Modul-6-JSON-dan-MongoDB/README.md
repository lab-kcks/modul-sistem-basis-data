# Modul 6: JSON dan MongoDB (CRUD Dasar)

## Daftar Isi

1. [Pengenalan: SQL vs NoSQL](#1-pengenalan-sql-vs-nosql)
2. [Pengenalan JSON](#2-pengenalan-json)
3. [Instalasi MongoDB](#3-instalasi-mongodb)
4. [Konsep Dasar MongoDB](#4-konsep-dasar-mongodb)
5. [Operasi CRUD](#5-operasi-crud)
6. [Latihan Singkat](#6-latihan-singkat)

---

## 1. Pengenalan: SQL vs NoSQL

MongoDB adalah salah satu basis data **NoSQL** (_Not Only SQL_) berorientasi dokumen. Berbeda dengan basis data relasional (MySQL, PostgreSQL) yang menyimpan data dalam tabel dengan skema kaku, MongoDB menyimpan data sebagai dokumen **BSON** (Binary JSON) dengan skema fleksibel.

| Aspek          | MongoDB (NoSQL)                       | MySQL (Relasional)                |
| -------------- | ------------------------------------- | --------------------------------- |
| Model Data     | Dokumen (BSON, mirip JSON)            | Tabel & Baris                     |
| Skema          | Dinamis, bisa berubah-ubah            | Tetap, didefinisikan di awal      |
| Bahasa Kueri   | MongoDB Query Language (MQL)          | SQL                               |
| Skalabilitas   | Horizontal (_sharding_)               | Vertikal (umumnya)                |
| Cocok untuk    | Big Data, data tidak terstruktur, CMS | Transaksi ACID, data relasional   |

**Kapan pakai MongoDB?** Saat skema data sering berubah, volume data besar, atau struktur data bersarang (_nested_) seperti katalog produk dan log aktivitas.

---

## 2. Pengenalan JSON

**JSON (JavaScript Object Notation)** adalah format pertukaran data berbasis teks yang ringan dan mudah dibaca manusia. MongoDB menggunakan ekstensi binernya (BSON), sehingga memahami JSON adalah prasyarat utama.

### Struktur Dasar

JSON terdiri atas dua struktur utama:

- **Objek**: pasangan _key/value_ dalam kurung kurawal `{}`.
- **Array**: kumpulan nilai berurutan dalam kurung siku `[]`.

```json
{
  "nama": "Budi",
  "usia": 30,
  "hobi": ["membaca", "memasak"],
  "alamat": {
    "kota": "Jakarta",
    "kodePos": "12345"
  },
  "menikah": false,
  "pasangan": null
}
```

<img src="./img/strukturjson.png" />

### Tipe Data JSON

- **String**: `"halo"`
- **Number**: `100`, `3.14`
- **Boolean**: `true` / `false`
- **Null**: `null`
- **Array**: `[1, 2, 3]`
- **Object**: `{ "key": "value" }`

> BSON menambahkan tipe seperti `ObjectId`, `Date`, dan `Binary` yang tidak ada di JSON murni.

---

## 3. Instalasi MongoDB

Pada Windows, dibutuhkan **dua paket terpisah**: MongoDB Community Server dan MongoDB Shell (`mongosh`).

### a. Install MongoDB Community Server

Unduh dari [MongoDB Download Center](https://www.mongodb.com/download-center/community), lalu jalankan installer.

<img src="./img/mongodbcommunityserver.png" style="width: 400px" />

Pilih setup type **Complete**:

<img src="./img/setuptype.png" />

Biarkan _service config_ pada nilai default. **Catat path instalasi** karena akan dibutuhkan nanti.

<img src="./img/serviceconfig.png" />

> MongoDB Compass (GUI) bersifat opsional. Praktikum tetap menggunakan `mongosh`.

### b. Install MongoDB Shell (`mongosh`)

Unduh dari [MongoDB Tools Download](https://www.mongodb.com/try/download/shell). Pilih paket **MSI** untuk kemudahan instalasi.

<img src="./img/mongodbshell.png" style="width: 400px" />

Catat path instalasi `mongosh`:

<img src="./img/mongoshpath.png" />

### c. Tambahkan ke Environment Variable

Buka _Edit System Environment Variables_, lalu tambahkan path `mongosh` ke variabel `Path`.

<img src="./img/mongosh.png" />

### d. Verifikasi

Buka terminal baru, ketik `mongosh`. Jika berhasil terhubung, instalasi selesai.

<img src="./img/doninstall.png" />

---

## 4. Konsep Dasar MongoDB

Hierarki penyimpanan MongoDB:

```
Database  →  Collection  →  Document
(MySQL: Database  →  Table  →  Row)
```

- **Database**: wadah utama yang berisi banyak _collection_.
- **Collection**: kumpulan dokumen (analog dengan tabel di SQL, tapi tanpa skema kaku).
- **Document**: unit data dalam format BSON (mirip JSON), unit terkecil di MongoDB.
- **`_id`**: setiap dokumen wajib punya field `_id` unik. Jika tidak diisi manual, MongoDB membuat `ObjectId` otomatis.

### Perintah Navigasi

```shell
show dbs                  # daftar semua database
use nama_database         # pindah / buat database
show collections          # daftar collection di database aktif
db.namaCollection.find()  # lihat isi collection
```

<img src='./img/6.png' />

> Database dan collection dibuat otomatis saat dokumen pertama disisipkan.

---

## 5. Operasi CRUD

CRUD = **Create, Read, Update, Delete**. Inilah operasi inti yang harus dikuasai.

### 5.1 Create — Menyisipkan Data

#### `insertOne()` — satu dokumen

```javascript
db.mahasiswa.insertOne({
  nim: "21120001",
  nama: "Andi",
  ipk: 3.5
});
```

#### `insertMany()` — banyak dokumen

```javascript
db.mahasiswa.insertMany([
  { nim: "21120002", nama: "Budi", ipk: 3.2 },
  { nim: "21120003", nama: "Citra", ipk: 3.8 },
  { nim: "21120004", nama: "Dedi", ipk: 2.9 }
]);
```

<img src='./img/7.jpeg' />

### 5.2 Read — Membaca Data

#### `findOne()` — satu dokumen pertama yang cocok

```javascript
db.mahasiswa.findOne({ nama: "Andi" });
```

#### `find()` — semua dokumen yang cocok

```javascript
db.mahasiswa.find({});                 // semua dokumen
db.mahasiswa.find({ ipk: { $gte: 3.5 } });  // IPK >= 3.5
```

#### Proyeksi (memilih field tertentu)

Argumen kedua menentukan field mana yang ditampilkan. `1` = tampilkan, `0` = sembunyikan.

```javascript
db.mahasiswa.find(
  { ipk: { $gte: 3.5 } },
  { _id: 0, nama: 1, ipk: 1 }
);
```

#### Operator Kueri yang Sering Dipakai

| Operator | Arti                  | Contoh                           |
| -------- | --------------------- | -------------------------------- |
| `$eq`    | sama dengan           | `{ ipk: { $eq: 3.5 } }`          |
| `$gt`    | lebih besar           | `{ ipk: { $gt: 3.0 } }`          |
| `$gte`   | lebih besar / sama    | `{ ipk: { $gte: 3.0 } }`         |
| `$lt`    | lebih kecil           | `{ ipk: { $lt: 3.0 } }`          |
| `$in`    | ada di dalam list     | `{ nama: { $in: ["Andi","Budi"] } }` |
| `$and`   | dan                   | `{ $and: [{...}, {...}] }`       |
| `$or`    | atau                  | `{ $or: [{...}, {...}] }`        |

### 5.3 Update — Memperbarui Data

#### `updateOne()` — perbarui satu dokumen

```javascript
db.mahasiswa.updateOne(
  { nim: "21120001" },          // filter
  { $set: { ipk: 3.7 } }        // perubahan
);
```

#### `updateMany()` — perbarui banyak dokumen

```javascript
db.mahasiswa.updateMany(
  { ipk: { $lt: 3.0 } },
  { $set: { status: "Pembinaan" } }
);
```

#### Operator Update Penting

- `$set`: mengatur nilai field (membuat field baru jika belum ada).
- `$unset`: menghapus field.
- `$inc`: menambah/mengurangi nilai numerik.
- `$rename`: mengganti nama field.
- `$push` / `$pull`: tambah / hapus elemen pada field array.

```javascript
// Naikkan IPK semua mahasiswa di koleksi sebesar 0.1
db.mahasiswa.updateMany({}, { $inc: { ipk: 0.1 } });
```

### 5.4 Delete — Menghapus Data

#### `deleteOne()` — hapus satu dokumen pertama yang cocok

```javascript
db.mahasiswa.deleteOne({ nim: "21120004" });
```

#### `deleteMany()` — hapus semua dokumen yang cocok

```javascript
db.mahasiswa.deleteMany({ ipk: { $lt: 2.5 } });
```

> ⚠️ **Hati-hati**: `db.mahasiswa.deleteMany({})` akan **menghapus semua dokumen** dalam collection. Operasi delete bersifat permanen.

---

## 6. Latihan Singkat

Coba kerjakan langsung di `mongosh`:

1. Buat database bernama `kampus`.
2. Buat collection `mahasiswa` dan masukkan 5 dokumen mahasiswa (NIM, nama, jurusan, IPK).
3. Tampilkan semua mahasiswa dengan IPK di atas 3.0, hanya tampilkan nama dan IPK saja.
4. Update IPK salah satu mahasiswa menjadi 4.0.
5. Tambahkan field baru `angkatan: 2021` ke semua mahasiswa.
6. Hapus mahasiswa dengan IPK di bawah 2.5.

---

## Ringkasan Perintah

| Operasi        | Perintah                                   |
| -------------- | ------------------------------------------ |
| Pilih database | `use namaDB`                               |
| Insert satu    | `db.coll.insertOne({...})`                 |
| Insert banyak  | `db.coll.insertMany([{...}, {...}])`       |
| Cari satu      | `db.coll.findOne({...})`                   |
| Cari banyak    | `db.coll.find({...})`                      |
| Update satu    | `db.coll.updateOne({filter}, {$set:{...}})` |
| Update banyak  | `db.coll.updateMany({filter}, {$set:{...}})` |
| Hapus satu     | `db.coll.deleteOne({filter})`              |
| Hapus banyak   | `db.coll.deleteMany({filter})`             |

---

> **Catatan**: untuk materi **Aggregation Pipeline** (`$match`, `$group`, `$project`, `$sort`, `$lookup`, `$out`, dll), silakan lihat versi lengkap di `README-original.md` atau modul lanjutan. Topik tersebut layak mendapat modul sendiri karena konsep _pipeline_ dan _accumulator_ membutuhkan latihan tersendiri.
