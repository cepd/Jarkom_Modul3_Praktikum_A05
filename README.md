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
### 3. IP Tuntap : NID_tuntap_tiap_kelompok + 1
### 4. IP Interface Router SURABAYA :
- `eth0` : NID_tuntap_tiap_kelompok + 2
- `eth3` : NID_DMZ_tiap_kelompok + 1 
- `eth1` : 192.168.0.1
- `eth2` : 192.168.1.1
### 5. IP Server (SUBNET 2) :
- `MALANG` : NID_DMZ_tiap_kelompok + 2
- `MOJOKERTO` : NID_DMZ_tiap_kelompok + 3
- `TUBAN` : NID_DMZ_tiap_kelompok + 4

# Soal
## 1. Membuat topologi jaringan sesuai kriteria sebagai berikut :

![soal](https://user-images.githubusercontent.com/52326074/99972968-cd41fa00-2dd1-11eb-8f4c-c8bae1ae2851.jpg)

Dimana `SURABAYA` sebagai router, `MALANG` sebagai DNS Server, `TUBAN` sebagai DHCP server, serta `MOJOKERTO` sebagai Proxy server, dan UML lainnya sebagai client.

## 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client

## 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200

## 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70

## 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

## 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit

## 7. Akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. User autentikasi milik Anri memiliki format:
- User : `userta_yyy` -> `userta_a05`
- Password : `inipassw0rdta_yyy` -> `inipassw0rdta_a05`

## 8. Jadwal pengerjaan TA Anri setiap hari Selasa-Rabu pukul 13.00-18.00

## 9. Jadwal bimbingan TA dengan Bu Meguri setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00)

## 10. Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA

## 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut :

![403](https://user-images.githubusercontent.com/52326074/99974165-24949a00-2dd3-11eb-8f25-9fd6efa16e09.jpg)

`Note` : File error page bisa diunduh dengan cara `wget 10.151.36.202/ERR_ACCESS_DENIED`, tidak perlu di extract, cukup `cp -r`

## 12. Karena sama-sama pelupa, untuk memudahkan maka Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080

`Note` : Domain janganlupa-ta.yyy.pw -> `janganlupa-ta.a05.pw`

# Catatan
```
Jika tidak bisa dan menyerah untuk setup DHCP Server pada TUBAN (dengan relay pada SURABAYA),
maka setup DHCP pada SURABAYA (tanpa DHCP Relay). Pastinya nilai tidak akan maksimal.
```
