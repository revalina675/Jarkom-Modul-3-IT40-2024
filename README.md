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

