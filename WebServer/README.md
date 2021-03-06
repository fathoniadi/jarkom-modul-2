# 2. Web Server
Web Server adalah perangkat yang menyediakan layanan akses kepada pengguna melalui protokol HTTP atau HTTPS melalui aplikasi web.

### 2.1 Instalasi apache2
Install Web Server Apache pada PUCANG yang sebelumnya sudah diupdate dengan mengetikkan __apt-get install apache2__. Setelah proses instalasi selesai, coba akses __IP PUCANG TIAP KELOMPOK__ pada browser maka akan muncul seperti gambar di bawah:

![Pucang1](images/01.png)

### 2.2 Instalasi PHP pada PUCANG
Supaya web server dapat menjalankan perintah PHP, maka kita perlu menginstall PHP pada PUCANG dengan mengetikkan __apt-get install php__

![Pucang2](images/02.png)

Setelah proses instalasi selesai, untuk mengetahui apakah instalasi PHP berhasil atau tidak, silahkan membuat file PHP yang diletakkan di __/var/www/html__
```
cd /var/www/html
nano ini.php
```

Kemudian isikan pada file __ini.php__:
```
<?php phpinfo(); ?>
```

Setelah selesai membuat file __ini.php__, untuk mencoba membukanya akses dengan __IP PUCANG TIAP KELOMPOK/ini.php__ maka akan muncul seperti gambar di bawah:

![Pucang3](images/03.png)

Untuk membuat sebuah halaman atau website, kita dapat menambahkannya pada direktori __/var/www/html__. Langkah-langkahnya sama seperti saat membuat halaman __ini.php__.

Sekarang bayangkan ketika kita ingin mengakses sebuah halaman web kita harus mengetikkan alamat IPnya terlebih dahulu. Ribet kan? Maka kita akan membuatnya menjadi mudah, silahkan lakukan konfigurasi sehingga ketika kita mengakses __pucang.com__ akan diarahkan ke halaman web yang sudah kita buat. 

![Pucang4](images/04.png)

Jika lupa caranya, silahkan buka lagi modul sebelumnya yang membahas tentang [DNS](https://github.com/rohanaq/jarkom2/tree/master/DNS).

### 2.3 Mengubah Direktori Default
Cobalah membuat sebuah halaman web di luar direktori __/var/www/html__ sebagai contoh kita akan membuat halaman __coba.php__ pada direktori __/var/www__. Maka ketika diakses akan muncul seperti gambar di bawah:

![Pucang5](images/05.png)

Tulisan yang ditampilkan pada browser adalah __NOT FOUND__, karena secara default web server apache akan mengakses file-file yang ada pada direktori __/var/www/html__. Namun halaman __coba.php__ tetap bisa diakses dengan cara mengubah direktori default dengan mengetikkan:
```
nano /etc/apache2/sites-enabled/000-default.conf
```

Setelah itu ubah __DocumentRoot__ menjadi __/var/www/__ seperti gambar di bawah ini:

![Pucang6](images/06.png)

Setelah itu restart apache2 dengan perintah `service apache2 restart` dan coba akses kembali halaman __coba.php__ pada browser kalian.

### 2.4 Konfigurasi VirtualHost
Sekarang kita akan mengkonfigurasi __VirtualHost__ untuk bisa mengakses web server. Pada PUCANG lakukan copy file terhadap file konfigurasi bawaan dari apache2. Setelah menyalin file konfigurasi bukalah dengan perintah __nano__
```
cd /etc/apache2
cp sites-available/000-default.conf sites-available/default2.conf
nano sites-available/default2.conf
```

Lalu silahkan ganti port pada __VirtualHost__ menjadi 8080, dan ubah __DocumentRoot__ menjadi /var/www/8080 dan tambahkan sintaks seperti di bawah ini:
```
<VirtualHost *:8080>
```

```
ServerAdmin webmaster@localhost
DocumentRoot /var/www/8080

<Directory /var/www/8080>
AllowOverride All
</Directory>
```

![Pucang7](images/07.png)

Lalu tambahkan port 8080 dengan mengedit file __ports.conf__ yang ada pada __/etc/apache2__. Tambahkan dengan sintaks `listen 8080`

![Pucang8](images/08.png)

Kemudian pada folder __/var/www__ buatlah folder 8080 dan setelah itu buatlah file index.php pada folder tersebut
```
cd /var/www
mkdir 8080
nano 8080/index.php
```

Lalu pada file index.php tersebut isikan tulisan seperti di bawah ini:
```html
<!DOCTYPE html>
<html>
<head>
    <title>JARKOM 2018</title>
</head>
<body>
    <h3>INI PORT 8080</h3>
</body>
</html>
```

Lalu untuk menjalankannya ketikkan `a2ensite default2.conf` dan kemudian restart apache2 dengan perintah `service apache2 restart`. Setelah itu pada browser silahkan akses website kalian dengan menggunakan __IP PUCANG TIAP KELOMPOK__ atau dengan DNS yang sudah dibuat, lalu tambahkan port 8080 pada akhir penulisannya. Contoh -> __10.151.73.235:8080__ atau __pucang.com:8080__. Maka akan muncul seperti gambar di bawah:

![Pucang9](images/09.png)

### 2.5 Alias Directory
Alias directory adalah membuat direktori lebih mudah diakses tetapi tetap di bawah DocumentRoot. Silahkan masuk ke dalam __/etc/apache2__ dan salinlah file __000-default.conf__ yang ada pada folder __sites-available__ dan ubah namanya menjadi __alias.conf__. Langkah-langkahnya sama dengan ketika membuat file __default.conf__ pada poin sebelumnya. Kemudian buka __alias.conf__ dan tambahkan sintaks seperti gambar di bawah:

![Pucang10](images/10.png)

Setelah itu buatlah direktori __/alias/test/jarkom/__ dengan mengetikkan
```
mkdir /var/www/alias
mkdir /var/www/alias/test
mkdir /var/www/alias/test/jarkom
```

Kemudian pada direktori __/var/www/alias/test/jarkom__ buatlah sebuah file bernama __jarkom.php__. Sebelum mengaktifkan konfigurasi __alias.conf__, pastikan konfigurasi yang lain sudah dimatikan. Karena konfigurasi yang baru kita coba adalah __default2.conf__ maka matikan konfigurasi tersebut dengan perintah `a2dissite default2.conf` kemudian restart apache2 dengan `service apache2 restart`. Setelah itu baru kita aktifkan konfigurasi __alias.conf__ dengan perintah `a2ensite alias.conf` dan kemudian restart apache2 `service apache2 restart`.

Sekarang coba buka pada browser __IP PUCANG TIAP KELOMPOK/jarkom__ atau __pucang.com/jarkom__ (sesuai dengan DNS yang sudah dibuat masing-masing kelompok)

![Pucang11](images/11.png)

Lalu apa perbedaannya ketika kita mengakses __/test/jarkom__?
![Pucang12](images/12.png)

### 2.6 Directory Listing