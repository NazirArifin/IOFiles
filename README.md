# IOFiles
Menghandle beberapa hal umum yang berhubungan dengan file seperti save, read, move, upload, force download, zip, extract, resize image, crop image, watermark, dsb.  Tidak semua fungsi dalam Filesystem PHP ada dalam class ini.

## Tabel Isi / Table of Contents

1. [Inisialisasi Object Baru](#--inisialisasi-object-baru)
2. [Operasi Pada File](#--operasi-pada-file)
3. [Upload File](#--upload-file)
4. [Operasi ZIP](#--operasi-zip)
5. [Operasi Gambar / Image](#--operasi-gambar--image)

## Instalasi / Installation
Unduh file dalam folder scr atau gunakan composer dengan perintah:
```sh
$ composer require nazir/iofiles
```

## Penggunaan / Usage
### - Inisialisasi Object Baru
```php
use \IOFiles\IOFiles;
$iofiles = new IOFiles;
\\ jika tanpa use maka $iofiles = new \IOFiles\IOFiles;
```
setelah ini object menggunakan beberapa public method sebagai berikut:
### - Operasi Pada File

#### a. Menggandakan file (_copy_)
```php
$iofiles->copy($file, $destination);
```
#### b. Memindah file ke lokasi baru (_move_)
```php
$iofiles->move($oldfile, $newfile);
```
#### c. Mengubah nama file (_rename_)
```php
$iofiles->rename($oldfile, $newfile);
```

#### d. Menghapus file (_delete_)
```php
$iofiles->delete($file);
```

#### e. Memaksa _browser_ mendownload file (_force download_)
```php
$iofiles->download($file);
```
Fungsi ini harus dipanggil sebelum ada output apapun karena dia akan mengeset beberapa header.

#### f. Gzip file
```php
$iofiles->gz_file($file);
```
File hasil gzip akan diletakkan bersebelahan dengan file sumber dengan nama `filesumber.gz`

#### g. Lihat properti file (_properties_)
```php
$iofiles->get_attrib($file, [$flags]);
```
`$flag` bisa berisi string salah satu atribut berikut atau array berisi kumpulan atribut (jika semua semua atribut akan di-_return_):
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

#### h. Lihat ekstensi file (_extension_)
```php
$iofiles->get_type($file)
```
Mendapatkan ekstensi dari file di UPPERCASE. Contoh `index.php` -> `'PHP'`

#### i. Mencari file (_find_)
```php
$iofiles->find($keyword, [$directory]);
```
`$directory` adalah folder dimana file dicari, defaultnya adalah _current_directory_ (.). Return array nama file jika ada file yang sesuai.

#### j. Mencari file (_search_)
```php
$iofiles->search($keyword, [$directory]);
// sama dengan $iofiles->find(..) :D
```

#### k. Membaca isi file (_read_)
```php
$iofiles->read($file, [[[$returnType], [$length]], [$readType]]);
```
* `$returnType` : dengan nilai valid __1__ (output berupa _string_), __2__ (output berupa _array_ dengan pemisah baris sebagai delimiter).
* `$length` : bytes maksimal pembacaan file, defaultnya adalah sesuai ukuran file
* `$readType` : mode baca dengan nilai __1__ (mode text), __2__ (mode biner)

#### l. Menulis data ke file (_write_)
```php
$iofiles->write($file, [[$content], [$mode]]);
```
`$mode` bisa diisi dengan nilai `'ab'` alias _append_ (default), `'wb'` alias _overwrite_, dsb. __Warning__: tidak ada validasi jika mode yang dimasukkan invalid. Lihat dokumentasi di PHP untuk mode yang valid.

### - Upload File
#### a. Konfigurasi (_configuration_)
```php
$iofiles->upload_config($config);
```
Harus dipanggil sebelum proses upload dimulai. `$config` berisi array dengan nilai valid sebagai berikut:
* `'max_size'` - ukuran maksimum file, juga dibatasi di `php.ini`
* `'max_filename'` - ukuran maksimum nama file
* `'allowed_types'` - ekstensi yang diperbolehkan, contoh: `'jpg|jpeg|png'`
* `'upload_path'` - lokasi untuk meletakkan file hasil upload
* `'overwrite'` - (true/false)
* `'encrypt_name'` - (true/false) apakah nama file baru diganti nama acak

Contoh penggunaan yang valid adalah:
```php
$iofiles->upload_config(
    array(
        'allowed_types' => 'docx|pdf',
        'overwrite' => true,
        'encrypt_name' => true
    )
);
$iofiles->upload('userfile');
```

#### b. Proses Upload (_uploading_)
```php
$iofiles->upload([$field]);
```
`$field` adalah nama yang dikirim untuk input file (defaultnya: `'userfile'`). __Pastikan memanggil fungsi ini SETELAH__ `$iofiles->upload_config()`.

#### c. Mendapatkan parameter upload (__get params__)
```php
$iofiles->upload_get_param($paramName);
```
Beberapa informasi saat upload dapat dipanggil dengan menggunakan fungsi ini menggunakan nilai sebagai berikut:
* `'file_temp'` - nama file temp saat upload
* `'file_size'` - ukuran file yang diupload
* `'file_type'` - jenis file
* `'orig_name'` - nama file asli
* `'file_ext'` - ekstensi file
* `'file_name'` - nama hasil upload (terutama jika `encrypt_name` bernilai `true`

#### d. Mendapatkan error saat upload (_show error_)
```php
$iofiles->upload_show_error();
```

### - Operasi ZIP
#### a. Mampatkan file dengan ZIP (_zip compress_)
```php
$iofiles->zip_compress($file, [$zipName]);
```
`$file` bisa berisi _string_ (satu file) atau _array_ (beberapa file). Hasil zip akan diletakkan bersebelahan dengan file sumber.

#### b. Ekstraksi ZIP (_zip extraction_)
```php
$iofiles->zip_extract($zipfile, $destdir);
```

### - Operasi Gambar / Image
#### a. Konfigurasi (_configuration_)
```php
$iofiles->image_config($config);
```
Harus dipanggil sebelum proses operasi gambar dimulai. `$config` berisi array dengan nilai valid sebagai berikut:
* `'source_image'` - gambar yang akan dioperasikan
* `'new_image'` - nama file baru hasil operasi
* `'width'` - lebar baru gambar (untuk _resize_)
* `'height'` - tinggi baru gambar (untuk _resize_)
* `'quality'` - kualitas gambar (0-100)
* `'maintain_ratio'` - menjaga rasio gambar (true/false)
* `'rotation_angle'` - sudut rotasi (*hanya untuk rotasi gambar)
* `'wm_type'` - jenis watermark, (`'text'`, `'overlay'`)
* `'wm_use_drop_shadow'` - watermark dengan drop shadow (true/false) default false
* `'wm_use_truetype'` - watermark dengan font truetype (true/false) default false
* `'wm_text'` - teks untuk dijadikan watermark
* `'wm_overlay_path'` - path gambar untuk dijadikan watermark
* `'wm_font_path'` - path font jika menggunakan truetype
* `'wm_font_size'` - ukuran font watermark
* `'wm_vrt_alignment'` - letak watermark secara vertikal, (`'T'` untuk atas, `'M'` untuk tengah, dan `'B'` untuk bawah)
* `'wm_hor_alignment'` - letak watermark secara horizontal, (`'L'` untuk kiri, `'C'` untuk tengah, `'R'` untuk kanan)
* `'wm_font_color'` - warna font watermark dengan kode hexa, contoh `'#ffffff'`
* `'wm_shadow_color'` - warna shadow watermark, dengan kode hexa
* `'wm_shadow_distance'` - jarak shadow dengan teks asli
* `'wm_opacity'` - opasitas watermark (untuk gambar) dengan nilai 1-100
Contoh penggunaan yang valid adalah:
```php
$iofiles->image_config(
    array(
        'source_image' => 'yourimage.png',
        'maintain_ratio' => true,
        'wm_type' => 'text',
        'wm_text' => 'Copyright 2017'
    )
);
```
#### b. Memotong gambar (_crop_)
```php
$iofiles->image_crop();
// pastikan image_config sudah dipanggil
```

#### c. Mengubah ukuran gambar (_resize_)
```php
$iofiles->image_resize();
```

#### d. Memutar gambar (_rotate_)
```php
$iofiles->image_rotate();
```

#### e. Lakukan watermark (_image watermark_)
```php
$iofiles->image_watermark();
```

#### f. Efek berkaca (_image mirror_)
```php
$iofiles->image_mirror();
```

#### g. Dapatkan error saat proses (_show error_)
```php
$iofiles->image_show_error();
```

#### h. Bersihkan proses (_clear_)
```php
$iofiles->image_clear();
```
Untuk memastikan bahwa semua bersih maka panggil fungsi ini ketika proses selesai atau ketika akan melakukan proses operasi gambar lagi.


## Kredit? / Credits
Beberapa fungsi diadaptasi dari kode di [CodeIgniter](http://codeigniter.com)

## License / Lisensi
MIT
__Free Software__