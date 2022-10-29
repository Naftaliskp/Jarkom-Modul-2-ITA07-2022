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
netmask 255.255.255.0
```

    - SSS
```    
auto eth0
iface eth0 inet static
address 10.43.1.2
netmask 255.255.255.0
gateway 10.43.1.1
```

    - Garden
```    
auto eth0
iface eth0 inet static
address 10.43.1.3
netmask 255.255.255.0
gateway 10.43.1.1
```

    - WISE 
```    
auto eth0
iface eth0 inet static
address 10.43.2.2
netmask 255.255.255.0
gateway 10.43.2.1
```

    - Berlint    
```    
auto eth0
iface eth0 inet static
address 10.43.3.2
netmask 255.255.255.0
gateway 10.43.3.1
```

    - Eden    
```
auto eth0
iface eth0 inet static
address 10.43.3.3
netmask 255.255.255.0
gateway 10.43.3.1
```

Lalu kami menjalankan perintah pada router utama Ostania sebagai berikut
    `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.43.0.0/16`
    <br/>
Pada setiap node selain router utama Ostania kami jalankan perintah berikut
    `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
    <br/>
Lalu kita testing untuk Nodenya dan berhasil terkoneksi ke internet
    ![Soal 1](Soal1.png)

## Soal Nomor 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, maka dibuat website utama dengan akses `wise.yyy.com` dengan alias `www.wise.yyy.com` pada folder WISE

Untuk mengerjakan nomor 2, langkah-langkahnya adalah sebagai berikut
<br/>

    - Install bind
```    
apt-get update
apt-get install bind9
```

    - Buka konfigurasi file `/etc/bind/named.conf.local` tambahkan sebagai berikut
```    
zone "wise.ita07.com" {
type master;
file "/etc/bind/wise/wise.ita07.com";
};
```

Lalu buat folder wise dengan `mkdir wise` pada folder bind
<br/> 
Lalu buat file konfigurasinya pada folder `/etc/bind/wise/wise.ita07.com`
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.ITA07.com. root.wise.ITA07.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.ITA07.com.
@               IN      A       10.43.2.2
eden            IN      A       10.43.3.3
www             IN      CNAME   wise.ITA07.com.
www.eden        IN      CNAME   eden.wise.ITA07.com.
```

Setelah itu kita coba apakah berhasil membuka webservernya dari Node lain, di sini menggunakan node SSS. Tambahkan nameserver terlebih dahulu
`echo "nameserver 10.43.2.2" > /etc/resolv.conf`
<br/>
Lalu Testing menggunakan komputer SSS
    ![Soal 2](Soal2.png)
