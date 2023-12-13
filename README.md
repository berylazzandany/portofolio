# portofolio #
Tugas yang di buat pada final project kali ini adalah wordpress
****
## langkah langkah install wordpress di ubuntu 22.04 LST Linux ##
1. Lakukan pembaruan Ubuntu 22.04
   Pertama-tama, jalankan perintah pembaruan sistem untuk memastikan semua paket di sistem kita mutakhir dan juga cache indeks paket APT dalam keadaan terbaru.
   
          *sudo apt update && sudo apt upgrade*
   
2. Instal Apache & PHP untuk WordPress
   Kita memerlukan web server Apache dan bahasa pemrograman PHP untuk setting CMS WordPress, mari kita install keduanya pada langkah ini.

          * sudo apt install apache2 *

   Setelah instalasi Apache selesai, aktifkan dan mulai layanannya.

          * sudo systemctl enable apache2 *

   Periksa status :
  
          * systemctl status apache2 *

   Catatan : alamat IP server dengan alamat Anda yang sebenarnya

          * http://192.168.56.111 *

   Instal PHP versi 8

   Versi default PHP tersedia untuk diinstal menggunakan repositori standar Ubuntu 22.04 LTS. Oleh karena itu, cukup jalankan perintah yang diberikan untuk menginstal PHP dan ekstensi yang diperlukan di sistem Anda.

         * sudo apt install -y php php-{common,mysql,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl} *

   cek versi php setelah di install

         * php -v *

 3. Instal MariaDB atau MySQL
    Kita bisa menggunakan MariaDB atau MySQL Database Server di Ubuntu 22.04 untuk menyimpan data yang dihasilkan oleh CMS WordPress. Di sini kami menggunakan Server MariaDB.

        * sudo apt install mariadb-server mariadb-client *

    Aktifkan, Mulai dan periksa status layanan:

        * sudo systemctl enable --now mariadb *

    Memeriksa:

        * systemctl status mariadb *
     Ctrl+C untuk keluar.

    Amankan Instalasi Basis Data Anda:

    Untuk mengamankan instance Database kami, jalankan perintah yang diberikan:

        * sudo mysql_secure_installation *

    Keluaran  

  Pertanyaan yang diberikan akan ditanyakan oleh sistem, contoh jawabannya juga diberikan di bawah ini:

    Enter current password for root (enter for none): Press ENTER
    Set root password? [Y/n]: Y
    New password: Set-your-new-password
    Re-enter new password: Set-your-new-password
    Remove anonymous users? [Y/n] Y
    Disallow root login remotely? [Y/n] Y
    Remove test database and access to it? [Y/n] Y
    Reload privilege tables now? [Y/n] Y

4. Buat Database untuk WordPress
   Masuk ke server Database Anda dengan menggunakan kata sandi yang telah Anda tetapkan untuk pengguna root.

        * sudo mysql -u root -p *

   Ikuti perintah untuk membuat DB baru. Namun, jangan lupa untuk mengganti new_user dengan nama apa pun yang ingin Anda berikan kepada pengguna Database Anda dan dengan cara yang sama - new_db dengan nama untuk Database dan kata sandi_Anda untuk kata sandinya.

     CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'your_password';
        
     CREATE DATABASE new_db;
        
     GRANT ALL PRIVILEGES ON new_db.* TO 'new_user'@'localhost';
        
     FLUSH PRIVILEGES;
        
     Exit;

 5. Instal WordPress di Ubuntu 22.04
    File untuk mengatur WordPress perlu diunduh secara manual dan kita dapat melakukannya menggunakan terminal perintah. Berikut adalah perintah yang harus diikuti:

        * sudo apt install wget unzip *

    Unduh WordPress:

        * wget https://wordpress.org/latest.zip *

    Ekstrak berkas:

        * sudo unzip latest.zip *

    Pindahkan ke folder web: 

        * sudo mv wordpress/ /var/www/html/ *

    Hapus file yang diunduh untuk mengosongkan ruang:

        * sudo rm latest.zip *

    Ubah izin file

        * sudo chown www-data:www-data -R /var/www/html/wordpress/ *

        * sudo chmod -R 755 /var/www/html/wordpress/ *

6. Konfigurasikan Apache di Ubuntu 22.04
   Selanjutnya, aktifkan modul dan file konfigurasi Vhost server web Apache Anda untuk memastikan modul tersebut menyajikan file CMS PorcessWire tanpa kesalahan apa pun.

   Buat file konfigurasi untuk WordPress

       * sudo nano /etc/apache2/sites-available/wordpress.conf *

        <VirtualHost *:80>
        
        ServerAdmin admin@example.com
        
        DocumentRoot /var/www/html/wordpress
        ServerName example.com
        ServerAlias www.example.com
        
        <Directory /var/www/html/wordpress/>
        
        Options FollowSymLinks
        AllowOverride All
        Require all granted
        
        </Directory>
        
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        
        </VirtualHost>

    Simpan file dengan menekan Ctlr+O , menekan tombol Enter , lalu keluar menggunakan Ctrl+X .

    Aktifkan host virtual

        * sudo a2ensite wordpress.conf *

    Aktifkan modul penulisan ulang

        * sudo a2enmod rewrite *

    Nonaktifkan halaman pengujian Apache default

        * sudo a2dissite 000-default.conf *

    Mulai ulang server web Apache untuk menerapkan perubahan:

        * sudo systemctl restart apache2 *

7. Pengaturan antarmuka web CMS WordPress

       http://192.168.56.111

## langkah langkah install bind9 ##
   bind9 disini berguan untuk mengganti pemanggilang website menggunakan ip menjadi azzandany.com
   
Langkah 1: Instalasi BIND9

Buka terminal dan jalankan perintah berikut untuk menginstal BIND9:

bash

    sudo apt-get update
    sudo apt-get install bind9

Langkah 2: Konfigurasi BIND9

  Buka file konfigurasi BIND:

    sudo nano /etc/bind/named.conf.options

    options {
        directory "/var/cache/bind";
        forwarders {
            8.8.8.8;
            8.8.4.4;
        };
        dnssec-validation auto;
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };

Tekan Ctrl + X, lalu Y untuk menyimpan perubahan.

Buka file konfigurasi zona BIND:

    sudo nano /etc/bind/named.conf.local

Tambahkan konfigurasi zona untuk azzandany.com:

    zone "azzandany.com" {
        type master;
        file "/etc/bind/zones/db.azzandany.com";
    };

Tekan Ctrl + X, lalu Y untuk menyimpan perubahan.

Buat direktori untuk file zona:

    sudo mkdir /etc/bind/zones

Buat dan konfigurasikan file zona untuk domain (db.azzandany.com):

    sudo nano /etc/bind/zones/db.azzandany.com

Isi file tersebut dengan konfigurasi yang sesuai:

    $TTL    604800
    @       IN      SOA     ns1.azzandany.com. admin.azzandany.com. (
                          2021121101 ; Serial
                          604800     ; Refresh
                          86400      ; Retry
                          2419200    ; Expire
                          604800 )   ; Negative Cache TTL

    @       IN      NS      ns1.azzandany.com.
    @       IN      A       192.168.56.111
    ns1     IN      A       192.168.56.111

    Tekan Ctrl + X, lalu Y untuk menyimpan perubahan.

Langkah 3: Restart dan Uji BIND9

  Restart BIND9:

    sudo service bind9 restart

Uji konfigurasi BIND9:

    sudo named-checkconf
    sudo named-checkzone azzandany.com /etc/bind/zones/db.azzandany.com

    Jika tidak ada pesan kesalahan, konfigurasi Anda mungkin sudah benar.

Langkah 4: Pengaturan DNS di Ubuntu

Buka file /etc/resolv.conf dan tambahkan alamat IP DNS server lokal:

    sudo nano /etc/resolv.conf

Tambahkan baris berikut:

    nameserver 192.168.56.111

Simpan perubahan dan tutup editor.
   
































    
































    
   
