# Komunikasi Data dan Jaringan Komputer
# Laporan Praktikum Modul 2 ITA07

# Anggota

| Nama                           | NRP          | 
| -------------------------------| -------------| 
| Naftali Salsabila Kanaya Putri    | `5027201012` | 
| Ariel Daffansyah Aliski           | `5027201058` | 
| Anak Agung Bintang Krisnadewi     | `5027201060` |

## Soal Nomor 1
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet. Maka konfigurasi terlebih dahulu sebagai berikut
<br/>
    - Ostania 
    ```
    
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
    address 10.43.1.1
    netmask 255.255.255.0

    auto eth2
    iface eth2 inet static
    address 10.43.2.1
    netmask 255.255.255.0

    auto eth3
    iface eth2 inet static
    address 10.43.3.1
    netmask 255.255.255.0```
<br/>
    - SSS
    ```
    
    auto eth0
    iface eth0 inet static
	address 10.43.1.2
	netmask 255.255.255.0
	gateway 10.43.1.1```
<br/>
    - Garden
    ```
    
    auto eth0
    iface eth0 inet static
    address 10.43.1.3
    netmask 255.255.255.0
    gateway 10.43.1.1```
<br/>
    - WISE
    ```
    
    auto eth0
    iface eth0 inet static
    address 10.43.2.2
    netmask 255.255.255.0
    gateway 10.43.2.1```
<br/>
    - Berlint
    ```
    
    auto eth0
    iface eth0 inet static
    address 10.43.3.2
    netmask 255.255.255.0
    gateway 10.43.3.1```
<br/>
    - Eden
    ```
    
    auto eth0
    iface eth0 inet static
    address 10.43.3.3
    netmask 255.255.255.0
    gateway 10.43.3.1```

Lalu kami menjalankan perintah pada router utama Ostania sebagai berikut
    `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.43.0.0/16`
Pada setiap node selain router utama Ostania kami jalankan perintah berikut
    `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
Lalu kita testing untuk Nodenya dan berhasil terkoneksi ke internet
    ![Soal 1](Soal1.png)
