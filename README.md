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

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop

### Jawaban

Untuk Doriki dan Jipangu, masukkan perintah berikut :
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Untuk melakukan testing, ping dilakukan ke host-host yang ada. 


## Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

### Jawaban
Masukkan perintah iptables berikut pada Doriki

```bash
iptables -A INPUT -s 192.177.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.177.0.128/25 -j REJECT
iptables -A INPUT -s 192.177.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.177.4.0/22 -j REJECT
```

### Testing
untuk testing bisa dilakukan `ping 192.177.0.11` pada waktu antara 07.00 hingga 15.00 hari senin sampai kamis. Dilakukan perintah `date` terlebih dahulu untuk mengecek waktu sekarang. Jika sesuai maka ping akan berhasil dan terlihat seperti berikut :

![image](https://user-images.githubusercontent.com/81372291/145679105-25f07b6e-e5bd-4a34-8600-7ba0c47e8728.png)

selain pada waktu itu ping akan direject dan akan terlihat sebagai berikut :

![image](https://user-images.githubusercontent.com/81372291/145679088-b99cc6dd-9f63-4df3-a737-c7e9d86b6d87.png)


## Soal 5
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

### Jawaban
Masukkan perintah iptables berikut pada Doriki

```bash
iptables -A INPUT -s 192.177.2.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 192.177.2.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.177.1.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 192.177.1.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.177.1.0/24 -j REJECT
iptables -A INPUT -s 192.177.2.0/23 -j REJECT
```

### Testing
untuk testing bisa dilakukan `ping 192.177.0.11` pada waktu antara 15.01 hingga 06.59. Dilakukan perintah `date` terlebih dahulu untuk mengecek waktu sekarang. Jika sesuai maka ping akan berhasil dan terlihat seperti berikut :
![image](https://user-images.githubusercontent.com/81372291/145673170-c3bb8256-15e0-4c00-bd00-bad48b6d7aea.png)


Untuk mengetes apakah selain antara 15.01 - 06.59 akan direject, bisa mengganti date menggunakan perintah `date -s "Mon Dec  6 09:36:18 UTC 2021"` dan melakukan ping lagi. Jika direject maka akan terlihat seperti berikut :

![image](https://user-images.githubusercontent.com/81372291/145673263-9276ee7c-7d97-49b5-a15e-9244f5ff0b0c.png)

## Soal 6

```bash
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.
```
Masukkan perintah - perintah iptables berikut pada **Guanhao**

```bash
iptables -A PREROUTING -t nat -d 192.177.0.10 -p tcp -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.177.0.18
iptables -A PREROUTING -t nat -d 192.177.0.10 -p tcp -j DNAT --to-destination 192.177.0.19
```

### Testing

- Pertama - tama, pada kedua node web server jorge dan maingate install service apache dengan menjalankan perintah berikut.

  ```bash
    apt-get update
    apt-get install apache2 -y
  ```

- Setelah berhasil diinstall, edit file _/var/www/html/index.html_ pada kedua node menjadi seperti pada gamber berikut. (Sesuaikan nama node-nya).

  - Maingate dan Jorge

![6-2](https://user-images.githubusercontent.com/55092974/145681678-d81d3d55-a880-4232-a5cc-5b6fc678262e.JPG)


- Kemudian restart service apache pada kedua node web server dengan menggunakan perintah :

  ```bash
  service apache2 restart
  ```

- Setelah kedua web server berhasil disiapkan, coba akses ke node Doriki (DNS Server) lewat salah satu node yang berhubungan langsung dengan router **Guanhao** (pada kasus ini digunakan node Elena) dengan menggunakan perintah :

  ```bash
  curl 192.177.0.11
  ```
- Pada Testing pertama akses ke Doriki (DNS Server), paket dialihkan ke node Web Server maingate. Perintahkan lagi ke node Doriki (DNS Server) dengan menggunakan perintah yang sama dengan yang diatas.

- maka akses ke Doriki sekarang dialihkan ke node Web Server Jorge. Jika dicoba terus menerus akses ke Doriki, maka paket akan terus menerus dialihkan secara bergantian ke Jorge atau Maingate. Dengan begitu, artinya konfigurasi yang diinginkan telah berhasil diterapkan.

![nomer 6](https://user-images.githubusercontent.com/55092974/145681695-56b1d72e-1d46-4846-a627-f90e6e98a644.JPG)



