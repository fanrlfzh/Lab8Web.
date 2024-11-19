# Lab8Web.

#Membuat database
CREATE DATABASE latihann1;
![Screenshot (514)](https://github.com/user-attachments/assets/4a33bd3e-34dd-4815-b6e5-71813dd48255)

#Membuat tabel
CREATE TABLE data_barangg (
id_barang int(10) auto_increment Primary Key,
kategori varchar(30),
nama varchar(30),
gambar varchar(100),
harga_beli decimal(10,0),
harga_jual decimal(10,0),
stok int(4)
);
![WhatsApp Image 2024-11-19 at 09 35 56](https://github.com/user-attachments/assets/092b8231-8c2f-4d62-be85-83e6d04fece5)

#Menambahkan data
INSERT INTO data_barangg (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES 
('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
![WhatsApp Image 2024-11-19 at 09 35 56 (1)](https://github.com/user-attachments/assets/3c2e7af4-4da7-4903-9c86-16574042b809)

#Tampilan table data barang
![WhatsApp Image 2024-11-19 at 09 36 24](https://github.com/user-attachments/assets/a0716550-7149-48e0-b0fa-52150dce7ce2)

#Koneksi.php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihann1";

$conn = mysqli_connect($host, $user, $pass, $db);

if ($conn == false) {
    echo "Koneksi ke server gagal.";
    die();
} else {
    echo "Koneksi berhasil";
}
?>
![WhatsApp Image 2024-11-19 at 09 48 45](https://github.com/user-attachments/assets/870cf199-1367-40d8-9524-442fd0812149)

#Indeks.php
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barangg';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Data Barang</title>
</head>
<body>
<div class="container">
<h1>Data Barang</h1>
<div class="main">
<table>
<tr>
<th>Gambar</th>
<th>Nama Barang</th>
<th>Katagori</th>
<th>Harga Jual</th>
<th>Harga Beli</th>
<th>Stok</th>
<th>Aksi</th>
</tr>
<?php if($result): ?>
<?php while($row = mysqli_fetch_array($result)): ?>
<tr>
<td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
<td><?= $row['nama'];?></td>
<td><?= $row['kategori'];?></td>
<td><?= $row['harga_beli'];?></td>
<td><?= $row['harga_jual'];?></td>
<td><?= $row['stok'];?></td>
<td><?= $row['id_barang'];?></td>
</tr>
<?php endwhile; else: ?>
<tr>
<td colspan="7">Belum ada data</td>
</tr>
<?php endif; ?>
</table>
</div>
</div>
</body>
</html>
![WhatsApp Image 2024-11-19 at 10 02 31](https://github.com/user-attachments/assets/6cfe455e-8d94-426c-8caf-da7dd4356170)

#tambah.php
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barangg';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <!-- Bagian ini dihapus karena menampilkan link Tambah Barang -->
        <!--
        <div>
            <a href="tambah.php" style="text-decoration: none; color: blue;">Tambah Barang</a>
        </div>
        -->
        <div class="main">
            <table border="1">
                <tr>
                    <th>Gambar</th>
                    <th>Nama Barang</th>
                    <th>Kategori</th>
                    <th>Harga Jual</th>
                    <th>Harga Beli</th>
                    <th>Stok</th>
                    <th>Aksi</th> 
                </tr>
                <?php if ($result && mysqli_num_rows($result) > 0): ?>
                    <?php while ($row = mysqli_fetch_assoc($result)): ?>
                        <tr>
                            <td>
                                <img src="<?= $row['gambar']; ?>" alt="<?= $row['nama']; ?>" width="100">
                            </td>
                            <td><?= $row['nama']; ?></td>
                            <td><?= $row['kategori']; ?></td>
                            <td><?= $row['harga_jual']; ?></td>
                            <td><?= $row['harga_beli']; ?></td>
                            <td><?= $row['stok']; ?></td>
                            <td>
                                <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a>
                                |
                                <a href="hapus.php?id=<?= $row['id_barang']; ?>" onclick="return confirm('Apakah Anda yakin ingin menghapus data ini?')">Hapus</a>
                            </td>
                        </tr>
                    <?php endwhile; ?>
                <?php else: ?>
                    <tr>
                        <td colspan="7">Belum ada data</td>
                    </tr>
                <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
![WhatsApp Image 2024-11-19 at 10 10 38](https://github.com/user-attachments/assets/6ea46100-0326-4212-8b79-7c1523cf57eb)

#Ubah.php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

// Validasi parameter ID
if (!isset($_GET['id']) || empty($_GET['id'])) {
    die('Error: ID tidak ditemukan di URL.');
}
$id = $_GET['id'];

// Query untuk mendapatkan data berdasarkan ID
$sql = "SELECT * FROM data_barangg WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);

if (!$result || mysqli_num_rows($result) == 0) {
    die('Error: Data tidak ditemukan di database.');
}

$data = mysqli_fetch_array($result);

// Variabel untuk memuat data
$nama = $data['nama'];
$kategori = $data['kategori'];
$harga_jual = $data['harga_jual'];
$harga_beli = $data['harga_beli'];
$stok = $data['stok'];

function is_select($var, $val) {
    return $var == $val ? 'selected="selected"' : '';
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $nama; ?>" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select('Komputer', $kategori); ?> value="Komputer">Komputer</option>
                    <option <?php echo is_select('Elektronik', $kategori); ?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select('Hand Phone', $kategori); ?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $harga_jual; ?>" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $harga_beli; ?>" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $stok; ?>" />
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="hidden" name="id" value="<?php echo $id; ?>" />
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
![WhatsApp Image 2024-11-19 at 10 42 34](https://github.com/user-attachments/assets/f02ed8a6-3d6b-4915-bec5-ad67e520c125)

#Hapus.php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barangg WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: indeks.php');
?>
![WhatsApp Image 2024-11-19 at 10 51 53](https://github.com/user-attachments/assets/270224d9-b463-4d2f-ad77-55e222939cad)










