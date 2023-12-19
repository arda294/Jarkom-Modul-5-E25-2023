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

![image](https://github.com/arda294/Jarkom-Modul-5-E25-2023/assets/114855785/7bad2ced-28e5-40ba-9c92-e4883fcad7ad)

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

## Soal-Soal
### Soal 1

#### Script

#### Hasil

### Soal 2

#### Script

#### Hasil

### Soal 3

#### Script

#### Hasil

### Soal 4

#### Script

#### Hasil

### Soal 5

#### Script

#### Hasil

### Soal 6

#### Script

#### Hasil

### Soal 7

#### Script

#### Hasil

### Soal 8

#### Script

#### Hasil

### Soal 9

#### Script

#### Hasil

### Soal 10

#### Script

#### Hasil









