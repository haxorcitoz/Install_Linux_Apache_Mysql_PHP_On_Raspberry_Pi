# Install_Linux_Apache_Mysql_PHP_On_Raspberry_Pi
Ini adalah cara (yang saya ikuti dan kebetulan berhasil) untuk menginstall LAMP (Linux Apache MySQL dan PHP) di Raspberry pi 4 menggunakan Raspberry Pi OS 64 bit

1. Hal pertama yang harus dilakukan adalah update dan upgrade dengan command `sudo apt update && sudo apt upgrade -y`
2. Lanjutkan dengan menginstall Apache2 dengan command `sudo apt install apache2 -y`
3. Setelah selesai instalasi cek folder `/var/www/html` dengan command `ls -la /var/www/html`. Apabila terdapat file `index.html` maka instalasi sudah benar
4. Selanjutnya lakukan test apakah server sudah berjalan. Untuk hal ini bisa dilakukan dengan membuka web browser dan mengetikkan `localhost` pada address bar atau apabila mengakses pi secara headless melalui putty atau ssh via cmd windows bisa dilakukan dengan mengetikkan ip addres dari pi tersebut. Untuk mengetahui ip address tersebut bisa dilakukan dengan cara mengetikkan command `hostname -I` setelah itu masukkan ip address tadi ke address bar browser dan apabila muncul halaman `Apache2 Debian Default Page` maka server sudah berhasil diinstal dan dijalankan.
5. Selanjutnya install php dengan command `sudo apt install php -y` dan apabila instalasi sukses lanjutkan dengan mengetikkan command `sudo service apache2 restart` untuk restart apache2.
6. Next install mysql (mariadb) dengan command `sudo apt install mariadb-server php-mysql -y` dan setelah terinstal ketikkan command `sudo service apache2 restart`
7. Selanjutnya tes apakah halaman phpmyadmin sudah bisa diakses. Caranya adalah buka web browser kemudian ketikkan `ip_addres_raspberryPi/phpmyadmin`. Apabila muncul halaman login silahkan coba masuk dengan `username = root` tanpa password
8. Apabila gagal maka kita harus buat dulu melalui terminal dengan menggunakan command `sudo mysql -u root -p` lalu enter dan akan muncul prompt untuk mengisi password. Karena menggunakan user `root` maka tidak perlu password, tinggal tekan enter saja.
9. Setelah itu buat user baru dengan command :
  ```
  create user admin@localhost identified by your_password;
  grant all privileges on *.* to admin@localhost;
  FLUSH PRIVILEGES;
  exit;
  ```
11. Kemudian install phpmyadmin dengan command `sudo apt install phpmyadmin -y`, lakukan konfigurasi menggunakan `dbconfig-common`
12. Setelah selesai konfigurasi lanjutkan dengan enable php module dengan menggunakan command `sudo phpenmod mysqli` kemudian restart apache dengan command `sudo service apache2 restart`
13. Apabila sudah selesai dan tidak ada error lanjutkan test untuk mengakses phpmyadmin melalui web browser dengan mengetikkan 'ip_address/phpmyadmin'
14. Jika tidak ada masalah maka tampilan halaman login phpmyadmin akan muncul, silahkan login menggunakan user dan password sudah dibuat pada langkah di atas
15. Apabila saat mengakses `ip_address/phpmyadmin` muncul error `The requested url /phpmyadmin was not found on this server` maka lakukan pemindahan folder `phpmyadmin` ke folder `/var/www/html` dengan command `sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin`
16. Done

Langkah opsional (namun direkomendasikan) : 
Untuk mengatur web pages maka direkomendasikan mengubah permissions dari folder `/var/www/html/` dengan cara :
```
ls -lh /var/www/
sudo chown -R pi:www-data /var/www/html/
sudo chmod -R 770 /var/www/html/
ls -lh /var/www/
```
Source : https://randomnerdtutorials.com/raspberry-pi-apache-mysql-php-lamp-server/
