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


