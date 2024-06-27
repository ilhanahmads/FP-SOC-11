# Pengembangan Program Deteksi _ARP Spoofing_ Sederhana Berbasis Teknik Analisis Lalu Lintas Jaringan Secara _Real-Time_
Final Project Mata Kuliah Security Operation Center Semester 4 Tahun 2024

## Anggota Kelompok
1. Iki Adfi Nur Mohamad (5027221033)
2. Muhammad Faishal Rizqy (5027221026)
3. Ilhan Ahmad Syafa (5027221040)

## Implementasi
### *Setup* dan Instalasi GNS3
Ikuti langkah-langkah pada [Modul Jarkom - GNS3](https://github.com/lab-kcks/Modul-Jarkom/tree/master/Modul-GNS3)

### Buat Topologi Simulasi Jaringan
![Screenshot 2024-06-26 170818](https://github.com/ilhanahmads/FP-SOC-11/assets/127307991/aef06080-285b-42f0-ae09-b6bfbae063f9) <br>

### *Setup* *Router* dan *Server*
Kemudian setup konfigurasi pada *router* Erangel seperti berikut:
```
Erangel:
auto eth0
iface eth0 inet dhcp


auto eth4
iface eth4 inet static
	address 192.246.4.1
	netmask 255.255.255.0

```
Dan setup konfigurasi pada *server* seperti berikut:
```
Severny:
auto eth0
iface eth0 inet static
	address 192.246.4.2
	netmask 255.255.255.0
	gateway 192.246.4.1

Stalber:
auto eth0
iface eth0 inet static
	address 192.246.4.3
	netmask 255.255.255.0
	gateway 192.246.4.1

```

Konfig kembali *router* Erangel:
- iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.246.0.0/16
- cat /etc/resolv.conf di Erangel dan didapatkan IP DNS 192.168.122.1
- Tambahkan command iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.246.0.0/16 pada /root/.bashrc di *router* Erangel
- Tambahkan command “echo nameserver 192.168.122.1 > /etc/resolv.conf” pada /root/.bashrc tiap *server*

Pastikan setiap *server* sudah terhubung dengan internet. Kita bisa mengeceknya dengan melakukan ping ke google, apabila terdapat respons maka *server* telah terhubung dengan internet.
![image](https://github.com/ilhanahmads/FP-SOC-11/assets/127307991/e5aab0ea-c261-4e9b-9c45-74e2ecd173a2)
![image](https://github.com/ilhanahmads/FP-SOC-11/assets/127307991/2afd956a-9ec7-48da-aa33-601337928d87)

### *Setup* Program pada *Server*
Lakukan instalasi berikut pada setiap *server*:
- Install Python: `sudo apt install python3 python3-pip python3-venv -y`
- Install pip: `sudo apt install python3-pip -y`
- Install scapy: `pip3 install scapy`

Kemudian buatlah suatu *file* python yang berisi program berikut
