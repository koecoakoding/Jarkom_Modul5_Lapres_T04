# Jarkom_Modul5_Lapres_T04

Fachrizal Rahmat Hidayat / 05311840000005
Faza Murtadho            / 05311840000034

## A. Topologi

![gambar](https://user-images.githubusercontent.com/55182321/103165777-52e01c00-484e-11eb-865c-bbc69d98d5bf.png)

Membuat Topologi dengan ketentuan:

SURABAYA diberikan IP TUNTAP

MALANG merupakan DNS Server diberikan IP DMZ

MOJOKERTO merupakan DHCP Server diberikan IP DMZ

MADIUN dan PROBOLINGGO merupakan WEB Server

Setiap Server diberikan memory sebesar 128M

Client dan Router diberikan memori sebesar 96M

Jumlah host pada subnet SIDOARJO 200 Host

Jumlah host pada subnet GRESIK 210 Host


Dan berikut adalah file topologi.sh yang kami buat
![gambar](https://user-images.githubusercontent.com/55182321/103165951-7015ea00-4850-11eb-9dc2-2fa2cff00a73.png)


## B. Subnetting

Pada bagian subnetting, kami menggunakan VLSM dan pembagianya adalah sebagai berikut
![gambar](https://user-images.githubusercontent.com/55182321/103165622-d26ceb80-484c-11eb-8001-0407dac15cb6.png)


## C. Routing

Routing pada masing masing router adalah sebagai berikut 
### SURABAYA
![gambar](https://user-images.githubusercontent.com/55182321/103165906-f2ea7500-484f-11eb-8e01-9d04e7de11c0.png)
![gambar](https://user-images.githubusercontent.com/55182321/103165913-01389100-4850-11eb-8874-1bad6c39eda2.png)

### BATU
![gambar](https://user-images.githubusercontent.com/55182321/103165921-1d3c3280-4850-11eb-8812-11e0576a1f60.png)

### KEDIRI
![gambar](https://user-images.githubusercontent.com/55182321/103165930-393fd400-4850-11eb-94e4-e227d9498a9b.png)

## D. DHCP server dan DHCP relay

### Pengaturan pada DHCP server

![gambar](https://user-images.githubusercontent.com/55182321/103166115-7d7fa400-4851-11eb-88ed-0ef2a9e92c0c.png)
![gambar](https://user-images.githubusercontent.com/55182321/103165996-fb8f7b00-4850-11eb-9b4f-3150855f26c2.png)
![gambar](https://user-images.githubusercontent.com/55182321/103166084-309bcd80-4851-11eb-8418-352c1eef5076.png)
![gambar](https://user-images.githubusercontent.com/55182321/103166090-427d7080-4851-11eb-8442-b099187f33ac.png)

### Pengaturan pada DHCP relay

#### BATU
![gambar](https://user-images.githubusercontent.com/55182321/103166137-af910600-4851-11eb-83d6-afb5d0a172ba.png)

#### SURABAYA
![gambar](https://user-images.githubusercontent.com/55182321/103166179-1d3d3200-4852-11eb-9654-71cd49b027ae.png)

#### KEDIRI
![gambar](https://user-images.githubusercontent.com/55182321/103166166-f8e15580-4851-11eb-82ca-586396a675ae.png)

## 1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

Mengkonfigurasikan iptables sepeerti pada baris 1 yaitu `iptables -t nat POSTROUTING -o eth0 -j SNAT --to-source 10.151.72.82 -s 192.168.0.0/16` 
![gambar](https://user-images.githubusercontent.com/55182321/103166252-d1d75380-4852-11eb-9f45-00e0b1c62154.png)

Hasil setelah iptables dijalankan:

![gambar](https://user-images.githubusercontent.com/55182321/103166284-2f6ba000-4853-11eb-8336-e8fcdd6a6b1f.png)


## 2. Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

Mengkonfigrasikan iptables seperti pada baris 6 `iptables -A FORWARD -d 10.151.73.160/29 -p tcp --dport 22 -i eth0 -j DROP`
![gambar](https://user-images.githubusercontent.com/55182321/103166252-d1d75380-4852-11eb-9f45-00e0b1c62154.png)


## 3. Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.
### MALANG
![gambar](https://user-images.githubusercontent.com/55182321/103166747-a30fac00-4857-11eb-8b81-1367a82e0e1e.png)

### MOJOKERTO
![gambar](https://user-images.githubusercontent.com/55182321/103166772-f4b83680-4857-11eb-80bc-23a82daa7e3f.png)

## 4. Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.
![gambar](https://user-images.githubusercontent.com/55182321/103166747-a30fac00-4857-11eb-8b81-1367a82e0e1e.png)

## 5. Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya.
![gambar](https://user-images.githubusercontent.com/55182321/103166747-a30fac00-4857-11eb-8b81-1367a82e0e1e.png)

## 6. Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

Saya menyerah :)

## 7. Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.

Membuat chain LOGGING dengan `iptables -N LOGGING`

Mendata yang masuk ke chain LOGGING dengan `LOG --log-prefix "IPtables:Dropped:" --log-level 6`

Mendrop semua yang masuk ke chain LOGGING `iptables -A LOGGING -j DROP`

Merubah `-j DROP` menjadi `-j LOGGING`

### SURABAYA
![gambar](https://user-images.githubusercontent.com/55182321/103166252-d1d75380-4852-11eb-9f45-00e0b1c62154.png)

### MALANG
![gambar](https://user-images.githubusercontent.com/55182321/103166747-a30fac00-4857-11eb-8b81-1367a82e0e1e.png)

### MOJOKERTO
![gambar](https://user-images.githubusercontent.com/55182321/103166772-f4b83680-4857-11eb-80bc-23a82daa7e3f.png)
