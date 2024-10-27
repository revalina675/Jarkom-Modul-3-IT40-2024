# LAPORAN RESMI MODUL 03 KOMUNIKASI DATA DAN JARINGAN KOMPUTER

## Kelompok IT40

### Anggota Kelompok :

| Nama                          | NRP        |
| ----------------------------- | ---------- |
| Revalina Fairuzy Azhari Putri | 5027231001 |
| Kevin Anugerah Faza           | 5027231027 |

# Topologi
![image](https://github.com/user-attachments/assets/3f417d2d-4c83-4368-8888-625d2e70c14e)

# Ketentuan
![image](https://github.com/user-attachments/assets/be84bc27-571a-41fb-a409-4100401a73a5)

_Pulau Paradis dan Marley, sama-sama menghadapi ancaman besar dari satu sama lain. Keduanya membangun infrastruktur pertahanan yang kuat untuk melindungi sistem vital mereka. Dengan strategi yang matang, mereka bersiap menghadapi serangan dan mempertahankan wilayah masing-masing._

_Bangsa Marley, dipimpin oleh Zeke, telah mempersiapkan Annie, Bertholdt, dan Reiner untuk menyerang menggunakan Laravel Worker. Di sisi lain, Klan Eldia dari Paradis telah mempersiapkan Armin, Eren, dan Mikasa sebagai PHP Worker untuk mempertahankan pulau tersebut. Warhammer bertindak sebagai Database Server, sementara Beast dan Colossal sebagai Load Balancer._

# Konfigurasi

### Paradis

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.83.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.83.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.83.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.83.4.1
	netmask 255.255.255.0

up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.83.0.0/16
```

### Tybur 

```
auto eth0
iface eth0 inet static
	address 10.83.4.2
	netmask 255.255.255.0
	gateway 10.83.4.1
```

### Fritz

```
auto eth0
iface eth0 inet static
	address 10.83.4.3
	netmask 255.255.255.0
	gateway 10.83.4.1
```

### Warhammer

```
iface eth0 inet static
	address 10.83.3.4
	netmask 255.255.255.0
	gateway 10.83.3.1
```

### Beast

```
auto eth0
iface eth0 inet static
	address 10.83.3.2
	netmask 255.255.255.0
	gateway 10.83.3.1
```

### Colossal

```
auto eth0
iface eth0 inet static
	address 10.83.3.3
	netmask 255.255.255.0
	gateway 10.83.3.1
```

### Annie

```
auto eth0
iface eth0 inet static
	address 10.83.1.2
	netmask 255.255.255.0
	gateway 10.83.1.1
```

### Bertholdt

```
auto eth0
iface eth0 inet static
	address 10.83.1.3
	netmask 255.255.255.0
```

### Reiner

```
auto eth0
iface eth0 inet static
	address 10.83.1.4
	netmask 255.255.255.0
	gateway 10.83.1.1
```

### Armin

```
auto eth0
iface eth0 inet static
	address 10.83.2.2
	netmask 255.255.255.0
	gateway 10.83.2.1
```

### Eren

```
auto eth0
iface eth0 inet static
	address 10.83.2.3
	netmask 255.255.255.0
	gateway 10.83.2.1
```

### Mikasa

```
auto eth0
iface eth0 inet static
	address 10.83.2.4
	netmask 255.255.255.0
	gateway 10.83.2.1
```

### Zeke

```
auto eth0
iface eth0 inet static
	address 10.83.1.5
	netmask 255.255.255.0
	gateway 10.83.1.1
```

### Erwin

```
auto eth0
iface eth0 inet static
	address 10.83.2.5
	netmask 255.255.255.0
	gateway 10.83.2.1
```

# Script /root/.bashrc
Script ini harus ada pada setiap node sesuai dengan pembagiannya.

### Paradis
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.83.0.0/16
apt-get update
apt-get install isc-dhcp-relay -y
```

### Tybur (DHCP Server)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```

### Fritz (DNS Server)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```

### Beast (Load Balancer/Laravel)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update 
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y
apt-get install php-fpm php-mbstring php-xml php-zip -y
service nginx start
```

### Colossal (Load Balancer/PHP)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update 
apt-get install bind9 -y
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y
apt-get install php-fpm php-mbstring php-xml php-zip -y
service nginx start
```

### Warhammer (Database Server)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y
service mysql start
```

### Zeke (Client)
```
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

### Erwin (Client)
```
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

### Armin, Eren, Mikasa (PHP Worker)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt install software-properties-common -y
apt install php -y
apt install php php-fpm -y
```

### Annie, Bertholdt, Reiner (Laravel Worker)
```
echo 'nameserver 10.83.4.3' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y
apt-get install wget -y
apt-get install unzip -y
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
apt install php7.3 -y
apt install php7.3-fpm -y
apt-get install nginx -y
apt-get install git -y
apt-get install htop -y
service nginx start
```

# Setup tambahan
Pembuatan Script warprepare.sh pada Fritz
```
echo 'zone "marley.it40.com" { 
        type master; 
        file "/etc/bind/marley/marley.it40.com";
};

zone "eldia.it40.com" {
        type master;
        file "/etc/bind/eldia/eldia.it40.com";
}; ' >> /etc/bind/named.conf.local

mkdir /etc/bind/marley
mkdir /etc/bind/eldia

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it40.com. root.marley.it40.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it40.com.
@       IN      A       10.83.1.2     ; IP Annie' > /etc/bind/marley/marley.it40.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it40.com. root.eldia.it40.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it40.com.
@       IN      A       10.83.2.2     ; IP Armin' > /etc/bind/eldia/eldia.it40.com

service bind9 restart
```

## NOTED: Pastikan Paradis dan Fritz berjalan terlebih dahulu untuk kemudian lanjut ke node Tybur dan seterusnya.

# Pengerjaan Soal

## Soal 1-5
### Paradis (soal1-5.sh)
```
echo '# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.83.4.2"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay start 
```
### Tybur (soal1-5.sh)
```
echo 'subnet 10.83.1.0 netmask 255.255.255.0 {
    range 10.83.1.05 10.83.1.25;
    range 10.83.1.50 10.83.1.100;
    option routers 10.83.1.1;
    option broadcast-address 10.83.1.255;
    option domain-name-servers 10.83.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.83.2.0 netmask 255.255.255.0 {
    range 10.83.2.09 10.83.2.27;
    range 10.83.2.81 10.83.2.243;
    option routers 10.83.2.1;
    option broadcast-address 10.83.2.255;
    option domain-name-servers 10.83.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.83.3.0 netmask 255.255.255.0 {
}

subnet 10.83.4.0 netmask 255.255.255.0 {
}' > /etc/dhcp/dhcpd.conf

echo '# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd'\''s config file (default: /etc/dhcp/dhcpd.conf).
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd'\''s PID file (default: /var/run/dhcpd.pid).
DHCPDv4_PID=/var/run/dhcpd.pid
DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#<----->Don'\''t use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#<----->Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="eth0"
INTERFACESv6=""' > /etc/default/isc-dhcp-server

service isc-dhcp-server restart
```

**CHECKING :**  Cek apakah internet yang ada di client maupun node lain bisa diakses, seharusnya jika file benar maka bisa tersambung ke internet.

## Soal 6
### Armin, Eren, dan Mikasa (soal6.sh)
```
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1TvebIeMQjRjFURKVtA32lO9aL7U2msd6' -O /root/bangsaEldia.zip
unzip /root/bangsaEldia.zip -d /var/www/eldia.it40.com
rm -rf /root/bangsaEldia.zip

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/eldia.it40.com
ln -s /etc/nginx/sites-available/eldia.it40.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/eldia.it40.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/eldia.it40.com

service php7.3-fpm start
service php7.3-fpm restart
service nginx restart
nginx -t
```

**CHECKING :** Lakukan cek dengan  `lynx http://eldia.it40.com` pada Client
![Screenshot 2024-10-27 004020](https://github.com/user-attachments/assets/fd6e8a04-1ed5-471e-a4b2-915842a168f9)

## Soal 7
### Colossal (soal7.sh)
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
        #hash $request_uri;
    server 10.83.2.2;
    server 10.83.2.3;
    server 10.83.2.4;
}

server {
    listen 80;
    server_name eldia.it40.com www.eldia.it40.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```
### Fritz (soal7.sh)
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it40.com. root.eldia.it40.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it40.com.
@       IN      A       10.83.3.3    ; IP Colossal
www     IN      CNAME   eldia.it40.com.' > /etc/bind/eldia/eldia.it40.com


echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it40.com. root.marley.it40.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it40.com.
@       IN      A       10.83.3.3     ; IP Colossal
www     IN      CNAME   marley.it40.com.' > /etc/bind/marley/marley.it40.com

service bind9 restart
```
**CHECKING:** Lakukan cek melalui client dengan `ab -n 6000 -c 200 http://eldia.it40.com`
![Screenshot 2024-10-26 232508](https://github.com/user-attachments/assets/ff5eebb1-e719-4f37-a15e-f732b3663780)

## Soal 8
Tinggal ubah config algoritma load balancing sesuai dengan yang diinginkan. Kemudian cek melalui client dengan command `ab -n 1000 -c 75 http://eldia.it40.com/`
- Least Connection

![Screenshot 2024-10-26 233744](https://github.com/user-attachments/assets/39b22343-2f82-48b6-8202-6b7614acb28e)

- IP Hash

![Screenshot 2024-10-26 234110](https://github.com/user-attachments/assets/13fe2282-55fc-4a63-bd11-b207698e205d)

- Round Robin (default NGINX)

![Screenshot 2024-10-26 234903](https://github.com/user-attachments/assets/bda6a208-2b1f-4415-8fbb-b7b2c2341977)

- Hash

![Screenshot 2024-10-26 235506](https://github.com/user-attachments/assets/280f53ab-d41f-4e86-bb08-ad685d6821e2)

**CHECKING:** Lakukan perbandingan dari 4 algoritma tersebut

### Grafik dan Analisis
![image](https://github.com/user-attachments/assets/ca15d2c0-706f-4253-9afb-a182c7ea43a3)

Least Connection memiliki performa terbaik dalam hal jumlah request per detik. Ini menunjukkan bahwa algoritma ini lebih efektif dalam menangani beban tinggi dibandingkan dengan algoritma lainnya seperti IP Hash, Round Robin, dan Hash.

## Soal 9
### Untuk mengerjakan soal ini, lakukan comment pada bagian "server..." di script soal7.sh yang ada pada Colossal. Berikut adalah bagian yang dimaksud
```
echo ' upstream worker {
        #least_conn;
        #ip_hash;
        #hash $request_uri;
    server 10.83.2.2; (Kalau mau 2 worker ini dicomment)
    server 10.83.2.3; (Kalau mau 1 worker ini dicomment)
    server 10.83.2.4;
}
```

- 3 Worker

![Screenshot 2024-10-26 235935](https://github.com/user-attachments/assets/805b3d95-58ae-4152-97b9-abad5c958725)

- 2 Worker
![WhatsApp Image 2024-10-27 at 09 39 33_e1f2e34d](https://github.com/user-attachments/assets/01e1f9e0-48e1-44c1-ba8e-9288c4cd08f7)

- 1 Worker
![WhatsApp Image 2024-10-27 at 09 38 03_c5f49b4e](https://github.com/user-attachments/assets/3278e9d9-f828-4bfa-914a-7ec00c3f12f9)


### Grafik dan Analisis
![image](https://github.com/user-attachments/assets/acb9310b-58db-4f5a-8436-3d3f0fc55d01)

peningkatan jumlah worker dari 1 menjadi 2 atau 3 justru menurunkan performa RPS secara signifikan, yang mungkin disebabkan oleh faktor bottleneck atau ineffisiensi dalam pengaturan worker di lingkungan ini

## Soal 10
### Jalankan script berikut di Colossal
```
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit40

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
	#hash $request_uri;
    server 10.83.2.2;
    server 10.83.2.3;
    server 10.83.2.4;
}

server {
    listen 80;
    server_name eldia.it40.com www.eldia.it40.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

## Soal 11
### Jalankan script berikut di Colossal
```
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit40

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
        #hash $request_uri;
    server 10.83.2.2;
    server 10.83.2.3;
    server 10.83.2.4;
}

server {
    listen 80;
    server_name eldia.it40.com www.eldia.it40.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```
**CHECKING:** Lakukan cek di client dengan menggunakan command `lynx http://eldia.it40.com/titan`

## Soal 12
### Jalankan script berikut di Colossal
```
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit40

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
        #hash $request_uri;
    server 10.83.2.2;
    server 10.83.2.3;
    server 10.83.2.4;
}

server {
    listen 80;
    server_name eldia.it40.com www.eldia.it40.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        allow 10.83.1.77;
        allow 10.83.1.88;
        allow 10.83.2.144;
        allow 10.83.2.156;
        deny all;
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```
Jika IP tidak sesuai dengan yang diizinkan, maka tidak bisa mengakses web

## Soal 13
Berikut adalah script yang harus dijalankan di Warhammer
```
service mysql start

echo '# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf 

sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf

service mysql restart
```
Langkah selanjutnya adalah masuk ke Annie/client lain dengan command `mysql -u root -p` (password dikosongkan saja). Berikut adalah list line shell yang harus dilakukan setiap insan didalamnya
```
- CREATE USER 'it40'@'%' IDENTIFIED BY 'it40';
- CREATE USER 'it40'@'localhost' IDENTIFIED BY 'it40';
- CREATE DATABASE db_it40;
- GRANT ALL PRIVILEGES ON *.* TO 'it40'@'%';
- GRANT ALL PRIVILEGES ON *.* TO 'it40'@'localhost';
- FLUSH PRIVILEGES;
```
Langkah terakhir adalah dengan menjalankan command `mysql –host=10.83.3.4 –port=3306 –user=it40 –password=it40 db_it40 -e “SHOW DATABASES;"` pada Annie.

![Screenshot 2024-10-27 011457](https://github.com/user-attachments/assets/a3f17b7c-68d2-4ff6-a92c-b0c4f7ee09b6)
