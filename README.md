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

```
iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```

Untuk membatasi waktu kita menggunakan ```-m time``` dengan waktu mulai akses diassign dengan ```--timestart 07:00``` dan selesai pada ```--timestop 16:00``` serta hari dibatasi dari senin hingga jumat dengan ```--weekdays```. Untuk menentukan ```ACCEPT``` dan ```REJECT``` nya kita menggunakan flag ```-j```.

### Testing saat di luar Weeksdays
![image](https://user-images.githubusercontent.com/90848018/206827886-c58f7fbb-67b1-4fa9-bd7b-cbee9e5b4372.png)

### Testing saat weekdays
![image](https://user-images.githubusercontent.com/90848018/206828001-8479001e-30c3-409c-93e6-40d94b78b70c.png)
