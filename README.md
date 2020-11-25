# Jarkom_Modul3_Praktikum_A05
- 05111840000030 MUHAMMMAD DAFFA’ AFLAH SYARIF
- 05111840000163 PUTU PUTRI NATIH DEVAYANTI

# Teknis Pengerjaan
### 1. Default memori adalah 64M, kecuali :
- `SURABAYA` : 256M
- `MALANG` : 160M
- `MOJOKERTO` : 128M
- `TUBAN` : 128M
### 2. Menghitung dan menggunakan IP sesuai dengan NID DMZ masing-masing kelompok
### 3. IP Tuntap : NID_tuntap_tiap_kelompok + 1 -> 10.151.72.25
### 4. IP Interface Router SURABAYA :
- `eth0` : NID_tuntap_tiap_kelompok + 2 -> `10.151.72.26`
- `eth3` : NID_DMZ_tiap_kelompok + 1 -> `10.151.73.49`
- `eth1` : 192.168.0.1
- `eth2` : 192.168.1.1
### 5. IP Server (SUBNET 2) :
- `MALANG` : NID_DMZ_tiap_kelompok + 2 -> `10.151.73.50`
- `MOJOKERTO` : NID_DMZ_tiap_kelompok + 3 -> `10.151.73.51`
- `TUBAN` : NID_DMZ_tiap_kelompok + 4 -> `10.151.73.52`

# Soal
## 1. Membuat topologi jaringan sesuai kriteria sebagai berikut :

![soal](https://user-images.githubusercontent.com/52326074/99972968-cd41fa00-2dd1-11eb-8f4c-c8bae1ae2851.jpg)

Dimana `SURABAYA` sebagai router, `MALANG` sebagai DNS Server, `TUBAN` sebagai DHCP server, serta `MOJOKERTO` sebagai Proxy server, dan UML lainnya sebagai client.

![1topologi](https://user-images.githubusercontent.com/52326074/100208977-e8307d80-2f3b-11eb-8305-194d9ab95949.png)

Setelah `topologi.sh` sudah terbuat, kemudian mengatur interface untuk masing-masing UML seperti berikut :

- Pada router `SURABAYA` melakukan setting sysctl dengan perintah `nano /etc/sysctl.conf`
- Kemudian uncommand pada bagian `net.ipv4.ip_forward=1`
- Selanjutnya ketik `sysctl -p` untuk mengaktifkan perubahan yang ada
- Lalu setting IP setiap UML dengan perintah `nano /etc/network/interfaces`

![sby1](https://user-images.githubusercontent.com/52326074/100209998-119dd900-2f3d-11eb-842b-8fa7b1be0ff4.jpg)
![sby2](https://user-images.githubusercontent.com/52326074/100210002-12cf0600-2f3d-11eb-9a33-7cc26800c4ba.jpg)
![mlg](https://user-images.githubusercontent.com/52326074/100210024-16fb2380-2f3d-11eb-9745-f1874ca9c41a.jpg)
![mjk](https://user-images.githubusercontent.com/52326074/100210019-16628d00-2f3d-11eb-9f03-6a93937247fa.jpg)
![tbn](https://user-images.githubusercontent.com/52326074/100210005-14003300-2f3d-11eb-981e-d9bf7ccc7942.jpg)
![gsk](https://user-images.githubusercontent.com/52326074/100210010-15316000-2f3d-11eb-8418-4338d17ce2ae.jpg)
![sdj](https://user-images.githubusercontent.com/52326074/100210003-13679c80-2f3d-11eb-93f5-597c286b6db6.jpg)
![bwi](https://user-images.githubusercontent.com/52326074/100210009-1498c980-2f3d-11eb-8fdb-3cffc5689eb4.jpg)
![mdi](https://user-images.githubusercontent.com/52326074/100210013-15316000-2f3d-11eb-9f8a-c7cf65071bd2.jpg)

- Kemudian restart network dengan perintah `service networking restart`
- Lalu cek pada setiap UML dengan perintah `ifconfig`
- Selanjutnya pad UML `SURABAYA` menjalankan `iptables.sh` yang didalamnya terdapat sebagai berikut :
```
iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16
iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.1.0/16
```

- Untuk mencoba apakah bisa ping keluar maka jalankan perintah `ping google.com` dan jika berhasil maka settingan sudah benar
- Kemudian export proxy untuk semua UML dengan menjalankaan perintah `source proxy.sh` yang didalamnya terdapat sebagai berikut :
```
export http_proxy=”http://DPTSI-562908-d1f98:90660@proxy.its.ac.id:8080”
export https_proxy=”http://DPTSI-562908-d1f98:90660@proxy.its.ac.id:8080”
export ftp_proxy=”http://DPTSI-562908-d1f98:90660@proxy.its.ac.id:8080”
```

- Selanjutnya melakukan update untuk setiap UML dengan perintah `apt-get update`

## 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client
Pada UML `SURABAYA` dilakukan hal berikut :

- menjalankan perintah `nano /etc/sysctl.conf`
- kemudian didalam file tersebut dilakukan perubahan dibagian `net.ipv4.conf.all.accept_source_route = 1` yang awalnya net.ipv4.conf.all.accept_source_route = 0

Kemudian pada UML `TUBAN` dilakukan perintah berikut :

- menjalankan perintah `apt-get update`
- kemudian jalankan perintah `apt-get install isc-dhcp-server -y`

![2installdhcpstbn](https://user-images.githubusercontent.com/52326074/100212103-86721280-2f3f-11eb-995f-f5c1921505f0.jpg)

Kemudian pada UML `SURABAYA` dilakukan perintah berikut :

- menjalankan perintah `apt-get update`
- kemudian jalankan perintah `apt-get install isc-dhcp-relay -y`

![2installdhcprsby1](https://user-images.githubusercontent.com/52326074/100212095-84a84f00-2f3f-11eb-86cb-9449bf6b6260.png)
![2installdhcprsby2](https://user-images.githubusercontent.com/52326074/100212100-85d97c00-2f3f-11eb-81d6-b341fe5643ae.jpg)

Selanjutnya melakukan konfigurasi DHCP Relay untuk `SURABAYA` dengan perintah `nano /etc/default/isc-dhcp-relay`

![2dhcpr](https://user-images.githubusercontent.com/52326074/100212105-870aa900-2f3f-11eb-991e-f2edb371fbf8.jpg)

Didalam soal dikatakan ada kriteria lain yang dimintauntuk topologi jaringan, salah satunya `TIDAK DIPERBOLEHKAN` menggunakan konfigurasi IP statis untuk semua UML Client. Berikut adalah salah satu bukti untuk Client tidak diperbolehkan menggunakan IP statis dalam konfigurasinya

![TDKBOLEHstatic](https://user-images.githubusercontent.com/52326074/100212798-61ca6a80-2f40-11eb-9c9a-09b288ca301f.jpg)
![TDKBOLEHstatic2](https://user-images.githubusercontent.com/52326074/100212801-62fb9780-2f40-11eb-9d1a-485822a26888.jpg)

## 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200
Yang pertama dilakukan yaitu melakukan konfigurasi interface DHCP Server pada `TUBAN` dengan perintah `nano /etc/default/isc-dhcp-server`. Kemudian mengisikan interface `eth0` untuk diberikan layanan DHCP.

![3configinterfacedhcpstbn](https://user-images.githubusercontent.com/52326074/100213249-fb921780-2f40-11eb-856a-aa08d18ddb2e.jpg)

Kemudian melakukan konfigurasi DHCP Server pada `TUBAN` dengan menjalankan perintah `nano /etc/dhcp/dhcpd.conf` lalu ditambahkan konfigurasi sebagai berikut :
```
subnet 10.151.73.48 netmask 255.255.255.248 {
    range 10.151.73.49 10.151.73.54;
    option routers 10.151.73.52; #interface TUBAN
    option broadcast-address 10.151.73.55;
    option domain-name-servers 202.46.129.2, 10.151.71.18;
    default-lease-time 3600;
    max-lease-time 3600;
}
```

![3configdhcpstbn](https://user-images.githubusercontent.com/52326074/100213239-f9c85400-2f40-11eb-8c56-3f7fcaebf17a.jpg)

Lalu untuk Client pada subnet 1 mendapatkan range IP dari `192.168.0.10` sampai `192.168.0.100` dan `192.168.0.110` sampai `192.168.0.200` dengan konfigurasi sebagai berikut :
```
//subnet1
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1; #interface SURABAYA subnet1 eth1
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 300;
    max-lease-time 3600;
}
```

![356](https://user-images.githubusercontent.com/52326074/100213250-fc2aae00-2f40-11eb-8260-f4d12cc1e65a.jpg)

## 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70
Client pada subnet 3 mendapatkan range IP dari `192.168.1.50` sampai `192.168.1.70` dengan konfigurasi sebagai berikut :
```
//subnet3
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.70;
    option routers 192.168.1.1; #interface SURABAYA subnet3 eth2
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 600;
    max-lease-time 7200;
}
```

![456](https://user-images.githubusercontent.com/52326074/100214351-5ed07980-2f42-11eb-9fe4-8e27e31f936c.jpg)

## 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
Client mendapatkan DNS Malang dan DNS `202.46.129.2` dari DHCP
```
//subnet1
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1; #interface SURABAYA subnet1 eth1
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 300;
    max-lease-time 3600;
}
//subnet3
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.70;
    option routers 192.168.1.1; #interface SURABAYA subnet3 eth2
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 600;
    max-lease-time 7200;
}
```

![356](https://user-images.githubusercontent.com/52326074/100213250-fc2aae00-2f40-11eb-8260-f4d12cc1e65a.jpg)
![456](https://user-images.githubusercontent.com/52326074/100214351-5ed07980-2f42-11eb-9fe4-8e27e31f936c.jpg)

## 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit
Client di subnet 1 mendapatkan peminjaman alamat IP selama `5 menit`, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama `10 menit`
```
//subnet1
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1; #interface SURABAYA subnet1 eth1
    option broadcast-address 192.168.0.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 300;
    max-lease-time 3600;
}
//subnet3
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.70;
    option routers 192.168.1.1; #interface SURABAYA subnet3 eth2
    option broadcast-address 192.168.1.255;
    option domain-name-servers 202.46.129.2, 10.151.73.50;
    default-lease-time 600;
    max-lease-time 7200;
}
```

![356](https://user-images.githubusercontent.com/52326074/100213250-fc2aae00-2f40-11eb-8260-f4d12cc1e65a.jpg)
![456](https://user-images.githubusercontent.com/52326074/100214351-5ed07980-2f42-11eb-9fe4-8e27e31f936c.jpg)

## 7. Akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. User autentikasi milik Anri memiliki format sebagai berikut :
- User : `userta_yyy` -> `userta_a05`
- Password : `inipassw0rdta_yyy` -> `inipassw0rdta_a05`

Pertama yang dilakukan pastinya menginstall `squid3` di `MOJOKERTO` dengan perintah `apt-get install squid3`, kemudian memastikan instalasi squid3 berhasil atau tidak dengan perintah `service squid3 status`.

Kemudian melakukan konfigurasi dasar `squid3` dengan langkahlangkah sebagai berikut :

- membackup file default yang disediakan squid3 dengan perintah `mv /etc/squid3/squid.conf /etc/squid3/squid.conf.bak`
- kemudian buat konfigurasi baru dengan perintah `nano /etc/squid3/squid.conf` yang didalamnya sebagai berikut :
```
http_port 8080
visible_hostname mojokerto
```

![7-1](https://user-images.githubusercontent.com/52326074/100224061-e3c19000-2f4e-11eb-8e5f-3fbf5139e45e.jpg)

- lalu restart squid3 dengan perintah `service squid3 restart`
- langkah selanjutnya yaitu mengatur proxy browser pada device dengan IP Address `10.151.73.51` dan port `8080`

![7-2](https://user-images.githubusercontent.com/52326074/100224062-e45a2680-2f4e-11eb-9f1d-5a857b6557db.jpg)

- selanjutnya coba akses website `its.ac.id` menggunakan incognito, dan hasilnya sebagai berikut.

![7-3](https://user-images.githubusercontent.com/52326074/100224063-e4f2bd00-2f4e-11eb-9fc9-35704c3d9b94.jpg)

- karena dalam mengakses website `its.ac.id` terkendala error maka menambahkan konfigurasi `http_access allow all`

![7-4](https://user-images.githubusercontent.com/52326074/100224045-e02e0900-2f4e-11eb-9815-bedc51fb6d41.jpg)

- kemudian simpan hasail konfigurasi dan restart squid3 dengan perintah `service squid3 restart`

![7-5](https://user-images.githubusercontent.com/52326074/100224052-e1f7cc80-2f4e-11eb-868a-9116b71fcca7.jpg)

Kemudian selanjutnya mengatur user login dengan langkah-langkah sebagai berikut :

- install `apache2-utils` pada UML `MOJOKERTO` dengan perintah `apt-get install apache2-utils`. Sebelumnya kalian sudah harus melakukan apt-get update
- kemudian membuat user dan password dengan perintah `htpasswd -c /etc/squid/passwd userta_a05` dengan password : `inipassw0rdta_a05`

![7-5](https://user-images.githubusercontent.com/52326074/100224052-e1f7cc80-2f4e-11eb-868a-9116b71fcca7.jpg)

- kemudian edit konfigurasi dengan perintah `nano /etc/squid3/squid.conf` yang didalamnya sebagai berikut.
```
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

![7-6](https://user-images.githubusercontent.com/52326074/100224055-e2906300-2f4e-11eb-9565-23426dfd085d.jpg)

- kemudian restart squid3 dengan perintah `service squid3 restart` dan akan muncul user autentifikasi seperti berikut.

![7-7](https://user-images.githubusercontent.com/52326074/100224058-e328f980-2f4e-11eb-89bf-7cbf306baf67.jpg)

## 8. Jadwal pengerjaan TA Anri setiap hari Selasa-Rabu pukul 13.00-18.00
- membuat file baru bernama `acl.conf` dengan perintah `nano /etc/squid3/acl.conf` yang didalamnya sebagai berikut :
```
acl AVAILABLE_WORKING time TW 13:00-18:00
```

- kemudian buka file `squid.conf` dengan perintah `nano /etc/squid3/squid.conf` dan diubah konfigurasinya menjadi seperti berikut :
```
include /etc/squid3/acl.conf

http_port 8080
http_access allow AVAILABLE_WORKING
http_access deny all
visible_hostname mojokerto
```

- kemudian simpan file tersebut dan restart dengan perintah `service squid3 restart`

## 9. Jadwal bimbingan TA dengan Bu Meguri setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00)
- menambahkan konfigurasi pada `acl.conf` dengan perintah `nano /etc/squid3/acl.conf` yang didalamnya sebagai berikut :
```
acl AVAILABLE_WORKING time TW 13:00-18:00
acl AVAILABLE_WORKING2 time TWH 21:00-23:59
acl AVAILABLE_WORKING3 time WHF 00:00-09:00
```

- kemudian buka file `squid.conf` dengan perintah `nano /etc/squid3/squid.conf` dan menambah konfigurasinya menjadi seperti berikut :
```
include /etc/squid3/acl.conf

http_port 8080
http_access allow AVAILABLE_WORKING
http_access allow AVAILABLE_WORKING2
http_access allow AVAILABLE_WORKING3
http_access deny all
visible_hostname mojokerto
```

- kemudian simpan file tersebut dan restart dengan perintah `service squid3 restart`

Berikut hasil konfigurasi untuk no 8 dan 9 :

![89-1](https://user-images.githubusercontent.com/52326074/100226460-5e3fdf00-2f52-11eb-990b-d83a27db13f7.jpg)
![89-2](https://user-images.githubusercontent.com/52326074/100226456-5d0eb200-2f52-11eb-8f69-f5250e0b3bbe.jpg)

## 10. Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA

## 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut :

![403](https://user-images.githubusercontent.com/52326074/99974165-24949a00-2dd3-11eb-8f25-9fd6efa16e09.jpg)

`Note` :

File error page bisa diunduh dengan cara `wget 10.151.36.202/ERR_ACCESS_DENIED`, tidak perlu di extract, cukup `cp -r`

## 12. Karena sama-sama pelupa, untuk memudahkan maka Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080

`Note` :

Domain janganlupa-ta.yyy.pw -> `janganlupa-ta.a05.pw`

# Catatan
```
Jika tidak bisa dan menyerah untuk setup DHCP Server pada TUBAN (dengan relay pada SURABAYA),
maka setup DHCP pada SURABAYA (tanpa DHCP Relay). Pastinya nilai tidak akan maksimal.
```
