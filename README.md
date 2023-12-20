# Jarkom-Modul-5-E25-2023
Berikut adalah laporan resmi untuk pengerjaan Praktikum Modul 5 Jarkom Firewall
## Anggota Kelompok E25
| Nama | NRP |
| --- | --- |
| Zakia Kolbi | 5025211049 |
| Ketut Arda Putra Mahotama Sadha | 5025211235 |

# Daftar Isi
- [Topologi](#topologi)
- [Konfigurasi Network Node](#konfigurasi-network-node)
- [Soal-Soal](#soal-soal)
  - [Soal 1](#soal-1)
    - [Script](#script)
    - [Hasil](#hasil)
  - [Soal 2](#soal-2)
    - [Script](#script-2)
    - [Hasil](#hasil-2)
  - [Soal 3](#soal-3)
    - [Script](#script-3)
    - [Hasil](#hasil-3)
  - [Soal 4](#soal-4)
    - [Script](#script-4)
    - [Hasil](#hasil-4)
  - [Soal 5](#soal-5)
    - [Script](#script-5)
    - [Hasil](#hasil-5)
  - [Soal 6](#soal-6)
    - [Script](#script-6)
    - [Hasil](#hasil-6)
  - [Soal 7](#soal-7)
    - [Script](#script-7)
    - [Hasil](#hasil-7)
  - [Soal 8](#soal-8)
    - [Script](#script-8)
    - [Hasil](#hasil-8)
  - [Soal 9](#soal-9)
    - [Script](#script-9)
    - [Hasil](#hasil-9)
  - [Soal 10](#soal-10)
    - [Script](#script-10)
    - [Hasil](#hasil-10)
 
## Topologi

![topoo drawio](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/108173647/37f33553-e0d3-40ed-a9f6-f19f7311f6fa)

## Konfigurasi Network Node

Menggunakan VLSM diperoleh

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/356e8352-afaa-404e-ad68-4c2827041264)

Pohon subnet adalah sebagai berikut

![VLSM Modul 5 Fix](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/8463325d-f2cc-4831-9c80-13bc1a1246d7)

### Aura

Network Config
```
auto eth0
iface eth0 inet dhcp
    up /sbin/iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $(/sbin/ip -4 a show eth0 | /bin/grep -Po 'inet \K[0-9.]*')

auto eth1
iface eth1 inet static
    address 10.49.14.129
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.49.14.133
    netmask 255.255.255.252
    
```

Routing
```
# Backtrack (Ke Cloud)

# Ke Frieren
route add -net 10.49.12.0 netmask 255.255.254.0 gw 10.49.14.134 #A4
route add -net 10.49.14.136 netmask 255.255.255.252 gw 10.49.14.134 #A6
route add -net 10.49.14.140 netmask 255.255.255.252 gw 10.49.14.134 #A7
route add -net 10.49.14.144 netmask 255.255.255.252 gw 10.49.14.134 #A8
route add -net 10.49.14.148 netmask 255.255.255.252 gw 10.49.14.134 #A9
route add -net 10.49.14.0 netmask 255.255.255.128 gw 10.49.14.134 #A10

# Ke Heiter
route add -net 10.49.0.0 netmask 255.255.248.0 gw 10.49.14.130 #A1
route add -net 10.49.8.0 netmask 255.255.252.0 gw 10.49.14.130 #A2
```

### Frieren

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.134
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.49.14.137
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.49.14.141
    netmask 255.255.255.252
```

Routing
```
# Backtrack
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.49.14.133

# Ke Himmel
route add -net 10.49.12.0 netmask 255.255.254.0 gw 10.49.14.138 #A4
route add -net 10.49.14.144 netmask 255.255.255.252 gw 10.49.14.138 #A8
route add -net 10.49.14.148 netmask 255.255.255.252 gw 10.49.14.138 #A9
route add -net 10.49.14.0 netmask 255.255.255.128 gw 10.49.14.138 #A10
```

### Himmel

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.138
    netmask 255.255.255.252
    up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
    address 10.49.14.1
    netmask 255.255.255.128

auto eth2
iface eth2 inet static
    address 10.49.12.1
    netmask 255.255.254.0
```

Routing
```
# Backtrack
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.49.14.137

# Ke Fern
route add -net 10.49.14.144 netmask 255.255.255.252 gw 10.49.14.2 #A8
route add -net 10.49.14.148 netmask 255.255.255.252 gw 10.49.14.2 #A9
```

Script Relay
```
yes | apt-get install isc-dhcp-relay -y

echo '
SERVERS="10.49.14.150"
INTERFACES="eth1 eth2"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

### Fern

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.2
    netmask 255.255.255.128

auto eth1
iface eth1 inet static
    address 10.49.14.149
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.49.14.145
    netmask 255.255.255.252
```

Routing
```
# Backtrack
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.49.14.1
```

### Heiter

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.130
    netmask 255.255.255.252
    up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
    address 10.49.8.1
    netmask 255.255.252.0

auto eth2
iface eth2 inet static
    address 10.49.0.1
    netmask 255.255.248.0
```

Routing
```
# Backtrack
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.49.14.129
```

Script Relay
```
yes | apt-get install isc-dhcp-relay -y

echo '
SERVERS="10.49.14.150"
INTERFACES="eth0 eth1 eth2"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

### Revolte (DHCP Server)

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.150
    gateway 10.49.14.149
    netmask 255.255.255.252
    up echo nameserver 192.168.122.1 > /etc/resolv.conf    
```

Script DHCP
```
yes | apt-get install isc-dhcp-server -y

echo "
INTERFACES=\"eth0\"
" > /etc/default/isc-dhcp-server

yes | echo "

default-lease-time 28800;
max-lease-time 57600;

ddns-update-style none;


subnet 10.49.14.148 netmask 255.255.255.252 {
    option routers 10.49.14.149;
    option broadcast-address 10.49.14.151;
    option domain-name-servers 192.168.122.1;
}

subnet 10.49.0.0 netmask 255.255.248.0 {
    range 10.49.0.2 10.49.7.254;
    option routers 10.49.0.1;
    option broadcast-address 10.49.7.255;
    option domain-name-servers 192.168.122.1;
}

subnet 10.49.8.0 netmask 255.255.252.0 {
    range 10.49.8.3 10.49.11.254;
    option routers 10.49.8.1;
    option broadcast-address 10.49.11.255;
    option domain-name-servers 192.168.122.1;
}

subnet 10.49.12.0 netmask 255.255.254.0 {
    range 10.49.12.2 10.49.13.254;
    option routers 10.49.12.1;
    option broadcast-address 10.49.13.255;
    option domain-name-servers 192.168.122.1;
}

subnet 10.49.14.0 netmask 255.255.255.128 {
    range 10.49.14.2 10.49.14.126;
    option routers 10.49.14.1;
    option broadcast-address 10.49.14.127;
    option domain-name-servers 192.168.122.1;
}

" > /etc/dhcp/dhcpd.conf

rm /var/run/dhcpd.pid

service isc-dhcp-server restart
service isc-dhcp-server status
```

### Ritcher (DNS Server)

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.146
    gateway 10.49.14.145
    netmask 255.255.255.252
    up echo nameserver 192.168.122.1 > /etc/resolv.conf    
```

### Sein (Web Server)

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.8.2
    gateway 10.49.8.1
    netmask 255.255.252.0
    up echo nameserver 192.168.122.1 > /etc/resolv.conf    
```

Script Webserver
```
apt-get install nginx

echo '
    <h1>Sein nih boss</h1>
' > /var/www/html/index.nginx-debian.html

echo 'server {
        listen 80 default_server;
        listen 443 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            try_files $uri $uri/ =404;
        }
}' > /etc/nginx/sites-available/default

service nginx restart
```

### Stark

Network Config
```
auto eth0
iface eth0 inet static
    address 10.49.14.142
    gateway 10.49.14.141
    netmask 255.255.255.252
    up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Script Webserver
```
apt-get install nginx

echo '
    <h1>Stark nih boss</h1>
' > /var/www/html/index.nginx-debian.html

echo 'server {
        listen 80 default_server;
        listen 443 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            try_files $uri $uri/ =404;
        }
}' > /etc/nginx/sites-available/default

service nginx restart
```

### Client (Node yang belum disebut)

Network Config
```
auto eth0
iface eth0 inet dhcp
```

## Soal-Soal
### Soal 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.

#### Script
```
/sbin/iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $(/sbin/ip -4 a show eth0 | /bin/grep -Po 'inet \K[0-9.]*')
```
#### Hasil

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/e640b905-c0f1-46db-a471-ea45fb199cfe)

### Soal 2
> Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

#### Script
```
#No 2

iptables -F

iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p udp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -j DROP
iptables -A INPUT -p udp -j DROP
```

#### Hasil

Packet didrop

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/ac907f4e-6e69-4a3d-863a-3bdea14dfc0e)

Packet tidak didrop

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/2e45ede1-b24d-4acf-aac9-d442f1c3a874)


### Soal 3
> Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.

#### Script

Pada Fern
```
# No 3
# Ritcher
iptables -t mangle -A PREROUTING -d 10.49.14.146 -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROPLOG
# Revolte
iptables -t mangle -A PREROUTING -d 10.49.14.150 -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROPLOG
```

#### Hasil

3 Client ping secara bersamaan

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/3fcc124e-646e-41fb-9c37-5c1960bb35b1)

Client ke-4

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/0be5c8e3-31ca-4b7a-9009-3c284754ff15)

### Soal 4
> Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.

#### Script

Pada Sein dan Stark
```
iptables -A INPUT -p tcp --dport 22 -m iprange --src-range 10.49.8.3-10.49.11.254  -j ACCEPT
iptables -A OUTPUT -p tcp --sport 22 -m iprange --dst-range 10.49.8.3-10.49.11.254 -j ACCEPT

iptables -A INPUT -p all -j DROP
```

#### Hasil

GrobeForest

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/d6a31643-8771-45b5-b34b-0af19d510b68)

Selain GrobeForest

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/70660cfa-918b-44a1-8c3b-307f2cd5557e)

### Soal 5
> Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.

#### Script

Pada Sein dan Stark
```
#No 5
iptables -A INPUT -m time --weekdays Sat,Sun -j DROP
iptables -A INPUT -p all -m time --timestart 16:00 --timestop 23:59:59 -j DROPLOG
iptables -A INPUT -p all -m time --timestart 00:00 --timestop 08:00 -j DROPLOG
```

#### Hasil

Packet accept

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/f27a4eae-5436-491a-bdd1-ab2a0d4f03a3)


Packet di drop (tidak ada respon)

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/4e8bbf04-99b2-4fd2-993f-8d06c7b6881b)

### Soal 6
> Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).

#### Script

Pada Sein dan Stark
```
#No 6
iptables -A INPUT -p all -m time --timestart 11:00 --timestop 13:00 --weekdays Fri -j DROPLOG
iptables -A INPUT -p all -m time --timestart 12:00 --timestop 13:00 --weekdays Mon,Tue,Wed,Thu -j DROPLOG
```

#### Hasil

Packet accept

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/f27a4eae-5436-491a-bdd1-ab2a0d4f03a3)


Packet di drop (tidak ada respon)

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/4e8bbf04-99b2-4fd2-993f-8d06c7b6881b)

### Soal 7
> Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.

#### Script

Pada Sein
```
#No 7 Load Balancing
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

iptables -t nat -A PREROUTING -p tcp --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.49.14.142:80
iptables -t nat -A POSTROUTING -p tcp --dport 80 -j SNAT --to-source 10.49.8.2
```

Pada Stark
```
#No 7 Load Balancing
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

iptables -t nat -A PREROUTING -p tcp --dport 443 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.49.8.2:443
iptables -t nat -A POSTROUTING -p tcp --dport 443 -j SNAT --to-source 10.49.14.142
```

#### Hasil

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/ee6118f9-aaf2-438f-a04c-b5703db75a7c)

### Soal 8
> Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.

#### Script

Pada Sein dan Stark
```
#No 8
iptables -A INPUT -p all -s 10.49.14.148/30 -m time --datestart 2024-02-14 --datestop 2024-06-26  -j DROPLOG
```

#### Hasil

Packet di drop (Tidak ada respon)

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/f40e2618-de47-4b18-b9d6-9e673938f69b)


Packet accept

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/d66d7a69-427e-446b-b7f1-baa997b2480d)


### Soal 9
> Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir  alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit. 
(clue: test dengan nmap)

#### Script

Pada Sein dan Stark
```
#No 9
iptables -A INPUT -p all -m state --state NEW -m recent --set --name Portscans
iptables -A INPUT -p all -m recent --rcheck --seconds 600 --hitcount 20 --name Portscans -j DROPLOG
```

#### Hasil

Scan 5 Port

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/d9fc5203-29a7-4f18-b6a0-73cb5827ebb9)

Scan lebih dari 20 port

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/c9c36965-45c0-41be-9ab9-48a519ac1d70)

Setelah diblokir (tidak ada respon)

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/56975f64-8fbf-4ec0-9a51-8764cb187b52)

### Soal 10
> Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.

#### Script

Tiap node yang drop packet
```
#No 10 Logging
iptables -N DROPLOG
iptables -A DROPLOG -j LOG --log-prefix "Dropped Packet:" --log-level debug
iptables -A DROPLOG -j DROP
```

Jika ingin drop packet, lakukan jump ke target DROPLOG









