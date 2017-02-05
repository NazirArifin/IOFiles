# IOFiles
Menghandle beberapa hal umum yang berhubungan dengan file seperti save, read, move, upload, force download, zip, extract, resize image, crop image, watermark, dsb.  Tidak semua fungsi dalam Filesystem PHP ada dalam class ini.

## Tabel Isi / Table of Contents


## Instalasi / Installation
Unduh file dalam folder scr atau gunakan composer dengan perintah:
```sh
$ composer require nazir/iofiles
```

## Penggunaan / Usage
Buat object dari class dengan perintah:
```php
use \IOFiles\IOFiles;
$iofiles = new IOFiles;
```
setelah ini object menggunakan beberapa public method sebagai berikut:
### - Menggandakan file
```php
$iofiles->copy($file, $destination);
```
### - Menghapus file
```php
$iofiles->delete($file);
```
### - Memaksa _browser_ mendownload file
```php
$iofiles->download($file);
```
- Fungsi ini harus dipanggil sebelum ada output apapun karena dia akan mengeset beberapa header.

### - Lihat properti file
```php
$iofiles->get_attrib($file, [$flags]);
```
- `$flag` bisa berisi string salah satu atribut berikut atau array berisi kumpulan atribut:
* `'size'` - ukuran file (dalam bytes)
* `'name'` - nama file
* `'type'` - jenis file
* `'path'` - lokasi path file
* `'perms` - hak akses pada file
* `'read'` - apakah file dapat dibaca (true/false), menggunakan PHP is_readable() lebih baik
* `'write'` - apakah file dapat ditulis (true/false), menggunakan PHP is_writeable() lebih baik
* `'accessed'` - kapan terakhir file diakses dengan format `(Y-m-d H:i:s)`
* `'modified'` - kapan terakhir file diubah dengan format `(Y-m-d H:i:s)`

Contoh penggunaan yang valid adalah:
```php
$iofiles->get_attrib('index.php', 'size');
$iofiles->get_attrib('index.php', array('size', 'perms');
```

### - Lihat ekstensi file
```php
$iofiles->get_type($file)
```
- Mendapatkan ekstensi dari file di UPPERCASE. Contoh `index.php` -> `'PHP'`

## Kredit? / Credits
Beberapa fungsi diadaptasi dari kode di [CodeIgniter](http://codeigniter.com)

## License / Lisensi
MIT
__Free Software__