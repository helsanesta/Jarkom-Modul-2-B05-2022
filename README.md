# Jarkom-Modul-2-B05-2022


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
  * Restart or Reload the Apache2 with `service apache2 reload`
