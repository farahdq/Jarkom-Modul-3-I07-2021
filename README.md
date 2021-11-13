# Jarkom-Modul-3-I07-2021

# Laporan Resmi Modul 3 Jaringan Komputer I07

### Ascarya Arkaandhiyaa Allaam	  (05111942000027)

### Farah Dhiah Qorirah 		       (05111942000018)

### Julius Adetya Eka Bhaswara 	  (05111942000026)

## Topology 

![Screenshot 2021-11-13 095532](https://user-images.githubusercontent.com/73812417/141603253-010e7a88-3a5b-4853-8143-b5024110497f.png)

## NO 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server 

## Jawaban

EniesLobby sebagai DNS Server.
Gunakan command 'apt-get install bind9'.

Pada .bashrc

![image](https://user-images.githubusercontent.com/77782259/141472142-891ac670-97eb-4ec8-a4dd-8257c7b26542.png)

Jipangu sebagai DHCP Server.
Gunakan 'apt-get install isc-dhcp-server'.

Pada .Bashrc

![image](https://user-images.githubusercontent.com/77782259/141472266-6da7944e-cd53-4ef7-818f-50f249242742.png)

Water7 sebagai Proxy Server. G
unakan 'apt-get install squid'.

Pada .bashrc

![image](https://user-images.githubusercontent.com/77782259/141472465-99aa0e93-3988-4072-8011-9943914b8f4c.png)

## NO 2
Foosha sebagai DHCP Relay 

## Jawaban

Foosha sebagai DHCP Relay 
Gunakan 'apt-get install isc-dhcp-relay'.

Pada foosha, tambahkan pada file '/etc/default/isc-dhcp-relay' menjadi:

```shell
# What servers should the DHCP relay forward requests to?
SERVERS="10.41.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

SERVERS mengarah ke jipangu yaitu '10.41.2.4' dan INTERFACES kepada 'eth1 eth2 eth3'.
Kemudian restart DHCP relay dengan command '/etc/init.d/isc-dhcp-relay restart'.

Pada script.sh

[![image.png](https://i.postimg.cc/SQwGMBCs/image.png)](https://postimg.cc/hh1xF3CW)

Pada .bashrc

[![image.png](https://i.postimg.cc/dtLhm3Rm/image.png)](https://postimg.cc/6yx6B9my)

## NO 3,4,5,6

Ada beberapa kriteria yang ingin dibuat oleh Luffy dan Zoro, yaitu:
Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169 (3)
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50 (4)
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut. (5)
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit. (6)

Pada jipangu, dalam '/etc/default/isc-dhcp-server' tambahkan 'INTERFACES="eth0"' terlebih dahulu.

### NO 3
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169 

### Jawaban

Pada subnet '10.41.1.0' terminal 1 tambahkan:

```shell
    range 10.41.1.20 10.41.1.99;
    range 10.41.1.150 10.41.1.169;
```

### NO 4 
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

### Jawaban

Pada subnet '10.41.3.0' terminal 3 tambahkan:

```shell
    range 10.41.3.30 10.41.3.50;
```

### NO 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

### Jawaban

Pada kedua subnet '10.41.1.0' terminal 1 dan '10.41.3.0' terminal 3 tambahkan:

```shell
    option domain-name-servers 10.41.2.2, 192.168.122.1
```

### NO 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

### Jawaban

Pada subnet '10.41.1.0' terminal 1 ubah 'default-lease-time' dan 'max-lease-time' menjadi:

```shell
    default-lease-time 360;
    max-lease-time 7200;
```

Pada subnet '10.41.3.0' terminal 3 ubah 'default-lease-time' dan 'max-lease-time' menjadi:

```shell
    default-lease-time 720;
    max-lease-time 7200;
```
###
Kemudian restart isc-DHCP menggunakan command 'service isc-dhcp-server restart'.

Pada setiap client, dalam file '/etc/network/interfaces' tambahkan:

```shell
auto eth0
iface eth0 inet dhcp
```

Checking pada 'Loguetown' pada terminal 1.
Menggunakan command 'ip a', dapat kita lihat IP adalah '10.41.1.32' dimana masih dalam range 1.20 - 1.99.

[![image.png](https://i.postimg.cc/mDtVwGT5/image.png)](https://postimg.cc/VrPjfprq)

Lease time 360 (6 menit).

[![image.png](https://i.postimg.cc/X7VLHFN5/image.png)](https://postimg.cc/py6KymFV)

Checking pada 'TottoLand' pada terminal 3.
Menggunakan command 'ip a', dapat kita lihat IP adalah '10.41.3.34' dimana masih dalam range 3.30 - 3.50.

[![image.png](https://i.postimg.cc/FR3pqVJy/image.png)](https://postimg.cc/ZBY6v3Hn)

Lease time 720 (12 menit).

[![image.png](https://i.postimg.cc/mkVYfXZH/image.png)](https://postimg.cc/LgYYjV19)

###

Pada .bashrc

[![image.png](https://i.postimg.cc/Kz2BKSRT/image.png)](https://postimg.cc/3ytd6qjr)

Pada script.sh

[![image.png](https://i.postimg.cc/DyJ0Kpz3/image.png)](https://postimg.cc/VdwmqRMK)
[![image.png](https://i.postimg.cc/cHCZB6gG/image.png)](https://postimg.cc/McChW6m9)

##

## NO 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69

## NO 8
Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000 

## NO 9
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy 

## NO 10 
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

## NO 11
Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie 

## NO 12
Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps

## NO 13
Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya
