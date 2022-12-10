# Jarkom-Modul-5-E11-2022

<table>
<tr>
<th>Nama</th>
<th>NRP </th>
</tr>
<tr>
<td>Surya Abdillah</td>
<td>5025201229 </td>
</tr>
<tr>
<td>Muhammad Afdal Abdallah</td>
<td>5025201163 </td>
</tr>
<tr>
<td>Kadek Ari Dharmika</td>
<td>5025201239 </td>
</tr>
</table>

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




## Soal 4

> Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.

#### Pada Eden

```
iptables -A INPUT -s 10.27.7.136/29 -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 10.27.7.136/29 -j REJECT
```

#### Pada SSS & Garden

```
iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```

Untuk membatasi waktu kita menggunakan ```-m time``` dengan waktu mulai akses diassign dengan ```--timestart 07:00``` dan selesai pada ```--timestop 16:00``` serta hari dibatasi dari senin hingga jumat dengan ```--weekdays```. Untuk menentukan ```ACCEPT``` dan ```REJECT``` nya kita menggunakan flag ```-j```.

#### Testing saat di luar Weeksdays
![image](https://user-images.githubusercontent.com/90848018/206827886-c58f7fbb-67b1-4fa9-bd7b-cbee9e5b4372.png)

#### Testing saat weekdays
![image](https://user-images.githubusercontent.com/90848018/206828001-8479001e-30c3-409c-93e6-40d94b78b70c.png)

## Soal 5

> Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.

Pertama, konfigurasi untuk masing-masing node dengan port untuk request masing-masing node adalah 80 dan 443 dengan menggunakan ```--dport``` karena saat terjadi request akan terdistribus antara SSS & Garden. Untuk mendistribusikan maka menggunakan ```--every 2``` dan diarahkan pada node sasaran distribusi dengan ```--to-destination```.

#### Pada Ostania
```
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.27.7.138 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.27.7.138:80
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.27.7.138 -j DNAT --to-destination 10.27.7.139:80
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.27.7.139 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.27.7.139:443
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.27.7.139 -j DNAT --to-destination 10.27.7.138:443
```

#### Testing Request dari Blackbell ke Garden 
![image](https://user-images.githubusercontent.com/90848018/206828415-41786e0a-dede-487c-973f-36f03b43c0b5.png)

#### Testing Request dari Blackbell ke SSS
![image](https://user-images.githubusercontent.com/90848018/206828418-7be8704d-b3c6-4596-9356-f901fdcda5e7.png)

#### Index.html di Garden
![image](https://user-images.githubusercontent.com/90848018/206828452-632d5f42-a8d8-4307-a8c1-da7c9ac972c0.png)

#### Index.html di SSS
![image](https://user-images.githubusercontent.com/90848018/206828461-892e2532-8432-4e14-a6c9-81fe179b8376.png)

## Soal 6
> Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.

#### Pada WISE  
```
service isc-dhcp-server restart
service isc-dhcp-server restart

iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: "
iptables -A LOGGING -j DROP

service rsyslog restart
```

Pertama, kita restart dulu DHCP di ```WISE```. Lalu tambahkan syntax ```sylog``` untuk melihat paket yang di drop.

#### Pada Node lainnya
```

iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -N LOGGING
iptables -A INPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Rejected: "
iptables -A LOGGING -j REJECT

```

#### Pada   WISE
![image](https://user-images.githubusercontent.com/90848018/206829235-6d413fc6-cb1e-4107-9e71-ad5c17b326e9.png)

#### Pada SSS
![image](https://user-images.githubusercontent.com/90848018/206829276-2af5f393-3cbb-4798-aed6-5817b01f2cc6.png)

#### Saat packet drop di Blackbell
![image](https://user-images.githubusercontent.com/90848018/206829322-0031e208-304e-4680-b496-4093f4ecaefd.png)

#### Log packet drop di Blackbell
![image](https://user-images.githubusercontent.com/90848018/206829345-dc76d06f-3a7c-44d5-acae-a68b62d38964.png)



