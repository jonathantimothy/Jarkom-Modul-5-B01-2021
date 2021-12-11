# Jarkom-Modul-5-B01-2021


Kelompok B01

|      NRP       |                  Nama                   |
| :------------: | :-------------------------------------: |
| 05111940000120 |       Jonathan Timothy Siregar          |
| 05111940000195 |      Muhammad Fikri Sandi Pratama       |
| 05111940000207 |         Mohammad Thoriq Huda            |

## Topologi GNS3

- Langkah Pertama yaitu membuat topologi pada GNS3 seperti pada gambar dibawah ini:

![image](https://user-images.githubusercontent.com/90582800/145456321-b3bc61de-d228-4017-b327-76990c04a03b.png)

Keterangan
- DORIKI adalah DNS Server
- JIPANGU adalah DHCP Server
- MAINGATE dan JORGE adalah Web Server
- Jumlah Host pada BLUENO adalah 100 host
- Jumlah Host pada CIPHER adalah 700 host
- Jumlah Host pada ELENA adalah 300 host
- Jumlah Host pada FUKUROU adalah 200 host

## Pembagian Subnet

![subnet](https://user-images.githubusercontent.com/55092974/145668879-75df1974-321b-4dad-8027-31ddfc2e3c6e.JPG)


|  Subnet   | Jumlah IP | Netmask |
| :-------: | :-------: | :-----: |
|    A1     |    3      |   /29   |
|    A2     |    101    |   /25   |
|    A3     |    701    |   /22   |
|    A4     |     2     |   /30   |
|    A5     |     2     |   /30   |
|    A6     |    301    |   /23   |
|    A7     |    201    |   /24   |
|    A8     |     3     |   /29   |
| **Total** | **1314**  | **/21** |

Dari data tersebut tersusun tree subnet VLSM seperti berikut :

![VLSM](https://user-images.githubusercontent.com/55092974/145668448-bd8b95df-883f-4b5d-ba70-d3ceb5f6884d.JPG)

Dari subnet tree dapat ditentukan NID, SubnetMask, dan Broadcastnya. Seperti pada Gambar berikut :

![excel](https://user-images.githubusercontent.com/55092974/145668496-4c90792d-08d1-4c87-9527-0890c7011d60.JPG)

- Kemudian atur konfigurasi untuk tiap subnet yang telah ditentukan pada gambar diatas.

- FOOSHA

```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.177.0.1
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 192.177.0.5
	netmask 255.255.255.252
```

- WATER7

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.2
	netmask 255.255.255.252
        gateway 192.177.0.1

auto eth1
iface eth1 inet static
	address 192.177.0.129
	netmask 255.255.255.128

auto eth2
iface eth2 inet static
	address 192.177.0.9
	netmask 255.255.255.248

auto eth3
iface eth3 inet static
	address 192.177.4.1
	netmask 255.255.252.0
```
- GUANHAO

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.6
	netmask 255.255.255.252
        gateway 192.177.0.5

auto eth1
iface eth1 inet static
	address 192.177.2.1
	netmask 255.255.254.0

auto eth2
iface eth2 inet static
	address 192.177.0.17
	netmask 255.255.255.248

auto eth3
iface eth3 inet static
	address 192.177.1.1
	netmask 255.255.255.0
```

- DORIKI

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.11
	netmask 255.255.255.248
	gateway 192.177.0.9
```

- JIPANGU

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.10
	netmask 255.255.255.248
	gateway 192.177.0.9
```

- JORGE

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.19
	netmask 255.255.255.248
	gateway 192.177.0.17
```

- MAINGATE

```bash
auto eth0
iface eth0 inet static
	address 192.177.0.18
	netmask 255.255.255.248
	gateway 192.177.0.17
```

- BLUENO

```bash
auto eth0
iface eth0 inet dhcp
```

- CHIPER

```bash
auto eth0
iface eth0 inet dhcp
```

- ELENA

```bash
auto eth0
iface eth0 inet dhcp
```

- FUKUROU
```bash
auto eth0
iface eth0 inet dhcp
```
- Langkah selanjutnya yaitu melakukan routing pada router FOOSHA seperti berikut :

![routing](https://user-images.githubusercontent.com/55092974/145669036-8236eb02-204e-4527-ab3e-c00cadf24bab.JPG)

- Pada router `WATER7`, `FOOSHA`, dan `GUANHAO` install DHCP Relay dan update dengan melakukan perintah seperti dibawah ini:

```bash
apt-get update
apt-get install isc-dhcp-relay -y
```

- Setelah instalasi selesai, masukkan perintah `nano /etc/default/isc-dhcp-relay`. Sesuaikan isc-dhcp-relay seperti pada gambar berikut:
- pada FOOHSA
```bash
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.177.0.10"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
- pada WATER7 dan GUANHAO
```bash
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.177.0.10"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
- Kemudian lakukan perintah `service isc-dhcp-relay restart` pada FOOSHA, WATER7, dan GUANHAO.

- Seletah itu pada node JIPANGU install DHCP Server dan update seperti berikut :
```bash
apt-get update
apt-get install isc-dhcp-server -y
```
- Setelah instalasi selesai, masukkan perintah `nano /etc/default/isc-dhcp-server` Lalu sesuaikan isc-dhcp-server seperti pada gambar berikut:

![jipangu-1](https://user-images.githubusercontent.com/55092974/145669346-7a2a6095-18a5-4afe-aba4-e45f9162c02c.JPG)

- Kemudian atur konfigurasi subnet pada node JIPANGU dengan memasukkan perintah `nano /etc/dhcp/dhcp.conf` seperti gambar berikut :

![jipangu-2](https://user-images.githubusercontent.com/55092974/145669461-7c47c2a3-67b0-4b8b-b746-f224a2cdcafa.JPG)

![jipangu-3](https://user-images.githubusercontent.com/55092974/145669465-3ba83d17-97d9-42c7-96c5-cd99ba9912c4.JPG)

![jipangu-4](https://user-images.githubusercontent.com/55092974/145669472-f348f71f-887c-4a00-aad6-5729204e133f.JPG)

- Lalu lakukan perintah `service isc-dhcp-server restart`

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

### Jawaban

Masukkan perintah iptables berikut pada **FOOSHA**

```bash
iptables -t nat -A POSTROUTING -s 192.177.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.212 (IP ini dirubah setiap menjalankan node) 
```

untuk testing lakukan perintah `ping google.com` pada node lain terutama client. Misalkan disini kami melakukan testing pada node **BLUENO**

![1](https://user-images.githubusercontent.com/55092974/145669628-0b64b675-066d-405e-9548-7fe89434f326.JPG)

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

### Jawaban

Masukkan perintah iptables berikut pada **WATER7**

```bash
iptables -A FORWARD -d 192.177.0.8/29 -p tcp --dport 80 -j DROP
```

Untuk testingnya, lakukan perintah `nmap -p 80 192.177.0.10` pada client. Disini kami menggunakan **BLUENO**

![2](https://user-images.githubusercontent.com/55092974/145669761-ba6f5ca7-b31d-4c04-9cfc-2aae3c78609e.JPG)

Jika outputnya port 80 `filtered` maka berhasil `iptables`-nya.
