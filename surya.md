# A. Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Loid dibawah ini:

<img width="410" alt="image" src="https://user-images.githubusercontent.com/103357229/206827578-ef4176ac-64ed-4e0f-98be-8ac3374031e9.png">

Keterangan:
- Eden adalah DNS Server
- WISE adalah DHCP Server
- Garden dan SSS adalah Web Server
- Jumlah Host pada Forger adalah 62 host
- Jumlah Host pada Desmond adalah 700 host
- Jumlah Host pada Blackbell adalah 255 host
- Jumlah Host pada Briar adalah 200 host

> Hasil pembuatan topologi, dan penentuan subnet

<img width="481" alt="image" src="https://user-images.githubusercontent.com/103357229/206827616-9b6710d5-9731-4772-8b4c-71326342d44c.png">


# B. Untuk menjaga perdamaian dunia, Loid ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM setelah melakukan subnetting.

> Pada praktik, kelompok kami menggunakan teknik VLSM.

## Perhitungan

| SUBNET | IP ADDRESS NEEDED    | NETMASK | SUbnet Mask | Wildcard |
| :---:   | :---: | :---: | :---: | :---: |
| A1 | 3 | /29 | 255.255.255.248 | 0.0.0.7 |
| A2 | 63 | /25 | 255.255.255.128 | 0.0.0.127 |
| A3 | 701 | /22 | 255.255.252.0 | 0.0.3.255 |
| A4 | 2 | /30 | 255.255.255.252 | 0.0.0.3 |
| A5 | 2 | /30 | 255.255.255.252 | 0.0.0.3 |
| A6 | 256 | /23 | 255.255.254.0 | 0.0.1.255 |
| A7 | 201 | /24 | 255.255.255.0 | 0.0.0.255 |
| A8 | 3 | /29 | 255.255.255.248 | 0.0.0.7 |

> Didapatkan total IP address yang diperlukan adalah 1231, yakni netmask /21.

## Pembagian IP

Dari perhitungan di atas, didapati bahwa netmask total adalah /21, sehingga bisa didapati pembagian seperti tree berikut:

![Modul5-Page-2 drawio (2)](https://user-images.githubusercontent.com/103357229/206828382-67fa32d1-cd99-473d-bb76-3e85629579b3.png)

> Sehingga, didapatkan pembagian IP sesuai dengan tabel berikut:

| SUBNET | NID  | Broadcast | Netmask |
| :---:   | :---: | :---: | :---: |
| A1 | 10.27.7.128 | 255.255.255.248 | 10.27.7.135 |
| A2 | 10.27.7.0 | 255.255.255.128 | 10.27.7.127 |
| A3 | 10.27.0.0 | 255.255.252.0 | 10.27.3.255 |
| A4 | 10.27.7.144 | 255.255.255.252 | 10.27.7.147 |
| A5 | 10.27.7.148 | 255.255.255.252 | 10.27.7.151 |
| A6 | 10.27.4.0 | 255.255.254.0 | 10.27.5.255 |
| A7 | 10.27.6.0 | 255.255.255.0 | 10.27.6.255 |
| A8 | 10.27.7.136 | 255.255.255.248 | 10.27.7.143 |

## Aplikasi pada GNS3

> Pada contoh di bawah, telah dilakukan penyesuaian untuk soal-soal selanjutnya, sebagai contoh penyesuaian untuk DHCP.

### Strix

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether b6:23:6b:b5:5d:33

auto eth1
iface eth1 inet static
address 10.27.7.149
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 10.27.7.145
netmask 255.255.255.252
```

### Westalis

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.27.7.146
netmask 255.255.255.252
gateway 10.27.7.145

auto eth1
iface eth1 inet static
address 10.27.0.1
netmask 255.255.252.0

auto eth2
iface eth2 inet static
address 10.27.7.1
netmask 255.255.255.128

auto eth3
iface eth3 inet static
address 10.27.7.129
netmask 255.255.255.248
```

### Ostania

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.27.7.150
netmask 255.255.255.252
gateway 10.27.7.149

auto eth1
iface eth1 inet static
address 10.27.4.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 10.27.7.137
netmask 255.255.255.248

auto eth3
iface eth3 inet static
address 10.27.6.1
netmask 255.255.255.0
```

### Eden

```
auto eth0
iface eth0 inet static
address 10.27.7.130
netmask 255.255.255.248
gateway 10.27.7.129
```

### WISE

```
auto eth0
iface eth0 inet static
address 10.27.7.131
netmask 255.255.255.248
gateway 10.27.7.129
```

### Forger

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

### Desmond

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

### Blackbell

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

### GArden

```
auto eth0
iface eth0 inet static
address 10.27.7.138
netmask 255.255.255.248
gateway 10.27.7.137
```

### SSS

```
auto eth0
iface eth0 inet static
address 10.27.7.139
netmask 255.255.255.248
gateway 10.27.7.137
```

### Briar

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

# C. Anya, putri pertama Loid, juga berpesan kepada anda agar melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

> Routing dilakukan pada router Strix, default routing pada Ostania dan Westalis sudah muncul secara otomatis

### Strix

```
route add -net 10.27.7.128 netmask 255.255.255.248 gw 10.27.7.146
route add -net 10.27.7.0 netmask 255.255.255.128 gw 10.27.7.146
route add -net 10.27.0.0 netmask 255.255.252.0 gw 10.27.7.146

route add -net 10.27.4.0 netmask 255.255.254.0 gw 10.27.7.150
route add -net 10.27.6.0 netmask 255.255.255.0 gw 10.27.7.150
route add -net 10.27.7.136 netmask 255.255.255.248 gw 10.27.7.150
```

### TESTING

> Strix menuju WISE

<img width="393" alt="image" src="https://user-images.githubusercontent.com/103357229/206828795-1970dd74-7d02-4d0a-9489-39aa4c92577a.png">

> Strix menuju SSS

<img width="422" alt="image" src="https://user-images.githubusercontent.com/103357229/206828837-56a0f282-aeff-4612-8120-a222891e1b68.png">

> SSS menuju WISE

<img width="426" alt="image" src="https://user-images.githubusercontent.com/103357229/206828862-ef98cd50-b21d-4154-a6f5-6adfd75046fd.png">


