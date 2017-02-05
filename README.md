# IOFiles
Menghandle beberapa hal umum yang berhubungan dengan file seperti save, read, move, upload, force download, zip, extract, resize image, crop image, watermark, dsb.  Tidak semua fungsi dalam Filesystem PHP ada dalam class ini.

## Tabel Isi / Table of Contents


## Instalasi / Installation
Unduh file dalam folder scr atau gunakan composer dengan perintah:
```sh
$ composer require nazir/iofiles
```

## Penggunaan / Usage
Beberapa public method yang dapat digunakan adalah sebagai berikut:
#### `copy($file, $destination)` - menggandakan file

#### `delete($file)` - menghapus file

#### `download($file)` - memaksa browser mendownload file

#### `get_attrib($file, [$flags])` - lihat properti file

#### `get_type($file)` - file extension

## Kredit? / Credits
Beberapa fungsi diadaptasi dari kode di [CodeIgniter](http://codeigniter.com)

## License / Lisensi
MIT
__Free Software__