# Jarkom-Modul-2-B05-2022

## Anggota 
Nama            | NRP
-------------   | -------------
Helsa Nesta Dhaifullah  | 5025201005
Achmad Nashruddin Riskynanda    | 5025201021
Haniif Ahmad Jauhari  | 5025201224

## Soal dan Pembahasan
### No 1
* Membuat Topologi Negara Ostania dengan WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet. <br>
![image](https://user-images.githubusercontent.com/70515589/198552348-dbf1de96-5da0-466b-befa-0440a8d4066f.png)
 * Masing-masing node dapat mengakses internet, hal ini ditunjukkan dengan melakukan ping ke google.com
   * WISE <br>
![image](https://user-images.githubusercontent.com/70515589/198553459-320a7c37-d9b8-435e-99a1-fedd12a5afc2.png)
   * SSS <br>
![image](https://user-images.githubusercontent.com/70515589/198553855-cf41e012-80a0-4094-a0b7-f21afa13aa25.png)
   * Garden <br>
![image](https://user-images.githubusercontent.com/70515589/198554033-2f7aa6b4-c840-4881-b61f-d5e3e06fd973.png)
   * Berlint <br>
![image](https://user-images.githubusercontent.com/70515589/198554197-b542bfd5-d5e6-4bd8-ae16-3dd6c9ee3ece.png)
   * Eden <br>
![image](https://user-images.githubusercontent.com/70515589/198554365-62ea0a76-86c7-4502-8d05-e3e45ef11c8b.png)

### No 2
* Membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise <br>
* Menambahkan zone "wise.B05.com" ke dalam folder `/etc/bind/named.conf.local`
```
echo ‘zone "wise.B05.com" {
type master;
file "/etc/bind/wise/wise.B05.com";
};
``` 
* Membuat folder wise pada `/etc/bind/` dengan command `mkdir /etc/bind/wise`, kemudian membuat file config wise.B05.com <br>
![image](https://user-images.githubusercontent.com/70515589/198555240-90cc081e-9372-4d86-b908-0a9c630d4ef0.png)
* Memodifikasi config pada file `/etc/bind/wise/wise.B05.com`. Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada wise.B05.com, kemudian domain dipetakan pada IP WISE yaitu 192.175.3.2. Untuk membuat alias www.wise.B05.com menggunakan CNAME. Berikut adalah modifikasi config yang telah dilakukan: <br>
![image](https://user-images.githubusercontent.com/70515589/198555942-f5b7ccfc-5fce-4f3b-8af9-7418b534a1bf.png)

### No 3
* Membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden <br>
* Memodifikasi config pada file `/etc/bind//wise/wise.B05.com`. Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada eden.wise.B05.com, kemudian domain dipetakan pada IP Eden yaitu 192.175.2.3. Untuk membuat alias www.eden.wise.B05.com menggunakan CNAME. Berikut adalah modifikasi config yang telah dilakukan: <br>
![image](https://user-images.githubusercontent.com/70515589/198556488-ddba254d-618b-4a53-ad53-1578750acdc1.png)
* Test melakukan ping eden.wise.B05.com dan www.eden.wise.B05.com pada SSS / Garden. Dapat terlihat bahwa ip mengarah pada Eden. <br>
![image](https://user-images.githubusercontent.com/70515589/198557779-613aaeca-6847-4bf5-9683-e852e98c8bb2.png)

### No 4
* Membuat reverse domain untuk domain utama <br>
* Menambahkan 3.175.192.in-addr.arpa pada file `/etc/bind/named.conf.local` <br>
![image](https://user-images.githubusercontent.com/70515589/198558241-5107cfa9-fc16-4d15-84eb-370fa56597ed.png)
* Membuat file config `3.175.192.in-addr.arpa` pada folder wise <br>
![image](https://user-images.githubusercontent.com/70515589/198558504-e9ff901e-bbba-4436-9fa9-b2d8b505b2cf.png)
* Memodifikasi config pada file  `/etc/bind/wise/3.175.192.in-addr.arpa. Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada 3.175.192.in-addr.arpa dengan domain wise.B05.com. Menggunakan PTR untuk mereverse domain dengan 2 sebagai byte keempat dari IP WISE. Berikut adalah modifikasi config yang telah dilakukan: <br>
![image](https://user-images.githubusercontent.com/70515589/198558968-ccc6fee0-a81b-43e6-b4a1-212dd09f5d44.png)
* Testing konfigurasi dengan host -t PTR 192.175.3.2 pada SSS / Garden <br>
![image](https://user-images.githubusercontent.com/70515589/198559169-a95900b8-7c4b-455b-b01f-a3744e65e476.png)

### No 5
* Buat Berlint sebagai DNS Slave untuk domain utama <br>
* Memodifikasi zone wise.B05.com pada `/etc/named.conf.local` WISE dengan menambahkan notify kepada IP Berlint sebagai DNS slave. <br>
![image](https://user-images.githubusercontent.com/70515589/198560299-756cd438-62d9-4011-b208-f4e5a11a6d2c.png)
* Menambahkan zone wise.B05.com pada `/etc/named.conf.local` pada Berlint sebagai DNS Slave dari WISE <br>
![image](https://user-images.githubusercontent.com/70515589/198560491-5013527b-5ac9-4976-97dc-9570a7ba51bf.png)
* Testing ping wise.B05.com dengan mematikan service bind9 WISE <br>
![image](https://user-images.githubusercontent.com/70515589/198560854-e8a887a4-bb95-4fcf-8640-ac8f192512cd.png)
* SSS / Garden sebagai client dapat melakukan ping ke wise.B05.com <br>
![image](https://user-images.githubusercontent.com/70515589/198561039-4fdcf61c-71fb-4616-8777-2dd90c78e6c1.png)

### No 6
* Buat subdomain operation.wise.B05.com dengan alias www.operation.wise.B05.com yang didelegasikan dari Wise ke Berlint dengan IP menuju ke Eden dalam folder Operation <br>
* Pada WISE
  * Memodifikasi config pada file `/etc/bind/wise/3.175.192.in-addr.arpa` dengan membuat subdomain operation.wise.B05.com dan domain alias www.operation.wise.B05.com. Kemudian subdomain dan aliasnya didelegasikan ke Berlint dengan ns1 dengan mengarahkan ke IP Berlint yaitu 192.175.2.2. Berikut adalah modifikasi config yang telah dilakukan: <br>
![image](https://user-images.githubusercontent.com/70515589/198564616-6e518134-bcdb-4caa-ba0a-f3a78f919775.png)
  * Edit file `/etc/bind/named.conf.options` dengan menambahkan `allow-query{any;};` dan hapus `dnssec-validation auto;` <br>
![image](https://user-images.githubusercontent.com/70515589/198565127-5235afa9-2da7-4e63-a1aa-df82cecd5892.png)
  * Menambahkan allow-transfer { 192.175.2.2; } pada file `/etc/bind/named.conf.local` sehingga zone wise.B05.com menjadi <br>
![image](https://user-images.githubusercontent.com/70515589/198565617-d9397d8a-8a07-464a-95b9-80fc3834301a.png)
* Pada Berlint
  * Edit file `/etc/bind/named.conf.options` dengan menambahkan `allow-query{any;};` dan hapus `dnssec-validation auto;` <br>
  ![image](https://user-images.githubusercontent.com/70515589/198565894-7ea2cfc3-3fb5-4e97-ac2a-2a63eae9ac89.png)
  * Menambahkan zone operation.wise.B05.com pada `/etc/bind/named.conf.local` sebagai berikut <br>
  ![image](https://user-images.githubusercontent.com/70515589/198566050-436a71b1-facd-40d1-a2d2-7a98f9e09cd2.png)
  * Membuat folder operation pada `/etc/bind/` dengan command `mkdir /etc/bind/operation`, kemudian membuat file config operation.wise.B05.com <br>
  ![image](https://user-images.githubusercontent.com/70515589/198566245-73924a56-dc82-4f78-9c26-a5d31b1a8d8a.png)
  * Memodifikasi config pada file `/etc/bind/operation/operation.wise.B05.com`. Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada operation.wise.B05.com, kemudian domain dipetakan pada IP Eden yaitu 192.175.2.3. Untuk membuat alias www.operation.wise.B05.com menggunakan CNAME yang juga mengarah ke IP Eden. Berikut adalah modifikasi config yang telah dilakukan: <br>
  ![image](https://user-images.githubusercontent.com/70515589/198566693-affae751-7210-4c7f-9227-e2317f3989fa.png)
* Testing melakukan ping operation.wise.B05.com dan www.operation.wise.B05.com pada SSS / Garden. Dapat terlihat bahwa IP mengarah pada IP Eden. <br>
![image](https://user-images.githubusercontent.com/70515589/198566997-12d3a827-0acf-4fba-b390-d58a14c51aec.png)

### No 7
* Buat subdomain melalui Berlint dengan nama strix.operation.wise.B05.com dengan alias www.strix.operation.wise.B05.com yang mengarah ke Eden <br>
* Memodifikasi config pada file `/etc/bind/operation/operation.wise.B05.com.` Membuat subdomain strix.operation.wise.B05.com dengan alias www.strix.operation.wise.B05.com. IP subdomain ini juga mengarah ke IP Eden. Berikut adalah modifikasi config yang telah dilakukan: <br>
![image](https://user-images.githubusercontent.com/70515589/198567407-e4e22101-7488-4f23-a393-34f915c42a26.png)
* Testing melakukan ping strix.operation.wise.B05.com dan www.strix.operation.wise.B05.com pada SSS / Garden. Dapat terlihat bahwa IP mengarah pada IP Eden. <br>
![image](https://user-images.githubusercontent.com/70515589/198567589-2f5084ce-7285-4d64-a4fe-1be2021531dc.png)

### No 8
* Pada WISE, kita buat konfigurasi PTR dari IP Eden yang mengarah ke `wise.B05.com` dengan melakukan copy `cp /etc/bind/wise/3.175.192.in-addr.arpa 
/etc/bind/wise/2.175.192.in-addr.arpa`

* Kemudian edit menggunakan `nano /etc/bind/wise/2.175.192.in-addr.arpa` agar menjadi seperti berikut.
```
;
; BIND data file for local loopback interface
;
$TTL 604800
@     IN     SOA    wise.B05.com.   root.wise.B05.com. (
                        2           ; Serial
                        604800      ; Refresh
                        86400       ; Retry
                        2419200     ; Expire
                        604800 )    ; Negative Cache TTL
;
2.175.192.in-addr.arpa. IN    NS     wise.B05.com.
3                       IN    PTR    wise.B05.com.
```
* Kemudian, tambahkan `zone "2.175.192.in-addr.arpa"` ke dalam `/etc/bind/named.conf.local` seperti berikut.
```
zone "2.175.192.in-addr.arpa" {
 type master;
 file "/etc/bind/wise/2.175.192.in-addr.arpa";
};
```

* Ubah konfigurasi `wise.B05.com` agar mengarah ke IP Eden seperti berikut.
```
;
; BIND data file for local loopback interface
;
$TTL 604800
@   IN  SOA     wise.B05.com.   root.wise.B05.com. (
                        2           ; Serial
                        604800      ; Refresh
                        86400       ; Retry
                        2419200     ; Expire
                        604800 )    ; Negative Cache TTL
;
@           IN      NS          wise.B05.com.
@           IN      A           192.175.2.3
www         IN      CNAME       wise.B05.com.
eden        IN      A           192.175.2.3
www.eden    IN      CNAME       eden.wise.B05.com.
ns1         IN      A           192.175.2.3
operation   IN      NS          ns1
@           IN      AAAA        ::1
```

* Restart bind9 dengan `service bind9 restart`

* Pada Eden, install semua package yang akan diperlukan.
```
apt-get update
apt-get install apache2
service apache2 start
apt-get install php
apt-get install libapache2-mod-php7.0
apt-get install wget -y
apt-get install unzip -y
```

* Lalu, buat konfigurasi site `wise.B05.com` pada apache2 dengan mengcopy `cp /etc/apache2/sites-available/000-default.conf 
/etc/apache2/sites-available/wise.B05.com.conf`

* Edit konfigurasi site `wise.B05.com` dengan `nano /etc/apache2/sites-available/wise.B05.com.conf` seperti berikut.
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/wise.B05.com
    ServerName wise.B05.com
    ServerAlias www.wise.B05.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

* Buat direktori `/var/www/wise.B05.com` dan download resource serta unzip dan pindahkan isinya ke direktori tersebut.
```
mkdir /var/www/wise.B05.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1S0XhL9ViYN7TyCj2W66BNEXQD2AAAw2e' -O /var/www/wise.zip
unzip /var/www/wise.zip -d /var/www
cp /var/www/wise/* /var/www/wise.B05.com
```

* Aktifkan site dengan command a2ensite dan restart serta reload apache2.
```
a2ensite wise.B05.com
service apache2 restart
service apache2 reload
```

* Pada SSS dan Garden, ubah nameserver pada `/etc/resolv.conf` agar mengarah ke IP WISE dan buka site `www.wise.B05.com` menggunakan lynx.
```
echo “nameserver 192.175.3.2” > /etc/resolv.conf
lynx www.wise.B05.com
```
![image8](https://user-images.githubusercontent.com/100585249/198834821-0d7edfeb-c19c-4c4f-b5ff-8b2a65fe711a.PNG)
### No 9
* Aktifkan modul rewrite dengan command `a2enmod rewrite`
* Buat konfigurasi file `.htaccess` dengan menggunakan `nano /var/www/wise.B05.com/.htaccess` seperti berikut.
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule (.*) /index.php/\$1 [L]
```
* Tambahkan potongan kode berikut pada konfigurasi site `wise.B05.com` di `/etc/apache2/sites-available/wise.B05.com.conf`.
```
<Directory /var/www/wise.B05.com>
    Options +FollowSymLinks -Multiviews
    AllowOverride All
</Directory>
 ```
* Restart apache2 dengan command `service apache2 restart`
![image8](https://user-images.githubusercontent.com/100585249/198834838-d9263992-a57f-451f-a0ce-33dcabcbba6d.PNG)
### No 10
* Pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com
* PADA EDEN
  * Copy file `/etc/apache2/sites-available/000-default.conf` menjadi `/etc/apache2/sites-available/eden.wise.B05.com.conf` untuk mengambil default confignya
  * Modifikasi config baru `eden.wise.b05.conf`dengan isian :
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.B05.com
    ServerName eden.wise.B05.com
    ServerAlias www.eden.wise.B05.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  * Buat directory root dengan cara `mkdir /var/www/eden.wise.B05.com`
  * Download resource yang telah diberikan
  ```
  wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV' -O /var/www/eden.wise.zip
  ```
  * Extract file dengan `unzip /var/www/eden.wise.zip -d /var/www`
  * Pindahkan sesuai dengan directory yang diinginkan
  ```
  cp /var/www/eden.wise/* /var/www/eden.wise.B05.com
  ```
  * Enable situs eden.wise dengan mengaktifkan config pada apache2 dengan cara `a2ensite eden.wise.b05.com`
  * Restart atau Reload Apache2 dengan `service apache2 reload`
* SSS & Garden
  * lynx www.eden.wise.b05.com
  * screenshot :
  ![Screenshot_2022-10-29_14_46_28](https://user-images.githubusercontent.com/91010605/198820368-637e6076-cb8b-41bd-8233-3b708fc0ca45.png)

### No 11
* Pada folder /public, Loid ingin hanya dapat melakukan directory listing saja
* Pada Eden
  * Tambahkan aturan Directory pada `/etc/apache2/sites-available/eden.wise.B05.com.conf`
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.B05.com
    ServerName eden.wise.B05.com
    ServerAlias www.eden.wise.B05.com
    <Directory /var/www/eden.wise.B05.com/public>
      Options +Indexes
    </Directory>
    <Directory /var/www/eden.wise.B05.com>
    Options +FollowSymLinks -Multiviews
      AllowOverride All
    </Directory>
    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  
  * Tinggal merestart atau reload apache2 dengan `service apache2 restart`
* Pada SSS & Garden
  * lynx www.eden.wise.b05.com/public
  * screenshot :
  ![Screenshot_2022-10-29_14_47_44](https://user-images.githubusercontent.com/91010605/198820386-ea6bcf56-5f3a-492e-92b1-c4c9465676de.png)\

### No 12
* Loid juga ingin menyiapkan error file 404.html pada folder /error untuk
mengganti error kode pada apache
* Pada Eden
  * Tambahkan opsi `ErrorDocument` pada konfigurasi situs apache2 `/etc/apache2/sites-available/eden.wise.B05.com.conf`
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.B05.com
    ServerName eden.wise.B05.com
    ServerAlias www.eden.wise.B05.com
    
    ErrorDocument 404 /error/404.html
    ErrorDocument 500 /error/404.html
    ErrorDocument 502 /error/404.html
    ErrorDocument 503 /error/404.html
    ErrorDocument 504 /error/404.html
    
    <Directory /var/www/eden.wise.B05.com/public>
      Options +Indexes
    </Directory>
    <Directory /var/www/eden.wise.B05.com>
      Options +FollowSymLinks -Multiviews
      AllowOverride All
    </Directory>
    
    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  * Restart atau Reload the Apache2 service dengan `service apache2 reload`
* Pada SSS & Garden
  * lynx eden.wise.b05.com/yow
  ![Screenshot_2022-10-29_14_48_51](https://user-images.githubusercontent.com/91010605/198820411-9b694922-bbd6-4543-b9c6-48fb0585c978.png)

### No 13
* Loid juga meminta Franky untuk dibuatkan
konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset
www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js.
* Pada Eden
  * Tambahkan opsi Alias pada konfigurasi `/etc/apache2/sites-available/eden.wise.B05.com.conf`
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.B05.com
    ServerName eden.wise.B05.com
    ServerAlias www.eden.wise.B05.com
 
    <Directory /var/www/eden.wise.B05.com/public>
      Options +Indexes
    </Directory>
    Alias "/js" "/var/www/eden.wise.B05.com/public/js"
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    ErrorDocument 404 /error/404.html
    ErrorDocument 500 /error/404.html
    ErrorDocument 502 /error/404.html
    ErrorDocument 503 /error/404.html
    ErrorDocument 504 /error/404.html
    <Directory /var/www/eden.wise.B05.com>
      Options +FollowSymLinks -Multiviews
      AllowOverride All
    </Directory>
  </VirtualHost>
  ```
  * Restart atau Reload the Apache2 service dengan `service apache2 reload`
 * Pada SSS & Garden
  * `lynx www.eden.wise.B05.com/js`
  * Screenshot : 
  ![Screenshot_2022-10-29_14_49_35](https://user-images.githubusercontent.com/91010605/198820449-b86ab1b1-fc1a-485c-b7c5-bbef4142b2c7.png)

### No 14
* Pada Eden, buat konfigurasi site `strix.operation.wise.B05.com` pada dir `/etc/apache2/sites-available/strix.operation.wise.B05.com.conf` seperti berikut.
```
<VirtualHost *:15000 *:15500>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/strix.operation.wise.B05.com
    ServerName strix.operation.wise.B05.com
    ServerAlias www.strix.operation.wise.B05.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

* Buat direktori `/var/www/strix.operation.wise.B05.com` dan download resource serta unzip dan pindahkan isinya ke direktori tersebut. Lalu aktifkan sitenya dengan command `a2ensite` seperti berikut.
```
mkdir /var/www/strix.operation.wise.B05.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT’ -O /var/www/strix.operation.wise.zip
unzip /var/www/strix.operation.wise.zip -d /var/www
cp -r /var/www/strix.operation.wise/* /var/www/strix.operation.wise.B05.com
rm -r /var/www/strix.operation.wise
a2ensite strix.operation.wise.B05.com
```

* Tambahkan port 15000 dan 15500 pada konfigurasi port di `/etc/apache2/ports.conf` seperti berikut.
```
Listen 80
Listen 15000
Listen 15500

<IfModule ssl_module>
    Listen 443
</IfModule>
<IfModule gnutls.c>
    Listen 443
</IfModule>
```
* Restart apache2 dengan command `service apache2 restart`

* Pada SSS dan Garden, ubah konfigurasi nameserver agar mengarah ke IP WISE dan Berlint. Kemudian gunakan lynx untuk mencoba membuka site nya seperti berikut.
```
echo "
nameserver 192.175.3.2 #IP WISE nameserver 
192.175.2.2 # IP Berlint
" > /etc/resolv.conf
lynx strix.operation.wise.B05.com:15000
lynx strix.operation.wise.B05.com:15500
```
![image14](https://user-images.githubusercontent.com/100585249/198834865-e7c025f6-98b8-44a4-9c0d-e812f2a22857.PNG)
### No 15
* Pada Eden, install package `apache2-utils` dengan command `apt-get install apache2-utils`.
* Buat konfigurasi password pada file `.htpasswd` dengan command berikut.
```
htpasswd -c /etc/apache2/.htpasswd Twilight opStrix
```
* Tambahkan kode berikut pada konfigurasi site `strix.operation.wise.B05.com` pada dir `/etc/apache2/sites-available/strix.operation.wise.B05.com.conf`.
```
<Directory "/var/www/strix.operation.wise.B05.com">
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```
* Restart apache2 dengan command `service apache2 restart`

* Pada SSS dan Garden, atur nameserver dan coba buka site menggunakan lynx sperti berikut.
```
echo "
nameserver 192.175.3.2 #IP WISE nameserver 
192.175.2.2 # IP Berlint
" > /etc/resolv.conf
lynx strix.operation.wise.B05.com:15000
lynx strix.operation.wise.B05.com:15500
```
![image15_1](https://user-images.githubusercontent.com/100585249/198834877-dddda542-68b9-4556-94c5-187b2e603d95.PNG)
![image15_2](https://user-images.githubusercontent.com/100585249/198834878-633acfbf-fcaa-4ea2-8ad1-2f7ac19d8853.PNG)
![image15_3](https://user-images.githubusercontent.com/100585249/198834881-d7bf9dd0-c70f-4f93-acd7-460cfcef61a4.PNG)
### No 16
* Pada Eden, ubah dan tambahkan kode berikut pada konfigurasi `000-default.conf` pad dir `/etc/apache2/sites-available/000-default.conf`.
```
RewriteEngine On
RewriteCond %{HTTP_HOST} !^wise.B05.com$
RewriteRule /.* http://wise.B05.com/ [R]
```
* Restart apache2 dengan command `service apache2 restart`

* Pada SSS dan Garden, coba buka IP Eden dengan lynx seperti berikut `lynx 192.175.2.3`
![image16](https://user-images.githubusercontent.com/100585249/198834896-ee46c7ba-a0b9-41bc-a3e3-ff24d43fd8b6.PNG)
### No 17
* Pada Eden, aktifkan modul rewrite dengan command `a2enmod rewrite`.
* Buat file `.htaccess` dengan menggunakan `nano /var/www/eden.wise.B05.com/.htaccess` dan tambahkan kode berikut.
```
RewriteEngine On
RewriteRule (.*)eden(.*)(\.jpg|\.png|\.gif)$ http://eden.wise.B05.com/public/images/eden.png [L,R=301]
```
* Ubah konfigurasi site `eden.wise.B05.com` pada dir `/etc/apache2/sites-available/eden.wise.B05.com.conf` seperti berikut.
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.B05.com
    ServerName eden.wise.B05.com
    ServerAlias www.eden.wise.B05.com

    ErrorDocument 404 /error/404.html
    ErrorDocument 500 /error/404.html
    ErrorDocument 502 /error/404.html
    ErrorDocument 503 /error/404.html
    ErrorDocument 504 /error/404.html

    <Directory /var/www/eden.wise.B05.com/public>
        Options +Indexes
    </Directory>

    Alias "/js" "/var/www/eden.wise.B05.com/public/js"

    <Directory /var/www/eden.wise.B05.com>
        Options +FollowSymLinks -Multiviews
        AllowOverride All
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

* Restart apache2 dengan command `service apache2 restart`

* Pada SSS, gunakan lynx untuk mengakses url seperti berikut
```
lynx eden.wise.B05.com/public/images/not-eden.png
```
* Pada Garden, gunakan lynx untuk mengakses url seperti berikut
```
lynx eden.wise.B05.com/public/images/buddies.jpg
```
![image17_1](https://user-images.githubusercontent.com/100585249/198834912-90d8f92d-8ce9-45b8-88cb-4bb3525c5a30.PNG)
![image17_2](https://user-images.githubusercontent.com/100585249/198834918-ede86887-ec15-43e1-ab1d-edc53eff8779.PNG)

## Kendala Pengerjaan Modul 2
* Lupa install libapache, sehingga di no 10 tidak bisa jalan / unknown host saat melakukan ping di node client.
* Peletakan nameserver yang terbalik, mempengaruhi saat melakukan ping (jika terbalik peletakannya, maka tidak akan bisa melakukan ping domain yg diminta).
