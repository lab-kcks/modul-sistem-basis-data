# Module 0 – Setup & Installation

## Daftar Isi

* [Instalasi XAMPP](#instalasi-xampp)

  * [Download XAMPP](#download-xampp)
  * [Proses Instalasi XAMPP](#proses-instalasi-xampp)
  * [Menjalankan XAMPP](#menjalankan-xampp)
  * [Mengakses phpMyAdmin](#mengakses-phpmyadmin)
* [Instalasi MongoDB](#instalasi-mongodb)
* [Instalasi MongoDB Shell (mongosh)](#instalasi-mongodb-shell-mongosh)

---

# Instalasi XAMPP

Pada praktikum ini, MySQL akan dijalankan menggunakan XAMPP.
XAMPP adalah paket software yang berisi:

* Apache (Web Server)
* MySQL / MariaDB (Database)
* PHP
* phpMyAdmin (Database Management berbasis web)

XAMPP digunakan untuk menjalankan server lokal (localhost) sehingga kita dapat mengembangkan dan menguji database tanpa koneksi internet.

---

## Download XAMPP

1. Buka website resmi Apache Friends:
   [https://www.apachefriends.org/index.html](https://www.apachefriends.org/index.html)

2. Pilih versi sesuai sistem operasi yang digunakan (Windows / macOS / Linux).

![17719370907008675337065351077660](https://github.com/user-attachments/assets/ea2ccb30-0b6a-45ea-863a-21d33ecf435b)

---

## Proses Instalasi XAMPP

1. Jalankan file installer yang telah diunduh.
2. Klik **Next** pada halaman awal instalasi.

![1771937277031169472281487605555](https://github.com/user-attachments/assets/aef41d38-aef8-4fab-9e1e-dc6b1f257f66)

3. Pada halaman pemilihan komponen:

   * Pastikan **Apache**
   * **MySQL**
   * **phpMyAdmin**

   dalam keadaan tercentang.

![17719373370668031042061510535071](https://github.com/user-attachments/assets/4c6dcbf8-48e0-4140-8eb4-cbb254a367c2)

4. Pilih folder instalasi (default biasanya: `C:\xampp`).

![17719373949478793754939476201315](https://github.com/user-attachments/assets/f894e3db-d419-46cb-b260-cc125d55e1ce)

5. Klik **Next** hingga proses instalasi selesai.

![17719374527514946659208405617235](https://github.com/user-attachments/assets/0c970faa-a5ec-403a-99b7-f917c9f1c8fb)

6. Klik **Finish** untuk membuka XAMPP Control Panel.

![17719375052596439207120433294654](https://github.com/user-attachments/assets/84696d9b-3601-4ec8-bc3f-b2244b9b3718)

---

## Menjalankan XAMPP

1. Buka **XAMPP Control Panel**
2. Klik tombol **Start** pada:

   * Apache
   * MySQL

Jika berhasil, keduanya akan berwarna hijau.

![17719375365775795544412321717726](https://github.com/user-attachments/assets/077c99d1-9bd1-4caf-9d3a-9bdc877c4ff7)

Untuk memastikan server berjalan dengan baik, buka browser dan akses:

```
http://localhost
```

atau

```
http://127.0.0.1
```

Jika halaman XAMPP muncul, berarti instalasi berhasil.

---

## Mengakses phpMyAdmin

phpMyAdmin adalah tool berbasis web untuk mengelola database MySQL.

Untuk mengaksesnya:

```
http://localhost/phpmyadmin
```

Jika berhasil, akan muncul halaman utama phpMyAdmin seperti berikut:

![1771937574435708280315611648113](https://github.com/user-attachments/assets/3e979639-911d-43a3-a0d5-f6e5619ca752)

Melalui phpMyAdmin, kita dapat:

* Membuat database
* Membuat tabel
* Menjalankan query SQL
* Mengelola data

---

