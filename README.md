# Jarkom-Modul-3-B24-2023
Kelompok: B24
Nama anggota:
- Gilang Aliefidanto (5025211119)
- Vito Febrian Ananta (5025211224)

Presentase kontribusi:
- Gilang Aliefidanto (5025211119) :
	- Nomor 8 sampai dengan Nomor 9 (Grimoire)
   	- Pembuatan keseluruhan lapres (Lapres)

- Vito Febrian Ananta (5025211224) :
	- Nomor 1 sampai dengan Nomor 13 (Praktikum)
   	- Nomor 14 sampai dengan Nomor 20 (Revisi)
   	- Nomor 15 sampai dengan Nomor 20 (Gimoire)
   	- Penambahan presentase kontribusi (Lapres)

Link Grimoire:
https://drive.google.com/file/d/1gW7jbaulkuDCfxfR5BOEDx4VVoiW8eBr/view?usp=sharing

## Persiapan
Untuk menjawab soal, maka kami membuat topologi sesuai arahan. Soal meminta image docker `danielcristh0/debian-buster:1.1` dan peta topologi sebagai berikut:
![gambar_topologi](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/blob/main/resource/topologi.png)
Dengan konfigurasi
- Aura (router)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp

  # Static config for eth1
  auto eth1
  iface eth1 inet static
	  address 192.190.1.0
	  netmask 255.255.255.0
 
  # Static config for eth2
  auto eth2
  iface eth2 inet static
	  address 192.190.2.0
	  netmask 255.255.255.0

  # Static config for eth3
  auto eth3
  iface eth3 inet static
	  address 192.190.3.0
	  netmask 255.255.255.0
 
  # Static config for eth4
  auto eth4
  iface eth4 inet static
	  address 192.190.4.0
	  netmask 255.255.255.0
  ```
- Himmel (DHCP server)
  ```
  # Static config for eth0
  auto eth0
  iface eth0 inet static
	  address 192.190.1.1
	  netmask 255.255.255.0
	  gateway 192.190.1.0
  ```
- Heiter (DNS server)
  ```
  # Static config for eth0
  auto eth0
  iface eth0 inet static
	  address 192.190.1.2
	  netmask 255.255.255.0
	  gateway 192.190.1.0
  ```
- Denken (Database server)
  ```
  # Static config for eth0
  auto eth0
  iface eth0 inet static
	  address 192.190.2.1
	  netmask 255.255.255.0
	  gateway 192.190.2.0
  ```
- Eisen (Load balancer)
  ```
  # Static config for eth0
  auto eth0
  iface eth0 inet static
	  address 192.190.2.2
	  netmask 255.255.255.0
	  gateway 192.190.2.0
  ```
- Frieren (Laravel worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Flamme (Laravel worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Fern (Laravel worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Lawine (PHP Worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Linie (PHP Worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Lugner (PHP Worker)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Revolte (Client)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Richter (Client)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Sein (Client)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
- Stark (Client)
  ```
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  ```
Kemudian semua node wajib diberi akses internet dengan cara menulis pada console. Hal ini bertujuan untuk melakukan install beberapa package dan file yang diperlukan.
- cd ~
- nano 0-connect-internet.sh
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.190.0.0/16
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- echo 'bash 0-connect-internet.sh' >> .bashrc


## Nomor 0
Soal:
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP mengarah pada worker yang memiliki IP [prefix IP].x.1.

Jawab:
Untuk membuat web, maka akan dilakukan install bind9 pada DNS server (Hitler)
- nano 0-install-bind9.sh
  ```
  apt-get update
  apt-get install bind9 -y
  service bind9 start
  ```
- nano 0-create-zone.sh
  ```
  mkdir /etc/bind/jarkom
  echo 'zone "riegel.canyon.b24.com" {
          type master;
          file "/etc/bind/jarkom/riegel.canyon.b24.com";
  };

  zone "granz.channel.b24.com" {
          type master;
          file "/etc/bind/jarkom/granz.channel.b24.com";
  };
  ' > /etc/bind/named.conf.local
  ```
- nano 0-create-domain.sh
  ```
  echo ';
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA   riegel.canyon.b24.com. root.riegel.canyon.b24.com. (
			      2023110101    ; Serial
                          604800        ; Refresh
                          86400         ; Retry
                          2419200       ; Expire
                          604800 )      ; Negative Cache TTL
  ;
  @               IN      NS      riegel.canyon.b24.com.
  @               IN      A       192.190.4.1 ; IP Frieren
  www             IN      CNAME   riegel.canyon.b24.com.
  ' > /etc/bind/jarkom/riegel.canyon.b24.com

  echo ';
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA   granz.channel.b24.com. root.granz.channel.b24.com. (
	  		    2023110101    ; Serial
                          604800        ; Refresh
                          86400         ; Retry
                          2419200       ; Expire
                          604800 )      ; Negative Cache TTL
  ;
  @               IN      NS      granz.channel.b24.com.
  @               IN      A       192.190.3.1 ; IP Lawine
  www             IN      CNAME   granz.channel.b24.com.
  ' > /etc/bind/jarkom/granz.channel.b24.com
  service bind9 restart
  ```
- nano .bashrc
  ```
  # Tambah bash
  bash 0-install-bind9.sh
  bash 0-create-zone.sh
  bash 0-create-domain.sh
  ```


## Nomor 1
Soal:
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Jawab:
Telah dikerjakan pada persiapan

## Nomor 2, 3, 4, 5
Soal:
- Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
- Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
- Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
- Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
Jawab:
Tambah semua node worker dengan hwadress (MAC address)
- Lawine
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether b2:f5:31:5a:5e:54
    ' > /etc/network/interfaces
    ```
- Linie
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether b2:3e:a8:47:ad:2c
    ' > /etc/network/interfaces
    ```
- Lugner
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether c6:10:b5:05:42:f9
    ' > /etc/network/interfaces
    ```
- Frieren
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether de:df:61:0a:c3:66
    ' > /etc/network/interfaces
    ```
- Flamme
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether 26:95:cf:80:4f:22
    ' > /etc/network/interfaces
    ```
- Fern
  - nano 2-5-add-interface-hwaddres.sh
    ```
    echo 'auto eth0
    iface eth0 inet dhcp

    hwaddress ether 16:3c:81:2c:1a:39
    ' > /etc/network/interfaces
    ```
- [all worker]
  - nano .bashrc
    ```
    # Tambah
    bash 2-5-add-interface-hwaddres.sh
    ```
- Himmel
  - nano 2-5-install-dhcp-server.sh
    ```
    apt-get update
    apt-get install isc-dhcp-server -y
    dhcpd --version
    ps aux | grep dhcpd
    service isc-dhcp-server stop
    ls -l /var/run/dhcpd.pid
    rm /var/run/dhcpd.pid
    ps aux | grep dhcpd
    service isc-dhcp-server start
    ```
  - nano 2-5-config-dhcp-server.sh
    ```
    echo '# Defaults for isc-dhcp-server initscript
    # sourced by /etc/init.d/isc-dhcp-server
    # installed at /etc/default/isc-dhcp-server by the maintainer scripts

    #
    # This is a POSIX shell fragment
    #

    # Path to dhcpds config file (default: /etc/dhcp/dhcpd.conf).
    #DHCPD_CONF=/etc/dhcp/dhcpd.conf

    # Path to dhcpds PID file (default: /var/run/dhcpd.pid).
    #DHCPD_PID=/var/run/dhcpd.pid

    # Additional options to start dhcpd with.
    #       Dont use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
    #OPTIONS=""

    # On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
    #       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
    INTERFACES="eth0"
    ' > /etc/default/isc-dhcp-server
    echo 'subnet 192.190.1.0 netmask 255.255.255.0 {
    }

    subnet 192.190.3.0 netmask 255.255.255.0 {
        range 192.190.3.16 192.190.3.32;
        range 192.190.3.64 192.190.3.80;
        option routers 192.190.3.0;
        option broadcast-address 192.190.3.255;
        option domain-name-servers 192.190.1.2;
        default-lease-time 120;
        max-lease-time 5760;

        host Lawine {
            hardware ethernet b2:f5:31:5a:5e:54;
            fixed-address 192.190.3.1;
        }

        host Linie {
            hardware ethernet b2:3e:a8:47:ad:2c;
            fixed-address 192.190.3.2;
        }

        host Lugner {
            hardware ethernet c6:10:b5:05:42:f9;
            fixed-address 192.190.3.3;
        }
    }

    subnet 192.190.4.0 netmask 255.255.255.0 {
        range 192.190.4.12 192.190.4.20;
        range 192.190.4.120 192.190.4.168;
        option routers 192.190.4.0;
        option broadcast-address 192.190.4.255;
        option domain-name-servers 192.190.1.2;
        default-lease-time 720;
        max-lease-time 5760;

        host Frieren {
            hardware ethernet de:df:61:0a:c3:66;
            fixed-address 192.190.4.1;
        }

        host Flamme {
            hardware ethernet 26:95:cf:80:4f:22;
            fixed-address 192.190.4.2;
        }

        host Fern {
            hardware ethernet 16:3c:81:2c:1a:39;
            fixed-address 192.190.4.3;
        }
    }
    ' > /etc/dhcp/dhcpd.conf
    service isc-dhcp-server restart
    service isc-dhcp-server status
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 2-5-install-dhcp-server.sh
    bash 2-5-config-dhcp-server.sh
    ```
- Aura
  - nano 2-5-install-dhcp-relay.sh
    ```
    apt-get update
    apt-get install isc-dhcp-relay -y
    service isc-dhcp-relay start
    ```
  - nano 2-5-config-dhcp-relay.sh
    ```
    echo 'SERVERS="192.190.1.1"
    INTERFACES="eth1 eth3 eth4"
    OPTIONS=
    ' > /etc/default/isc-dhcp-relay
    echo 'net.ipv4.ip_forward=1
    ' > /etc/sysctl.conf
    service isc-dhcp-relay restart
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 2-5-install-dhcp-relay.sh
    bash 2-5-config-dhcp-relay.sh
    ```


## Nomor 6
Soal:
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3
Jawab:
- [PHP worker]
  - nano 6-add-wget-unzip.sh
    ```
    apt-get update
    apt-get install wget unzip -y
    ```
  - nano 6-install-nginx-php-php-fpm.sh
    ```
    apt-get update
    apt-get install nginx -y
    service nginx start
    service nginx status
    apt-get install php php-fpm -y
    php -v
    service php7.3-fpm start
    service php7.3-fpm status
    ```
  - nano 6-deploy-granz.sh
    ```
    echo 'server {

    listen 80;

    root /var/www/granz.channel.b24.com;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
    }
    ' > /etc/nginx/sites-available/granz.channel.b24.com

    wget --no-check-certificate -nc 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O downloaded-zip
    unzip downloaded-zip -d /var/www/
    mkdir /var/www/granz.channel.b24.com
    mv /var/www/modul-3/* /var/www/granz.channel.b24.com/
    rm -rf /var/www/modul-3
    rm -rf downloaded-zip
    unlink /etc/nginx/sites-enabled/default
    ln -s /etc/nginx/sites-available/granz.channel.b24.com /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 6-add-wget-unzip.sh
    bash 6-install-nginx-php-php-fpm.sh
    bash 6-deploy-granz.sh
    ```
- Eisen
  - nano 6-install-nginx.sh
    ```
    apt-get update
    apt-get install nginx -y
    service nginx start
    service nginx status
    ```
  - nano 6-lb.sh
    ```
    echo '#Default menggunakan Round Robin
    upstream backend {
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;

    }
    ' > /etc/nginx/sites-available/lb
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-rrw
    ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 6-lb.sh
    bash 6-install-nginx.sh
    ```
- Heiter
  - nano 6-forward.sh
    ```
    echo 'options {
        directory "/var/cache/bind";
        forwarders {
        192.168.122.1;
        };
        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no; # conform to RFC1035
        listen-on-v6 { any; };
    };
    ' > /etc/bind/named.conf.options
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 6-forward.sh
    ```
- [Client]
  - nano 6-install-lynx.sh
    ```
    apt update
    apt-get install lynx -y
    ```
  - bash .bashrc
    ```
    # Tambah
    bash 6-install-lynx.sh
    ```
## Nomor 7
Soal:
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
- Lawine, 4GB, 2vCPU, dan 80 GB SSD.
- Linie, 2GB, 2vCPU, dan 50 GB SSD.
- Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.
Jawab:
| Algoritma  | File |
| ------------- | ------------- |
| ROUND ROBIN  | 6-lb.sh  |
| ROUND ROBIN WEIGHTED  | 7-lb-rrw.sh  |
| LEAST CONNECTION  | 8-lb-lc.sh  |
| IP HASH  | 8-lb-iph.sh  |
| GENERIC HASH  | 8-lb-gh.sh  |

- Richter
  - nano 7-install-ab.sh
    ```
    apt-get update && apt-get install apache2-utils -y
    ab -V
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 7-install-ab.sh
    ```
  - Lakukan test dengan 1000 request dan 100 request/second
    ```
    ab -n 1000 -c 100 http://192.190.2.2/
    ```
- [PHP worker]
  - nano 7-install-htop.sh
    ```
    apt-get install htop
    ```
  - nano .bashrc
    ```
    # Tambah
    bash 7-install-htop.sh
    ```
- Eisen
  - nano 6-lb.sh
    ```
    echo '#Default menggunakan Round Robin
    upstream backend {
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;

    }
    ' > /etc/nginx/sites-available/lb
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-gh
    ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano 7-lb-rrw.sh
    ```
    echo 'upstream backend {
    server 192.190.3.1 weight=4; #IP Lawine
    server 192.190.3.2 weight=2; #IP Linie
    server 192.190.3.3 weight=1; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb-rrw_error.log;
    access_log /var/log/nginx/lb-rrw_access.log;

    }
    ' > /etc/nginx/sites-available/lb-rrw
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-gh
    ln -s /etc/nginx/sites-available/lb-rrw /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
Dapat dilihat dari asumsi pada soal bahwa adanya perbedaan resource antar worker, sehingga algoritma round robin weighted akan lebih memaksimalkan kinerja Eisen (load balancer). Server yang memiliki weight paling besar yakni Lawine akan dijadikan prioritas ketika menerima request dari client, karena memiliki resource yang lebih dibandingkan dengan server lainnya (Linie, Lugner). Weight dapat digunakan untuk mengoptimalkan load balancing dan memastikan bahwa server yang lebih kuat memiliki beban yang lebih besar.***

Perbedaan testing load balancing dapat dilihat pada video melalui link berikut:
https://drive.google.com/file/d/1PhsyT17_67-EJwVyi7Nh0aXg5kBKAYRy/view?usp=sharing

## Nomor 8
Soal:
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis
Jawab:
- Eisen
  - nano 8-lb-lc.sh
    ```
    echo 'upstream backend {
    least_conn;
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb-lc_error.log;
    access_log /var/log/nginx/lb-lc_access.log;

    }
    ' > /etc/nginx/sites-available/lb-lc
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-gh
    rm -f /etc/nginx/sites-enabled/lb-15-16-17
    ln -s /etc/nginx/sites-available/lb-lc /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano 8-lb-iph.sh
    ```
    echo 'upstream backend {
    ip_hash;
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb-iph_error.log;
    access_log /var/log/nginx/lb-iph_access.log;

    }
    ' > /etc/nginx/sites-available/lb-iph
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-gh
    rm -f /etc/nginx/sites-enabled/lb-15-16-17
    ln -s /etc/nginx/sites-available/lb-iph /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano 8-lb-gh.sh
    ```
    echo 'upstream backend {
    hash $request_uri consistent;
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
    }

    server {
    listen 80;
    server_name granz.channel.b24.com;

            location / {
                    proxy_pass http://backend;
                    proxy_set_header    X-Real-IP $remote_addr;
                    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header    Host $http_host;
            }

    error_log /var/log/nginx/lb-gh_error.log;
    access_log /var/log/nginx/lb-gh_access.log;

    }
    ' > /etc/nginx/sites-available/lb-gh
    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-15-16-17
    ln -s /etc/nginx/sites-available/lb-gh /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
- Ritcher
  Lakukan test pada node ini (ubah algoritma sesuai pada tabel nomor 7)
  ```
  ab -n 200 -c 10 http://192.190.2.2/
  ```
Setelah membuat lima algoritma web server, semua algoritma sekilas mirip, hanya beda pada saat 90% request terlayani. Jika lebih diteliti, algoritma Round Robin Weight merupakan algoritma terbaik dalam melayani request. Walaupun membutuhkan banyak waktu saat di bawah 5%, algoritma tersebut membutuhkan waktu yang efisien setelah itu. Terlihat jika algoritma Rund Robin Weight memiliki waktu yang lebih singkat daripada algoritma lainnya. Selain itu, Rund Robin Weight juga lebih stabil.
Perbandingan waktu antar algoritma jika sampai semua request terpenuhi:
![gambar_grafik-algo](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/blob/main/resource/grafik-algo.png)


## Nomor 9
Soal:
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.
Jawab:
- [PHP worker]
  Matikan 1 atau 2 nginx pada node PHP worker sesuai pengujian
  ```
  service nginx stop
  ```
- Ritcher
  Lakukan test pada node ini (ubah algoritma sesuai pada tabel nomor 7)
  ```
  ab -n 200 -c 10 http://192.190.2.2/
  ```
Perbandingan waktu antar algoritma jika sampai semua request terpenuhi:
![gambar_grafik-algo](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/blob/main/resource/grafik-lb.png)


## Soal 10
Soal:
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/
Jawab:
- Eisen
  - nano 10-auth.sh
    ```
    apt-get install apache2-utils -y
    rm -rf /etc/nginx/rahasiakita
    mkdir /etc/nginx/rahasiakita
    htpasswd -c -b /etc/nginx/rahasiakita/.htpasswd netics ajkb24
    ```
  - nano 10-lb.sh
    ```
    cat << 'EOF' > /etc/nginx/sites-available/lb
    upstream backend {
        server 192.190.3.1; #IP Lawine
        server 192.190.3.2; #IP Linie
        server 192.190.3.3; #IP Lugner
    }

    server {
        listen 80;
        server_name granz.channel.b24.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;

            auth_basic "Administrator's Area";
            auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }

        location ~ /\.ht {
           deny all;
        }

        error_log /var/log/nginx/lb_error.log;
        access_log /var/log/nginx/lb_access.log;
    }
    EOF

    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-gh
    rm -f /etc/nginx/sites-enabled/lb-15-16-17

    ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano .bashrc
    ```
    bash 10-auth.sh
    bash 10-lb.sh
    ```
- Richter
  Lakukan pengujian dan masuk ke dalam web
  ```
  ab -A netics:ajkb24 -n 100 -c 100 http://192.190.2.2/
  lynx http://192.190.2.2/
  ```


## Nomor 11
Soal:
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id
Jawab:
- Eisen
  - nano 11-lb.sh
    ```
    cat << 'EOF' > /etc/nginx/sites-available/lb
    upstream backend {
        server 192.190.3.1; #IP Lawine
        server 192.190.3.2; #IP Linie
        server 192.190.3.3; #IP Lugner
    }

    server {
        listen 80;
        server_name granz.channel.b24.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;

            auth_basic "Administrator's Area";
            auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }

        location ~ /\.ht {
            deny all;
        }

        location /its {
            proxy_pass https://www.its.ac.id;
        }

        location ~ ^/(.*/)its {
            proxy_pass https://www.its.ac.id;
        }

        error_log /var/log/nginx/lb_error.log;
        access_log /var/log/nginx/lb_access.log;
    }
    EOF

    rm -f /etc/nginx/sites-enabled/default
    rm -f /etc/nginx/sites-enabled/lb-lc
    rm -f /etc/nginx/sites-enabled/lb-rrw
    rm -f /etc/nginx/sites-enabled/lb-iph
    rm -f /etc/nginx/sites-enabled/lb-gh
    rm -f /etc/nginx/sites-enabled/lb-15-16-17

    ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
    ls /etc/nginx/sites-enabled/
    service nginx restart
    ```
  - nano .bashrc
    ```
    bash 11-lb.sh
    ```
- Richter
  ```
  lynx 192.190.2.2/its
  lynx 192.190.2.2/foo/its
  ```
  
## Nomor 12
Soal:
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.
Jawab:
- Revolte
nano 12-add-interface-hwaddres.sh
```
echo 'auto eth0
iface eth0 inet dhcp

hwaddress ether 4e:66:b6:ad:8a:d9
' > /etc/network/interfaces

nano .bashrc
bash 12-add-interface-hwaddres.sh

Richter
nano 12-add-interface-hwaddres.sh
echo 'auto eth0
iface eth0 inet dhcp

hwaddress ether 5e:17:cc:01:6c:91
' > /etc/network/interfaces
```
nano .bashrc
```
bash 12-add-interface-hwaddres.sh
```
- Stark
nano 12-add-interface-hwaddres.sh
```
echo 'auto eth0
iface eth0 inet dhcp

hwaddress ether ea:88:d9:be:1f:d4
' > /etc/network/interfaces
```
nano .bashrc
```
bash 12-add-interface-hwaddres.sh
```
- Himmel
nano 12-add-host-fixed-address.sh
```
echo 'subnet 192.190.1.0 netmask 255.255.255.0 {
}

subnet 192.190.3.0 netmask 255.255.255.0 {
    range 192.190.3.16 192.190.3.32;
    range 192.190.3.64 192.190.3.80;
    option routers 192.190.3.0;
    option broadcast-address 192.190.3.255;
    option domain-name-servers 192.190.1.2;
    default-lease-time 120;
    max-lease-time 5760;

    host Lawine {
        hardware ethernet b2:f5:31:5a:5e:54;
        fixed-address 192.190.3.1;
    }

    host Linie {
        hardware ethernet b2:3e:a8:47:ad:2c;
        fixed-address 192.190.3.2;
    }

    host Lugner {
        hardware ethernet c6:10:b5:05:42:f9;
        fixed-address 192.190.3.3;
    }


    host Revolte {
        hardware ethernet 4e:66:b6:ad:8a:d9;
        fixed-address 192.190.3.69;
    }
    
    host Richter {
        hardware ethernet 5e:17:cc:01:6c:91;
        fixed-address 192.190.3.70;
    }
}

subnet 192.190.4.0 netmask 255.255.255.0 {
    range 192.190.4.12 192.190.4.20;
    range 192.190.4.120 192.190.4.168;
    option routers 192.190.4.0;
    option broadcast-address 192.190.4.255;
    option domain-name-servers 192.190.1.2;
    default-lease-time 720;
    max-lease-time 5760;

    host Frieren {
        hardware ethernet de:df:61:0a:c3:66;
        fixed-address 192.190.4.1;
    }

    host Flamme {
        hardware ethernet 26:95:cf:80:4f:22;
        fixed-address 192.190.4.2;
    }

    host Fern {
        hardware ethernet 16:3c:81:2c:1a:39;
        fixed-address 192.190.4.3;
    }

    host Stark {
        hardware ethernet ea:88:d9:be:1f:d4;
        fixed-address 192.190.4.168;
    }
}
' > /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
service isc-dhcp-server status
```
nano .bashrc
```
bash 12-add-host-fixed-address.sh
```
- Eisen
nano 12-lb.sh
```
cat << 'EOF' > /etc/nginx/sites-available/lb
upstream backend {
    server 192.190.3.1; #IP Lawine
    server 192.190.3.2; #IP Linie
    server 192.190.3.3; #IP Lugner
}

server {
    listen 80;
    server_name granz.channel.b24.com;

    location / {
        allow 192.190.3.69;
        allow 192.190.3.70;
        allow 192.190.4.167;
        allow 192.190.4.168;
        deny all;

        proxy_pass http://backend;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;

        auth_basic "Administrator's Area";
        auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
    }

    location ~ /\.ht {
        deny all;
    }

    location /its {
        proxy_pass https://www.its.ac.id;
    }

    location ~ ^/(.*/)its {
        proxy_pass https://www.its.ac.id;
    }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}
EOF

# Assuming the rest of the script remains unchanged
rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/lb-lc
rm -f /etc/nginx/sites-enabled/lb-rrw
rm -f /etc/nginx/sites-enabled/lb-iph
rm -f /etc/nginx/sites-enabled/lb-gh
rm -f /etc/nginx/sites-enabled/lb-15-16-17

ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
service nginx restart
```
nano .bashrc
```
bash 12-lb.sh
```


## Nomor 13
Soal:
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.
Jawab:
- Denken
nano 13-install-mariadb.sh
```
apt-get update
apt-get install mariadb-server -y
service mysql start
```
nano .bashrc
```
bash 13-install-mariadb.sh
```
```
dpkg --configure -a
mysql

CREATE USER 'kelompokb24'@'%' IDENTIFIED BY 'passwordb24';
CREATE USER 'kelompokb24'@'localhost' IDENTIFIED BY 'passwordb24';
CREATE DATABASE dbkelompokb24;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokb24'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokb24'@'localhost';
FLUSH PRIVILEGES;

[ctrl +c] (exit mysql)

mysql -u kelompokb24 -p
passwordb24

SHOW DATABASES;
USE dbkelompokb24;
SHOW TABLES;
```
nano 13-allow-all-ip.sh
```
cat << 'EOF' > /etc/mysql/my.cnf
# The MariaDB configuration file
#
# The MariaDB/MySQL tools read configuration files in the following order:
# 1. "/etc/mysql/mariadb.cnf" (this file) to set global defaults,
# 2. "/etc/mysql/conf.d/*.cnf" to set global options.
# 3. "/etc/mysql/mariadb.conf.d/*.cnf" to set MariaDB-only options.
# 4. "~/.my.cnf" to set user-specific options.
#
# If the same option is defined multiple times, the last one will apply.
#
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.

#
# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address
EOF

service mysql restart

bash 13-allow-all-ip.sh

Sein
nano 13-install-mariadb-client.sh
apt-get update
apt-get install mariadb-client -y
```
nano .bashrc
```
bash 13-install-mariadb-client.sh
```
```
mariadb --host=192.190.2.1 --port=3306 --user=kelompokb24 --password
passwordb24
SHOW DATABASES;
USE dbkelompokb24;
```
## REVISI DARI NO 14-20
## Nomor 14
Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer
- Frieren, Flamme, Fern
nano 14-install-pkg.sh
```
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer
composer -V
apt-get install git -y

cd /var/www
git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
cd laravel-praktikum-jarkom
composer update && composer install
cp .env.example .env

cat << 'EOF' > .env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.190.2.1
DB_PORT=3306
DB_DATABASE=dbkelompokb24
DB_USERNAME=kelompokb24
DB_PASSWORD=passwordb24

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
EOF
```
nano .bashrc
```
bash 14-install-pkg.sh
```
- Flamme
nano 14-migrate.sh
```
cd /var/www/laravel-praktikum-jarkom
php artisan migrate:fresh
php artisan db:seed --class=AiringsTableSeeder
cd ../../../root
```
bash 14-migrate.sh

- Denken
```
mysql -u kelompokb24 -p
passwordb424
SHOW DATABASES;
USE dbkelompokb24;
SHOW TABLES;
SELECT * FROM airings;
```

- Frieren, Flamme, Fern
nano 14-generate.sh
```
cd /var/www/laravel-praktikum-jarkom
php artisan key:generate
cd ../../../root
```
bash 14-generate.sh

Frieren
nano 14-deploy-nginx.sh
```
cat << 'EOF' > /etc/nginx/sites-available/laravel-praktikum-jarkom
server {
    listen 8001;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/laravel-praktikum-jarkom_error.log;
    access_log /var/log/nginx/laravel-praktikum-jarkom_access.log;
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/laravel-praktikum-jarkom
ln -s /etc/nginx/sites-available/laravel-praktikum-jarkom /etc/nginx/sites-enabled/
cd /etc/nginx/sites-available/
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
cd ../../../root
service php8.0-fpm start
service nginx restart
cd /var/www/laravel-praktikum-jarkom/
php artisan jwt:secret --no-interaction
cd ../../../root
apt-get update
apt-get install lynx -y
```
bash 14-deploy-nginx.sh

- Flamme
nano 14-deploy-nginx.sh
```
cat << 'EOF' > /etc/nginx/sites-available/laravel-praktikum-jarkom
server {
    listen 8002;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/laravel-praktikum-jarkom_error.log;
    access_log /var/log/nginx/laravel-praktikum-jarkom_access.log;
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/laravel-praktikum-jarkom
ln -s /etc/nginx/sites-available/laravel-praktikum-jarkom /etc/nginx/sites-enabled/
cd /etc/nginx/sites-available/
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
cd ../../../root
service php8.0-fpm start
service nginx restart
cd /var/www/laravel-praktikum-jarkom/
php artisan jwt:secret --no-interaction
cd ../../../root
apt-get update
apt-get install lynx -y
```
bash 14-deploy-nginx.sh

- Fern
nano 14-deploy-nginx.sh
```
cat << 'EOF' > /etc/nginx/sites-available/laravel-praktikum-jarkom
server {
    listen 8003;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/laravel-praktikum-jarkom_error.log;
    access_log /var/log/nginx/laravel-praktikum-jarkom_access.log;
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/laravel-praktikum-jarkom
ln -s /etc/nginx/sites-available/laravel-praktikum-jarkom /etc/nginx/sites-enabled/
cd /etc/nginx/sites-available/
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
cd ../../../root
service php8.0-fpm start
service nginx restart
cd /var/www/laravel-praktikum-jarkom/
php artisan jwt:secret --no-interaction
cd ../../../root
apt-get update
apt-get install lynx -y
```
bash 14-deploy-nginx.sh

- Frieren
```
lynx http://localhost:8001/
```
- Flamme
```
lynx http://localhost:8002/
```
- Fern
```
lynx http://localhost:8003/
```


## Nomor 15, 16, 17
Soal:
Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
- POST /auth/register
- POST /auth/login
- GET /me

Jawaban:
- Eisen
nano 15-16-17-lb.sh
```
cat << 'EOF' > /etc/nginx/sites-available/lb-15-16-17
upstream laravel {
    server 192.190.4.1:8001; #IP Frieren
    server 192.190.4.2:8002; #IP Flamme
    server 192.190.4.3:8003; #IP Fern
}

server {
    listen 80;
    server_name riegel.canyon.b24.com;

    location / {
        proxy_pass http://laravel;
    }
}
EOF

# Assuming the rest of the script remains unchanged
rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/lb-lc
rm -f /etc/nginx/sites-enabled/lb-rrw
rm -f /etc/nginx/sites-enabled/lb-iph
rm -f /etc/nginx/sites-enabled/lb-gh
rm -f /etc/nginx/sites-enabled/lb

ln -s /etc/nginx/sites-available/lb-15-16-17 /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
service nginx restart
```
nano .bashrc	
```
bash 15-16-17-lb.sh
```
- Clients
```
lynx http://192.190.2.2/
```
nano user.json
```
{
        "username": "username3",
        "password": "password3"
}
```
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/register
```
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/login
```
```
curl -X POST http://192.190.2.2/api/auth/login -H "Content-Type: application/json" -d '{"username":"username3","password":"password3"}'
```
```
ab -n 100 -c 10 -H 'Authorization: Bearer [token login]' -T 'application/json' http://192.190.2.2/api/me
```

Hasil test nomor 15:
![testing-register](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/8d46aca9-b24d-471a-8f2d-f6e9e4495a66)

Hasil test nomor 16:
![testing-login](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/60590c4e-85dc-404c-b500-86b05a1cab7b)

Hasil test nomor 17:
![testing-me](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/fadc68d2-7ef3-44e0-965e-f0c7ff5bce7e)

## Nomor 18
Soal:
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.
Jawab:
- Eisen
nano 18-lb.sh
```
cat << 'EOF' > /etc/nginx/sites-available/lb-18
upstream laravel {
    server 192.190.4.1:8001; #IP Frieren
    server 192.190.4.2:8002; #IP Flamme
    server 192.190.4.3:8003; #IP Fern
}

server {
    listen 80;
    server_name riegel.canyon.b24.com;

    location / {
        proxy_pass http://laravel;
    }

    location /frieren/ {
        # Frieren
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.1:8001/;
    }

    location /flamme/ {
        # Flamme
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.2:8002/;
    }

    location /fern/ {
        # Fern
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.3:8003/;
    }
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/lb-lc
rm -f /etc/nginx/sites-enabled/lb-rrw
rm -f /etc/nginx/sites-enabled/lb-iph
rm -f /etc/nginx/sites-enabled/lb-gh
rm -f /etc/nginx/sites-enabled/lb
rm -f /etc/nginx/sites-enabled/lb-15-16-17

ln -s /etc/nginx/sites-available/lb-18 /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
service nginx restart
```
bash 18-lb.sh

- Frieren, Flamme, Fern
nano 18-change-index.sh
```
cat << 'EOF' > /var/www/laravel-praktikum-jarkom/resources/views/welcome.blade.php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamics Page</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <center>
            <?php
                $hostname = gethostname();
                echo "Hello World!<br>";
                echo "Selamat datang di: $hostname<br>";
            ?>
        </center>
    </div>
</body>
</html>
EOF
```
bash 18-change-index.sh

- Clients
```
lynx 192.190.2.2/frieren
```
```
lynx 192.190.2.2/flamme
```
```
lynx 192.190.2.2/fern
```

## Nomor 19
Soal:
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.
Jawab:
- Eisen
nano 19-install-fpm.sh
```
apt-get update
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
```
bash 19-install-fpm.sh
```
service php8.0-fpm status
service php8.0-fpm start
service php8.0-fpm status
```
nano 19-pool.sh
```
cat << 'EOF' > /etc/php/8.0/fpm/pool.d/eisen.conf
[eisen_site]
user = eisen_user
group = eisen_user
listen = /var/run/php8.0-fpm-eisen-site.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s
EOF
```
```
groupadd eisen_user
useradd -g eisen_user eisen_user
service php8.0-fpm restart
```
bash 19-pool.sh

nano 19-lb.sh
```
cat << 'EOF' > /etc/nginx/sites-available/lb-19
upstream laravel {
    server 192.190.4.1:8001; #IP Frieren
    server 192.190.4.2:8002; #IP Flamme
    server 192.190.4.3:8003; #IP Fern
}

server {
    listen 80;
    server_name riegel.canyon.b24.com;

    location / {
        proxy_pass http://laravel;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
    #
    #       # With php7.0-cgi alone:
    #       fastcgi_pass 127.0.0.1:9000;
    #       # With php7.0-fpm:
            fastcgi_pass unix:/run/php8.0-fpm-eisen-site.sock;
    }

    location /frieren/ {
        # Frieren
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.1:8001/;
    }

    location /flamme/ {
        # Flamme
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.2:8002/;
    }

    location /fern/ {
        # Fern
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.3:8003/;
    }
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/lb-lc
rm -f /etc/nginx/sites-enabled/lb-rrw
rm -f /etc/nginx/sites-enabled/lb-iph
rm -f /etc/nginx/sites-enabled/lb-gh
rm -f /etc/nginx/sites-enabled/lb
rm -f /etc/nginx/sites-enabled/lb-15-16-17
rm -f /etc/nginx/sites-enabled/lb-18

ln -s /etc/nginx/sites-available/lb-19 /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
service nginx restart
```
bash 19-lb.sh

- Clients
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/login
```
- Eisen
nano 19-pool-2.sh
```
cat << 'EOF' > /etc/php/8.0/fpm/pool.d/eisen.conf
[eisen_site]
user = eisen_user
group = eisen_user
listen = /var/run/php8.0-fpm-eisen-site.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 125
pm.start_servers = 15
pm.min_spare_servers = 10
pm.max_spare_servers = 25
pm.process_idle_timeout = 10s
EOF
```
```
groupadd eisen_user
useradd -g eisen_user eisen_user
service php8.0-fpm restart
```
bash 19-pool-2.sh

- Clients
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/login
```

- Eisen
nano 19-pool-3.sh
```
cat << 'EOF' > /etc/php/8.0/fpm/pool.d/eisen.conf
[eisen_site]
user = eisen_user
group = eisen_user
listen = /var/run/php8.0-fpm-eisen-site.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 175
pm.start_servers = 20
pm.min_spare_servers = 15
pm.max_spare_servers = 30
pm.process_idle_timeout = 10s
EOF
```
```
groupadd eisen_user
useradd -g eisen_user eisen_user
service php8.0-fpm restart
```
bash 19-pool-3.sh

- Clients
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/login
```

Hasil test nomor 19:
- Test ke-1:
![testing-pool-1](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/f6a84091-94a3-4137-aa83-26a5ffa0180d)

- Test ke-2:
![testing-pool-2](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/364aacba-3bcf-4a38-80f5-93030850f8c6)

- Test ke-3:
![testing-pool-3](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/b2add384-e45f-4e06-afc5-2ca8b7c35c9f)

Analisa nomor 19:
- Meskipun terdapat variasi konfigurasi PHP-FPM, hasil uji coba (Requests per
second, Time per request) tetap relatif serupa di sepanjang tes. Hal ini mungkin
menunjukkan bahwa pengaturan tidak secara signifikan memengaruhi kinerja di
bawah beban dan konfigurasi spesifik ini.
- Bagian "Connection Times" menunjukkan statistik tentang pembentukan
koneksi, pemrosesan, dan total waktu. Waktu-waktu ini bervariasi dalam
rentang tertentu di seluruh tes.
- Meskipun beban dan konfigurasi berubah-ubah, dampaknya terhadap metrik
kinerja (Requests per second, Time per request) nampaknya relatif konsisten.
Penting untuk mempertimbangkan pola lalu lintas dunia nyata dan kondisi
beban untuk optimisasi yang lebih akurat dalam lingkungan produksi. Selain itu,
faktor lain seperti sumber daya server, logika aplikasi, dan infrastruktur juga bisa
memengaruhi kinerja.

## Nomor 20
Soal:
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.
Jawab:
- Eisen
nano 20-lb.sh
```
cat << 'EOF' > /etc/nginx/sites-available/lb-20
upstream laravel {
    least_conn;
    server 192.190.4.1:8001; #IP Frieren
    server 192.190.4.2:8002; #IP Flamme
    server 192.190.4.3:8003; #IP Fern
}

server {
    listen 80;
    server_name riegel.canyon.b24.com;

    location / {
        proxy_pass http://laravel;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
    #
    #       # With php7.0-cgi alone:
    #       fastcgi_pass 127.0.0.1:9000;
    #       # With php7.0-fpm:
            fastcgi_pass unix:/run/php8.0-fpm-eisen-site.sock;
    }

    location /frieren/ {
        # Frieren
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.1:8001/;
    }

    location /flamme/ {
        # Flamme
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.2:8002/;
    }

    location /fern/ {
        # Fern
        proxy_bind 192.190.2.2;
        proxy_pass http://192.190.4.3:8003/;
    }
}
EOF

rm -f /etc/nginx/sites-enabled/default
rm -f /etc/nginx/sites-enabled/lb-lc
rm -f /etc/nginx/sites-enabled/lb-rrw
rm -f /etc/nginx/sites-enabled/lb-iph
rm -f /etc/nginx/sites-enabled/lb-gh
rm -f /etc/nginx/sites-enabled/lb
rm -f /etc/nginx/sites-enabled/lb-15-16-17
rm -f /etc/nginx/sites-enabled/lb-18
rm -f /etc/nginx/sites-enabled/lb-19

ln -s /etc/nginx/sites-available/lb-20 /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
service nginx restart
```
bash 20-lb.sh

- Clients
```
ab -n 100 -c 10 -T 'application/json' -p user.json -H 'Content-Type: application/json' http://192.190.2.2/api/auth/login
```

Hasil test nomor 20:
![testing-least-conn](https://github.com/gilangalief05/Jarkom-Modul-3-B24-2023/assets/142609964/7fc3b444-7214-4349-8763-d940f1ccb7b6)

Analisa nomor 20:
- Secara umum, hasil tes masih menunjukkan kinerja yang serupa dengan hasil
sebelumnya, di mana tidak ada perubahan yang signifikan dalam metrik kinerja
seperti requests per second (permintaan per detik) atau time per request (waktu
per permintaan).
- Meskipun menggunakan strategi "least_conn" untuk load balancing, tidak
terlihat perubahan yang signifikan dalam waktu respon atau kinerja keseluruhan
dibandingkan dengan tes sebelumnya yang menggunakan metode load
balancing round robin.
- Meskipun begitu, ada sedikit peningkatan dalam beberapa persentil waktu
pemrosesan pada test terbaru, yang bisa menunjukkan sedikit peningkatan
dalam latensi untuk sebagian kecil permintaan.
- Kesimpulannya, meskipun telah beralih ke strategi load balancing "least_conn",
kinerja secara umum masih serupa dengan tes sebelumnya. Pengukuran
kinerja dalam tes terbaru tidak menunjukkan perbaikan signifikan dalam waktu
respon atau throughput dibandingkan dengan pengaturan
