<h1 align="center"><img src="https://esotalk.org/img/blog/9-introducing-flarum/flarum-logo.png"></h1>

# Flarum

## Sekilas Tentang

**Flarum** adalah aplikasi web mengenai *social network* dan forum yang gratis dan dapat dihost secara lokal. Sebagai forum online, aplikasi ini memiliki beberapa kelebihan, yaitu desain UI yang elegan, cepat & ringan, teroptimasi untuk semua perangkat, dan lain lain.



## Instalasi

#### Kebutuhan Sistem :
- Unix, Linux atau Windows.
- Apache (dengan mod_rewrite enabled) atau Nginx
- PHP 7.1+ dengan ekstensi tambahan: dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer
- MySQL 5.6+ atau MariaDB 10.0.5+ 

#### Sistem Yang Digunakan :
- Xubuntu 18.04 Bionic Beaver
- Apache2 (dengan mod_rewrite enabled)
- PHP 7.2 ditambah ekstensi: mbstring, xmlrpc, soap, gd, xml, mysql, cli, mcrypt, zip, curl, dom.
- MySQL 5.7.24
- Ubuntu Virtual Server dari labkom
- SSH (command-line) untuk menjalankan Composer dan lain-lain

#### Proses Instalasi :
1. Login kedalam server menggunakan SSH.
    ```
    $ ssh student@localhost -p 2222
    ```

2. Update repo update sistem dan install kebutuhan-kebutuhan aplikasi web.
    ```
    $ sudo apt-get update
    $ sudo apt-get install software-properties-common
    ```
   Tambahkan repository php
    ```
    $ sudo add-apt-repository ppa:ondrej/php
    ```
   Update repo kembali
    ```
    $ sudo apt-get update
    ```
   Install `apache2`, `mysql-server` serta `mysql-client`, dan `composer`
    ```
    $ sudo apt-get install apache2 mysql-server mysql-client composer
    ```
   Install ekstensi-ekstensi php yang dibutuhkan
    ```
    $ sudo apt install sudo apt install -y php7.2 libapache2-mod-php7.2 php7.2-common php7.2-mbstring 
      php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-mysql php7.2-cli php7.0-mcrypt php7.2-zip 
      php7.2-curl php7.2-dom composer openssl
   
3. Pindah ke direktori /var/www/html/ dan instalasi aplikasi webserver localhost dengan menggunakan composer
    ```
    $ cd /var/www/html/
    $ sudo composer create-project flarum/flarum . --stability=beta
    ```

5. Ubah hak kepemilikan direktori ke user www-data (webserver) 
    ```
    $ sudo chown www-data:www-data -R ../html
    ```

6. Buat database dan user aplikasi web.
    ```
    $ mysql -uroot -e "create database testdb"
    $ mysql -uroot -e "create user testuser"
    $ mysql -uroot -e "grant all privileges on testdb.* to 'testuser'@'localhost' identified by 'mypassword'"
    $ mysql -uroot -e "flush privileges"
    ```

7. Konfigurasi Apache web server agar aplikasi web tidak mendapat 500 Internal Server Error.
    ```
    $ sudo a2enmod rewrite
    $ sudo nano /etc/apache2/sites-available/000-default.conf

    <VirtualHost *:80>
      ServerAdmin admin@your-domain.com
      DocumentRoot /var/www/html/flarum
      ServerName your-domain.com
      ServerAlias www.your-domain.com
      <Directory /var/www/html/flarum/>
        Options FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
      </Directory>
    ErrorLog /var/log/apache2/your-domain.com-error_log
    CustomLog /var/log/apache2/your-domain.com-access_log common
    </VirtualHost>
    ```

9. Restart Apache web server.
    ```
    $ sudo systemctl apache2 restart
    ```

10. Buka web server pada browser untuk instalasi aplikasi web. `localhost:8888`

    - Isi tabel dengan konfigurasi database yang sudah dibuat tadi
      ![install flarum](http://i.imgur.com/bYNCCQr.png)



## Konfigurasi

- Tampilan awal setelah instalasi forum berhasil.

  ![Homepage](https://i.imgur.com/LWPVj0a.png)

- Menu drop down ketika klik user, lalu ke administration.

  ![Menu](https://i.imgur.com/DvMOhkW.png)
 
- Tampilan page administration yang dapat diakses oleh admin.

  Menu **Dashboard** menampilkan overview forum dan aktivitas forum.

  ![Administration Page](http://imgur.com/agRF5gn.png)
  
- Konfigurasi **Basics** untuk mengubah judul forum, deskripsi forum, dan welcome banner.

  ![Basics](http://imgur.com/hou9RLt.png)
  
- Konfigurasi **Email** untuk mengatur setting SMTP server forum untuk mengirim email konfirmasi akun
  dan lain-lain.
  (namun untuk saat ini karena aplikasi web dihost secara local, pengiriman email dari aplikasi web tidak dapat dilakukan)
  
  ![Email](http://imgur.com/r7QBTPY.png)

- Konfigurasi **Permissions** di mana admin dapat mengatur apa yang dapat dan tidak dapat dilakukan, dilihat, dirubah oleh user, moderator, atau admin dapat menambahkan kategori group baru dengan fitur **(+) New Group**.

  ![Permissions](http://i.imgur.com/GxdlEv3.png)
  
- Konfigurasi **Appearance** untuk mengatur warna forum, logo, dan lain-lainnya. Warna forum dapat diubah dengan kode hex, terdapat dark mode yang merubah tampilan forum menjadi gelap seperti yang terlihat di gambar. Header dan Footer forum dapat dirubah dengan menggunakan custom header / footer sendiri, tampilan forum pun dapat dirombak dengan menggunakan custom CSS sendiri.

  ![Appearance](http://i.imgur.com/o8poBO6.png)
  
- Konfigurasi **Extensions** di mana ekstensi tambahan untuk forum dapat diaktifkan atau nonaktifkan.

  ![Extensions](http://i.imgur.com/8pmMGLb.png)
  
  Terdapat ekstensi tambahan **Facebook Login** dan **Twitter Login** yang membolehkan user untuk register dan masuk ke dalam forum dengan akun Facebook atau Twitter user, namun kedua ekstensi ini membutuhkan API ID/API Key dari masing-masing media sosial tersebut untuk dapat bekerja.
  
  ![Facebook](http://i.imgur.com/Ksk6Po0.png)
  
  ![Twitter](http://i.imgur.com/fDBuyye.png)
  
  ![Login](http://i.imgur.com/aUrbHjZ.png)
  
- Konfigurasi **Tags** untuk mengatur / membuat tags yang digunakan untuk mengorganisir postingan dalam forum diskusi.

  ![Tags](http://i.imgur.com/3SgUkhr.png)
  
  Pembuatan tag baru dengan **Create Tag**
  
  ![Create new tag](http://i.imgur.com/qsqJ1xU.png)
  
  Tag baru muncul di homepage
  
  ![tag baru](http://i.imgur.com/BczkubO.png)



## Cara Pemakaian

  Penggunaan forum diskusi flarum sangat mudah dengan *user interface* yang mudah dimengerti, agar dapat memulai diskusi, user harus login dengan menggunakan akun yang dibuat pada forum.
  
  ![Login](http://i.imgur.com/aUrbHjZ.png)
  
  Setelah login, klik tombol **Start a discussion** untuk membuat thread diskusi baru.
  
  ![homepage](https://i.imgur.com/LWPVj0a.png)
  
  ![Start a discussion](http://i.imgur.com/nffv8vK.png)
  
  User dapat memilih tags untuk thread yang akan dibuat, `tag baru` akan digunakan untuk thread ini.
  
  ![Choose tags](http://i.imgur.com/VEkGcqS.png)
  
  ![Post-ready](http://i.imgur.com/1e2fnCR.png)
  
  Tampilan diskusi forum
  
  ![foo](http://i.imgur.com/Tity9vU.png)
  
  Ketika tombol *drop-down* **Reply** diklik, maka akan muncul pilihan **Rename**, **Sticky**, **Edit Tags**, dan **Delete**. Ke-empat fungsi ini hanya bisa dilakukan oleh pemilik thread.
  
  ![Reply dropdown menu](http://i.imgur.com/gSJQmue.png)
  
  **Rename** berfungsi untuk merubah judul thread diskusi
  
  ![Rename](http://i.imgur.com/Z6iihmP.png)
  
  **Sticky** berfungsi untuk menempelkan thread diskusi yang akan membuat thread muncul di paling atas list thread diskusi pada *homepage*
  
  **Edit Tags** berfungsi untuk merubah atau menambahkan tag pada thread diskusi tersebut.
  ![Edit Tags](http://i.imgur.com/fEpeziJ.png)
  
  **Delete** berfungsi untuk menghapus thread diskusi
  
  
  Akan dibuat thread diskusi baru dengan tag lain bernama `tag lain`
  
  ![foo2](http://i.imgur.com/wcWMlo8.png)
  
  Kedua thread akan muncul pada *homepage* jika melihat *homepage* dengan **All dicsussions**.
  
  ![homepage](http://i.imgur.com/WUJ0m4U.png)
  
  Namun jika **tag baru** diklik, maka thread yang akan muncul hanya thread **foo** dengan tag `tag baru`, dan jika **tag lain** diklik, maka hanya thread dengan tag `tag lain` yang akan muncul yaitu **foo2**.
  
  ![tag baru](http://i.imgur.com/i46UPqF.png)
  
  ![tag lain](http://i.imgur.com/x0rbylP.png)

## Pembahasan

  Pros:
  - UI yang cukup menarik dan mudah untuk dipahami.
  - Loading time yang cepat.
  - Memiliki fitur yang lumayan banyak.
  - Sangat *customizeable*
  - Mudah untuk dikembangkan karena merupakan situs open source
  
  Cons:
  - Meskipun mudah untuk dikembangkan tetapi lama untuk pengembangannya
  - Banyak *bugs*
  - Masih dalam tahap produksi beta dan tidak stabil
  - Tidak berguna jika digunakan secara lokal atau *self local hosted*

## Dokumentasi

*to be added*

## Referensi

1. [Flarum](https:/flarum.org/)
2. [Flarum Documentations](https://flarum.org/docs/)
3. [How to Install Flarum on Ubuntu 18.04](https://www.howtoforge.com/tutorial/ubuntu-flarum/)
4. [Flarum Forum DIscussion](https://discuss.flarum.org/)
5. [Flarum Github](https://github.com/flarum/core/)

