# Jarkom-Modul-3-B05-2022

## Anggota 
Nama            | NRP
-------------   | -------------
Helsa Nesta Dhaifullah  | 5025201005
Achmad Nashruddin Riskynanda    | 5025201021
Haniif Ahmad Jauhari  | 5025201224

## Soal dan Pembahasan
### Konfigurasi Network
![image](https://user-images.githubusercontent.com/70515589/200468417-e4668fdb-c294-4376-b1d9-ec4b4b36a215.png)
* Ostania
  ```
  auto eth0
  iface eth0 inet dhcp
  
  auto eth1
  iface eth1 inet static
	      address 192.175.1.1
	      netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
	      address 192.175.2.1
	      netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
  	    address 192.175.3.1
	      netmask 255.255.255.0
  ```
* WISE
  ```
  auto eth0
  iface eth0 inet static
	      address 192.175.2.2
	      netmask 255.255.255.0
	      gateway 192.175.2.1
  ```
* Berlint
  ```
  auto eth0
  iface eth0 inet static
	      address 192.175.2.3
	      netmask 255.255.255.0
	      gateway 192.175.2.1
  ```
* Westalis
  ```
  auto eth0
  iface eth0 inet static
	      address 192.175.2.4
	      netmask 255.255.255.0
	      gateway 192.175.2.1
  ```
* SSS
  ```
  auto eth0
  iface eth0 inet static
        address 192.175.1.2
        netmask 255.255.255.0
        gateway 192.175.1.1
  ```
* Garden
  ```
  auto eth0
  iface eth0 inet static
        address 192.175.1.3
        netmask 255.255.255.0
        gateway 192.175.1.1
  ```
* Eden
  ```
  auto eth0
  iface eth0 inet static
        address 192.175.3.2
        netmask 255.255.255.0
        gateway 192.175.3.1
  ```
* NewstonCastle
  ```
  auto eth0
  iface eth0 inet static
        address 192.175.3.3
        netmask 255.255.255.0
        gateway 192.175.3.1
  ```
* KemonoPark
  ```
  auto eth0
  iface eth0 inet static
        address 192.175.3.4
        netmask 255.255.255.0
        gateway 192.175.3.1
  ```

### --Soal 1--
Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server <br>

#### Langkah Penyelesaian
* WISE
  - Jalankan command apt-get update dan apt-get install bind9 -y untuk menginstall bind9 <br>
* Westalis
  - Jalankan command apt-get update dan apt-get install isc-dhcp-server -y untuk menginstall isc-dhcp-server <br>
  - Setting INTERFACES di Westalis pada file /etc/default/isc-dhcp-server dengan menambahkan eth0 <br>
* Berlint
  - Jalankan command apt-get update dan apt-get install squid -y untuk menginstall squid

### --Soal 2--
Ostania sebagai DHCP Relay <br>

#### Langkah Penyelesaian
* Ostania
  - Jalankan command apt-get update dan apt-get install isc-dhcp-relay -y untuk menginstall isc-dhcp-relay
  - Jika muncul prompt `What servers should the DHCP relay forward requests to?` ketikkan `IP Westalis`
  - Kemudian muncul prompt `On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?` ketikkan `eth1 eth2 eth3`
  - Terakhir, jalankan command `service isc-dhcp-relay restart`

### --Soal 3--
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155 <br>

#### Langkah Penyelesaian
* Westalis
  - Edit file `/etc/dhcp/dhcpd.conf' dengan menambahkan :
  ```
  subnet 192.175.1.0 netmask 255.255.255.0 {
        range 192.175.1.50 192.175.1.88;
        range 192.175.1.120 192.175.1.155;
        option routers 192.175.1.1;
        option broadcast-address 192.175.1.255;
        option domain-name-servers 192.175.2.2;
        default-lease-time 300;
        max-lease-time 6900;
  }
  subnet 192.175.2.0 netmask 255.255.255.0{
  }
  ```
  -  Selanjutnya, jalankan command `service isc-dhcp-server restart` dan `service isc-dhcp-server status` <br>
  ![image](https://user-images.githubusercontent.com/70515589/200472436-a62c61ea-82d9-4240-a471-253b0fdec2a4.png)

* SSS
  - Cek ip a untuk melihat ip awal <br>
  ![image](https://user-images.githubusercontent.com/70515589/200469897-cdaaeac1-0f9a-4918-9501-8e28e9f8df0d.png)
  - Edit file '/etc/network/interfaces' menjadi seperti :
  ```
  #auto eth0
  #iface eth0 inet static
  #        address 192.175.1.2
  #        netmask 255.255.255.0
  #        gateway 192.175.1.1
  auto eth0
  iface eth0 inet dhcp
  ```
  - Matikan kemudian nyalakan ulang node SSS
  - kemudian buka di web console untuk melihat bahwa SSS sudah mendapatkan server melalui DHCP server <br>
  ![image](https://user-images.githubusercontent.com/70515589/200470232-db46a2df-71d8-4052-a57c-3a3341314f46.png)
* Garden
  - Cek ip a untuk melihat ip awal <br>
  ![image](https://user-images.githubusercontent.com/70515589/200470327-5d07357e-8830-4f98-b3bf-f9254cca0299.png)
  - Edit file '/etc/network/interfaces' menjadi seperti :
  ```
  #auto eth0
  #iface eth0 inet static
  #        address 192.175.1.3
  #        netmask 255.255.255.0
  #        gateway 192.175.1.1
  auto eth0
  iface eth0 inet dhcp
  ```
  - Matikan kemudian nyalakan ulang node Garden
  - kemudian buka di web console untuk melihat bahwa Garden sudah mendapatkan server melalui DHCP server<br>
  ![image](https://user-images.githubusercontent.com/70515589/200470364-dee558d4-33df-41d6-8247-c38937761de0.png)

### --Soal 4--
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85 <br>

#### Langkah Penyelesaian
* Westalis
  - Edit file `/etc/dhcp/dhcpd.conf' dengan menambahkan :
  ```
  subnet 192.175.3.0 netmask 255.255.255.0 {
        range 192.175.3.10 192.175.3.30;
        range 192.175.3.60 192.175.3.85;
        option routers 192.175.3.1;
        option broadcast-address 192.175.3.255;
        option domain-name-servers 192.175.2.2;
        default-lease-time 600;
        max-lease-time 6900;
  }
  ```
  -  Selanjutnya, jalankan command `service isc-dhcp-server restart`
* Eden
  - Cek ip a untuk melihat ip awal <br>
  ![image](https://user-images.githubusercontent.com/70515589/200470746-b0b56243-29a0-40c1-8995-59da9872f2e2.png)
  - Edit file '/etc/network/interfaces' menjadi seperti :
  ```
  #auto eth0
  #iface eth0 inet static
  #        address 192.175.3.2
  #        netmask 255.255.255.0
  #        gateway 192.175.3.1
  auto eth0
  iface eth0 inet dhcp
  ```
  - Matikan kemudian nyalakan ulang node Eden
  - kemudian buka di web console untuk melihat bahwa Eden sudah mendapatkan server melalui DHCP server <br>
  ![image](https://user-images.githubusercontent.com/70515589/200470842-1e56d326-d77f-4345-8bc7-c6248fca0611.png)
  
* NewstonCastle
  - Cek ip a untuk melihat ip awal <br>
  ![image](https://user-images.githubusercontent.com/70515589/200472210-0f86967d-873c-43da-91b0-5315430b8285.png)
  - Edit file '/etc/network/interfaces' menjadi seperti :
  ```
  #auto eth0
  #iface eth0 inet static
  #        address 192.175.3.3
  #        netmask 255.255.255.0
  #        gateway 192.175.3.1
  auto eth0
  iface eth0 inet dhcp
  ```
  - Matikan kemudian nyalakan ulang node NewstonCastle
  - kemudian buka di web console untuk melihat bahwa NewstonCastle sudah mendapatkan server melalui DHCP server <br>
  ![image](https://user-images.githubusercontent.com/70515589/200472243-c30725bf-1391-4aba-b5d5-6cb258a5a0d3.png)
  
* KemonoPark
  - Cek ip a untuk melihat ip awal <br>
  ![image](https://user-images.githubusercontent.com/70515589/200472375-868ea48c-8e1b-4c99-85d9-a88ed9404846.png)
  - Edit file '/etc/network/interfaces' menjadi seperti :
  ```
  #auto eth0
  #iface eth0 inet static
  #        address 192.175.3.4
  #        netmask 255.255.255.0
  #        gateway 192.175.3.1
  auto eth0
  iface eth0 inet dhcp
  ```
  - Matikan kemudian nyalakan ulang node KemonoPark
  - kemudian buka di web console untuk melihat bahwa KemonoPark sudah mendapatkan server melalui DHCP server <br>
  ![image](https://user-images.githubusercontent.com/70515589/200472337-31a20bd1-b713-429e-b885-52e067d2af62.png)
  
### --Soal 5--
Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut <br>

#### Langkah Penyelesaian
* WISE
  - Edit file /etc/bind/named.conf.options dengan menambahkan
  ```
  forwarders {
        "IP nameserver dari Ostania";
    };
  allow-query{any;};
  ```
  - Dan komen bagian 
  ```
  // dnssec-validation auto;
  ```
  - Jalankan command `service bind9 restart`
  
* Westalis
  - Jalankan command `service isc-dhcp-server restart`
* Ostania
  - Jalankan command `service isc-dhcp-relay restart`
* SSS, Garden, Eden, NewstonCastle, KemonoPark
  - Jalankan command `cat /etc/resolv.conf` untuk melihat nameserver nya mengarah ke WISE <br>
  ![image](https://user-images.githubusercontent.com/70515589/200473230-336b4ce8-7efb-4649-98b4-7d24f5d08064.png) <br>
  ![image](https://user-images.githubusercontent.com/70515589/200473280-bc22cb26-2603-47d0-84b1-fa6d95cb5178.png)  
  
### --Soal 6--
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit <br>

#### Langkah Penyelesaian
* Westalis
  - untuk switch 1 (5 menit * 60 = 300 dan 115 menit * 60 = 6900)
  ```
  subnet 192.175.1.0 netmask 255.255.255.0 {
        range 192.175.1.50 192.175.1.88;
        range 192.175.1.120 192.175.1.155;
        option routers 192.175.1.1;
        option broadcast-address 192.175.1.255;
        option domain-name-servers 192.175.2.2;
        default-lease-time 300;
        max-lease-time 6900;
  }
  ```
  - untuk switch3 (10 menit * 60 = 600 dan 115 menit * 60 = 6900)
  ```
  subnet 192.175.3.0 netmask 255.255.255.0 {
        range 192.175.3.10 192.175.3.30;
        range 192.175.3.60 192.175.3.85;
        option routers 192.175.3.1;
        option broadcast-address 192.175.3.255;
        option domain-name-servers 192.175.2.2;
        default-lease-time 600;
        max-lease-time 6900;
  }
  ```

### --Soal 7--
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13 <br>

#### Langkah Penyelesaian
* Westalis
  - Edit file /etc/dhcp/dhcpd.conf dengan menambahkan baris ini
  ```
  host Eden {
        hardware ethernet "hardware address Eden";
        fixed-address 192.175.3.13;
  }
  ```
  -  Jalankan command `service isc-dhcp-server restart`

* Ostania
  - jalankan command `service isc-dhcp-relay restart`
  
* Eden
  - Kemudian tambahkan hwaddress ether "hardware address Eden" pada `/etc/network/interfaces` agar hwaddress tidak berubah-ubah ketika project direstart atau diexport
  - Matikan Node Eden dan Nyalakan Ulang untuk melihat kalau berhasil <br>
  ![image](https://user-images.githubusercontent.com/70515589/200474142-afedccdf-98eb-40de-9486-8fe3b432a122.png)

  
## --Proxy--
### No 1
Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)

#### Langkah Penyelesaian
- Berlint
    - Install squid dengan command `apt-get install squid`
    - Buat backup file konfigurasi squid dengan command `mv /etc/squid/squid.conf /etc/squid/squid.conf.bak`
    - buat file konfigurasi acl dengan dir `/etc/squid/acl.conf` yang berisi berikut.
    ```
    acl WORKING time MTWHF 08:00-17:00
    ```
    - buat konfigurasi squid baru dengan dir `/etc/squid/squid.conf` yang berisi kode berikut.
    ```
    include /etc/squid/acl.conf

    http_port 8080
    visible_hostname Berlint

    http_access deny WORKING
    http_access allow all
    ```
    - restart squid dengan command `service squid restart`
- SSS/Garden/Eden
    - aktifkan proxy dengan command `export http_proxy=”http://192.175.2.3:8080”`
    - Ubah waktu menjadi waktu jam kerja, contoh `date -s “7 nov 2022 13:00”`. Lalu test dengan command `lynx google.com`
    [image]
    - Ubah waktu menjadi waktu non jam kerja, contoh `date -s “7 nov 2022 18:00”`. Lalu test dengan command `lynx google.com`
    [image]

### No 2
Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)

#### Langkah Penyelesaian
- Wise
    - Buat domain `loid-work.com` dan `franky-work.com` dengan menambahkan kode berikut ke `/etc/bind/named.conf.local`
    ```
    zone "loid-work.com" {
    	type master;
    	file "/etc/bind/jarkom/loid-work.com";
    };
    zone "franky-work.com" {
    	type master;
    	file "/etc/bind/jarkom/franky-work.com";
    };
    ```
    - Buat dir `/etc/bind/jarkom/` dengan menggunakan `mkdir /etc/bind/jarkom/`
    - Buat file konfigurasi `/etc/bind/jarkom/loid-work.com` sebagai berikut.
    ```
    $TTL	604800
    @	IN	SOA	loid-work.com.	root.loid-work.com. (
    			     1		; Serial
    			604800		; Refresh
    			 86400		; Retry
    		   2419200		; Expire
    			604800 )	; Negative Cache TTL
    ;
    @	IN	NS	    loid-work.com.
    @	IN 	A	    192.75.2.2
    @	IN	AAAA	::1
    www IN 	CNAME 	loid-work.com.
    ```
    - Buat file konfigurasi `/etc/bind/jarkom/franky-work.com` sebagai berikut.
    ```
    $TTL	604800
    @	IN	SOA	franky-work.com.	root.franky-work.com. (
    			     1			; Serial
    			604800			; Refresh
    			 86400			; Retry
    		   2419200			; Expire
    			604800 )		; Negative Cache TTL
    ;
    @	IN	NS	    franky-work.com.
    @	IN 	A	    192.75.2.2
    @	IN	AAAA	::1
    www IN 	CNAME 	franky-work.com.
    ```
    - Restart bind9 dengan command `service bind9 restart`
- Berlint
    - Buat file acl `/etc/squid/work-sites.acl` sebagai berikut.
    ```
    loid-work.com
    franky-work.com
    ```
    - Ubah konfigurasi squid menjadi sebagai berikut.
    ```
    include /etc/squid/acl.conf

    http_port 8080
    visible_hostname Berlint

    acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
    http_access allow WORKSITES
    http_access deny WORKING
    http_access allow all
    ```
    - Restart squid dengan command `service squid restart`
- SSS/Garden/Eden
    - aktifkan proxy dengan command `export http_proxy=”http://192.175.2.3:8080”`
    - Ubah waktu menjadi waktu jam kerja, contoh `date -s “7 nov 2022 13:00”`
    - Test `lynx google.com`
    [image]
    - Test `lynx loid-work.com`
    [image]
    - Test `lynx franky-work.com`
    [image]
### No 3
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

#### Langkah Penyelesaian
- Berlint
    - Ubah konfigurasi squid menjadi sebagai berikut
    ```
    include /etc/squid/acl.conf

    http_port 8080
    visible_hostname Berlint

    acl SSL_ports port 443
    acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
    http_access deny !SSL_ports
    http_access allow WORKSITES
    http_access deny WORKING
    http_access allow all
    ```
- SSS/Garden/Eden
    - Aktifkan proxy dengan command `export http_proxy=”http://192.175.2.3:8080”`
    - Ubah waktu menjadi waktu non jam kerja, contoh `date -s “7 nov 2022 18:00”`
    - Test `lynx http://example.com`
    [image]
    - Test `lynx https://example.com`
    [image]
  

